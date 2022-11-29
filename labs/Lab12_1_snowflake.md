# Lab 12-1 Getting Started with Snowflake 

This entry-level guide is designed for database and data warehouse administrators and architects will help you navigate the Snowflake interface and introduce you to some of our core capabilities.  


This lab is based on the analytics team at Citi Bike, a real, citywide bike sharing system in New York City, USA. The team wants to run analytics on data from their internal transactional systems to better understand their riders and how to best serve them.

We will first load structured .csv data from rider transactions into Snowflake. Later we will work with open-source, semi-structured JSON weather data to determine if there is any correlation between the number of bike rides and the weather.


This lab is largely based on [**Snowflake Quickstarts - Get Started with Snowfalek - Zero to Snowflake**](https://quickstarts.snowflake.com/guide/getting_started_with_snowflake/index.html)

What You'll Learn:
- How to create stages, databases, tables, views, and virtual warehouses.
- How to load structured and semi-structured data.
- How to perform analytical queries on data in Snowflake, including joins between tables.
- How to clone objects.
- How to undo user errors using Time Travel.
- How to create roles and users, and grant them privileges.
- How to consume datasets in the Snowflake Data Marketplace.

## 1: Get familiar with the Snowflake UI
Let's get you acquainted with Snowflake! This section covers the basic components of the user interface. 

1.1 Explore the main tabs in the left panel:

![Panel](https://quickstarts.snowflake.com/guide/getting_started_with_snowflake/img/ce13b4af78a66c6b.png)

- **Worksheets**: provides an interface for submitting SQL queries, performing DDL and DML operations, and viewing results as your queries or operations complete
- **Dashboard**: The Dashboards tab allows you to create flexible displays of one or more charts (in the form of tiles, which can be rearranged). 
- **Data**: Under Data, the **Databases** tab shows information about the databases you have created or have permission to access. You can create, clone, drop, or transfer ownership of databases, as well as load data in the UI.
- **Marketplace**: The Marketplace tab is where any Snowflake customer can browse and consume data sets made available by providers. 
- **Activity**: Under Activity there are two tabs **Query History** and Copy History.
- **Admin**: Under Admin, the Warehouses tab is where you **set up and manage virtual warehouses**. 

## 2: Preparing to Load Data

Let's start by preparing to load the structured Citi Bike rider transaction data into Snowflake.

This section walks you through the steps to:

- Create a database and table.
- Create an external **stage**.
- Create a file format for the data.

The data we will be using is bike share data provided by Citi Bike NYC. The data has been exported and pre-staged for you in an Amazon AWS S3 bucket in the US-EAST region. On AWS S3, the data represents 61.5M rows, 377 zipped csv files, 

The data has the following format:
![data format](https://quickstarts.snowflake.com/guide/getting_started_with_snowflake/img/a58c8af550412709.png)

- It is in comma-delimited format 
- with a single header line and 
- double quotes enclosing all string values.

### 2.1 Create a Virtual Warehouse

- Ensure you are using the **sysadmin** role. This is because the objects created by one role may not be accessible in another role. We will use this role mostly.
- Go to the **Admin** tab > **Warehouses**, click **"+ Warehouse"** button (top right) to create a new virtual warehouse. 
- Give the warehouse a name, `compute_wh` and set its size to **small**.
- Then create warehouse. After this, you should see a warehouse.

### 2.2 Create a database

- Navigate to the **Databases** tab. Click **Create**, name the database `CITIBIKE`, then click **CREATE**.


> note that when you create a new database, two schemas are automatically created,`PUBIC` and `INFORMATION_SCHEMA`. The former contains the actual tables whereas the latter is the meta tables of the database. 


### 2.3 Create a worksheet and configure it
- Now navigate to the **Worksheets** tab, create a new worksheet using **+ Worksheet** on the top right. 
- Modify the worksheet name to "**Citibike**" using the dropdown menu from the top left.
- Set the **role** and **warehouse** (top right) for the worksheet to **SYSADMIN** and **COMPUTE_WH** respectively.
- Set the **context** of the worksheet (on top of the worksheet) to **CITIBIKE.PUBLIC**


We will use the worksheet to run most of our commands. 

### 2.4 Create the `Trips` table.

Copy the following SQL text into your worksheet:

```sql
create or replace table trips
(tripduration integer,
starttime timestamp,
stoptime timestamp,
start_station_id integer,
start_station_name string,
start_station_latitude float,
start_station_longitude float,
end_station_id integer,
end_station_name string,
end_station_latitude float,
end_station_longitude float,
bikeid integer,
membership_type string,
usertype string,
birth_year integer,
gender integer);
```

Run the query by placing your cursor anywhere in the SQL text and clicking the blue **Play/Run** button in the top right of the worksheet. Or use the keyboard shortcut <kbd>Ctrl/Cmd+Enter</kbd>.


### 2.5 Verify the table.

- Navigate to the **Databases** tab by clicking the **HOME** icon in the upper left corner of the worksheet. 
- Then click **Data** > **Databases**. In the list of databases, click CITIBIKE > PUBLIC > TABLES to see your newly created `TRIPS` table. 

> Some times you need to refresh it (using the <kbd>...</kbd> button next to the database) to see the table

### 2.6 Create an External Stage
We are working with structured, comma-delimited data that has already been staged in a public, external S3 bucket. 

Before we can use this data, we first need to create a **Stage** that specifies the location of our external bucket.


From the **Databases** tab, click the  **CITIBIKE** database and **PUBLIC** schema. In the **Stages** tab, click the **Create** button, then **Stage** > **Amazon S3**.

In the "Create Securable Object" dialog, replace the following values in the SQL statement:

- stage_name: `citibike_trips`
- url: `s3://snowflake-workshop-lab/citibike-trips-csv/`

Note: 
- Make sure to include the final forward slash (/) at the end of the URL or you will encounter errors later when loading data from the bucket. 
- Also ensure you have removed ‘credentials = (...)' statejment which is not required because the bucket is public (but this would be needed if you have a private bucket).


### 2.7 Verify stage contents

Now let's take a look at the contents of the `citibike_trips` stage.  Navigate to the **Worksheets** tab and execute the following SQL statement:

```sql
list @citibike_trips;
```

> **list @stageName**: Returns a list of files that have been staged. for more detail of the list command, please check out the [documentation](https://docs.snowflake.com/en/sql-reference/sql/list.html)

### 2.8 Create a File Format

Before we can load the data into Snowflake, we have to create a file format that matches the data structure.

In the worksheet, run the following command to create the file format:

```sql
create or replace file format csv type='csv'
  compression = 'auto' field_delimiter = ',' record_delimiter = '\n'
  skip_header = 0 field_optionally_enclosed_by = '\042' trim_space = false
  error_on_column_count_mismatch = false escape = 'none' escape_unenclosed_field = '\134'
  date_format = 'auto' timestamp_format = 'auto' null_if = ('');
```

In the above, we specify

- the format name is "csv" (via `format csv`)
- the file may be compressed (via `compression = 'auto' `)
- the file is comma separated (via `field_delimiter = ','`)
- the file has no header (via skip_header =0)
- files are quoted using *: via `field_optionally_enclosed_by = '\042'`)

for more detailed options of this `create file format` command, please visit [documentation page](https://docs.snowflake.com/en/sql-reference/sql/create-file-format.html)

The file format type could be  CSV, JSON, AVRO, ORC, PARQUET, XML.

### 2.9 Verify the File Format

Verify that the file format has been created with the correct settings by executing the following command:

```sql
show file formats in database citibike;
```

## 3 Loading Data


### 3.1 Load stage files

Execute the following statements in the worksheet to load the staged data into the table. This may take 20-30 seconds.

```sql
copy into trips from @citibike_trips file_format=csv PATTERN = '.*csv.*' ;
```

> This utilizes the stage and file format we have created in the previous step.

> We also limit the stage files to ones containing "csv" (via RegEx pattern `PATTERN = '.*csv.*'` )

### 3.2 Check Query History

- Next, navigate to the **Query History** tab by clicking the **Home** icon and then **Activity** > **Query History**. 
- Select the query at the top of the list, which should be the COPY INTO statement that was last executed. 
- Select the **Query Profile** tab and note the steps taken by the query to execute, query details, most expensive nodes, and additional statistics.

### 3.3 Preparation for Reloading the data with a large virtual warehouse

Now let's reload the `TRIPS` table with a larger warehouse to see the impact the additional compute resources have on the loading time.

First, go back to the worksheet and use the TRUNCATE TABLE command to clear the table of all data and metadata:

```sql
truncate table trips;
```

Then, Verify that the table is empty by running the following command:
```sql
select * from trips limit 10;
```

The result should show "Query produced no results".

Change the warehouse size to large using the following ALTER WAREHOUSE (This is equivalent to changing the warehouse through UI):

```sql
alter warehouse compute_wh set warehouse_size='large';
```

Verify the change using the following SHOW WAREHOUSES:

```sql
show warehouses;
```

### 3.4 Reload the data with a large virtual warehouse

Execute the same COPY INTO statement as before to load the same data again:

```sql
copy into trips from @citibike_trips file_format=CSV PATTERN = '.*csv.*' ; 
```



### 3.5 Verify the speed of loading

- Once the load is done, navigate back to the **Queries** page (**Home** icon > **Activity** > **Query History**). 
- Compare the times of the two COPY INTO commands. The load using the Large warehouse was significantly faster.


## 4: Working with Queries and Cloning

### 4.1 Create a New Warehouse for Data Analytics

Navigate to the **Admin** > **Warehouses** tab, click **+ Warehouse**, and name the new warehouse **ANALYTICS_WH** and set the size to **Large**.

If you are using Snowflake Enterprise Edition (or higher) and Multi-cluster Warehouses is enabled, you will see additional settings:

- Make sure **Max Clusters** is set to 1.
- Leave all the other settings at their defaults. It is most important that you keep the **auto-suspend** and **auto-resume** turned on. 

### 4.2 Run some Queries

Your worksheet context should be the following:

Role: `SYSADMIN` Warehouse: `ANALYTICS_WH` (L) Database: `CITIBIKE` Schema = `PUBLIC`

First run,

```sql
select * from trips limit 20;
```

### 4.3 Hourly statistics on Citi Bike usage

Then, execute the following to obtain hourly statistics on citi bike usage. 

```sql
select date_trunc('hour', starttime) as "date", count(*) as "num trips",
avg(tripduration)/60 as "avg duration (mins)", avg(haversine(start_station_latitude, start_station_longitude, end_station_latitude, end_station_longitude)) as "avg distance (km)"
from trips
group by 1 order by 1;
```

In the above: 

- `date_trunc` [is a function](https://docs.snowflake.com/en/sql-reference/functions/date_trunc.html) that truncates a DATE, TIME, or TIMESTAMP to the specified precision. e.g. `date_truncate('DAY','2015-05-08 23:39:20.123')` would produce ` 2015-05-08`
- `HAVERSINE( lat1, lon1, lat2, lon2 )`: Calculates the great circle distance in kilometers between two points on the Earth’s surface, using the Haversine formula. The two points are specified by their latitude and longitude in degrees.
- Group by 1 and order by 1: these "1"s refer to the first field in the select statement, i.e. 'date'. 

### 4.4 Using the Result Cache

Snowflake has a result cache that holds the results of every query executed in the past 24 hours. These are available across warehouses, so query results returned to one user are available to any other user on the system who executes the same query, provided the underlying data has not changed. 

Let's see the result cache in action by running the exact same query again.

```sql
select date_trunc('hour', starttime) as "date", count(*) as "num trips",
avg(tripduration)/60 as "avg duration (mins)", avg(haversine(start_station_latitude, start_station_longitude, end_station_latitude, end_station_longitude)) as "avg distance (km)"
from trips
group by 1 order by 1;
```

In the Query Details pane on the right, note that the second query runs significantly faster because the results have been cached.

### 4.5 Which months are the busiest

Next, let's run the following query to see which months are the busiest:

```sql
select
monthname(starttime) as "month",
count(*) as "num trips"
from trips
group by 1 order by 2 desc;
```

### 4.6 Zero-copy cloning

Snowflake allows you to create clones, also known as "zero-copy clones" of tables, schemas, and databases in seconds. When a clone is created, Snowflake takes a snapshot of data present in the source object and makes it available to the cloned object. The cloned object is writable and independent of the clone source. Therefore, changes made to either the source object or the clone object are not included in the other.

A popular use case for zero-copy cloning is to clone a production environment for use by Development & Testing teams to test and experiment without adversely impacting the production environment and eliminating the need to set up and manage two separate environments.

A table clone statement looks like this:

```sql
create table Y clone X;
```

Write the following command in the worksheet to create a development (dev) table clone `trips_dev` of the `trips` table.

Then, click the three dots (...) in the left pane and select **Refresh**. Expand the object tree under the `CITIBIKE` database and verify that you see a new table named `trips_dev`. 

Your Development team now can do whatever they want with this table, including updating or deleting it, without impacting the trips table or any other object.


```sql
create table trips_dev clone trips
```


## 5: Working with Semi-Structured Data, Views, & Joins

This section requires loading additional data and, therefore, provides a review of data loading while also introducing loading semi-structured data.

In this section, we will:

- Load weather data in semi-structured JSON format held in a public S3 bucket.
- Create a view and query the JSON data using SQL dot notation.
- Run a query that joins the JSON data to the previously loaded TRIPS data.
- Analyze the weather and ride count data to determine their relationship.

The JSON data consists of weather information provided by MeteoStat detailing the historical conditions of New York City from 2016-07-05 to 2019-06-25. It is also staged on AWS S3 where the data consists of 75k rows, 36 objects, and 1.1MB compressed. If viewed in a text editor, the raw JSON in the GZ files looks like:

![json](https://quickstarts.snowflake.com/guide/getting_started_with_snowflake/img/c025f1200b524e26.png)


In other words, this json file is a list of json documents. Our strategy is to create a table with one column of **variant**, then create a view based on this table using the data extracted from each json document (i.e. a row of the table).


### 5.1 Create a New Database and Table for the Data

First, in the worksheet, let's create a database named `WEATHER` to use for storing the semi-structured JSON data.



```sql
create database weather;
```

### 5.2 Set up the worksheet context using SQL commands

Execute the following USE commands to set the worksheet context appropriately:

- role:sysadmin
- warehouse:compute_wh
- database:weather
- schema:public



```sql
-- set the context
use role sysadmin;
use warehouse compute_wh;
use database weather;
use schema public;
```

### 5.3 Create a Table for Loading the Json data

Next, let's create a table named `JSON_WEATHER_DATA` to use for loading the JSON data. 
- The table should have a single column `v` of variant type.

> Note that Snowflake has a special column data type called VARIANT that allows storing the entire JSON object as a single row and eventually query the object directly.

```sql
create table json_weather_data (v variant);
```

### 5.4 Create Another External Stage

In the worksheet, create a stage `nyc_weather` that points to the bucket where the semi-structured JSON data is stored on AWS S3: `s3://snowflake-workshop-lab/zero-weather-nyc`

Then,  View the files in the stage area using `list @stagename`.


```sql

-- create stage
create stage nyc_weather
url = 's3://snowflake-workshop-lab/zero-weather-nyc';

-- View the files in the stage area

list @nyc_weather;
```


### 5.5 Load and Verify Json data

In the following, we will use copy into command to load json files into our table. 

```sql
COPY INTO <table> FROM @stage file_format = <file_format>;
```

Here we will can specify a **FILE FORMAT object inline** in the command. In the previous section where we loaded structured data in CSV format, we had to define a file format to support the CSV structure. Because the JSON data here is well-formed, we are able to simply specify the JSON type and use all the default settings and set these options:

- type: json
- strip_outer_array: true

After loading, select 10 rows to verify the loading result.


```sql
-- load json:
copy into json_weather_data
from @ nyc_weather
file_format = (type=json strip_outer_array=true)

-- verify
select * from json_weather_data limit 10;
```

### 5.6 Create a View and Query Semi-Structured Data

Next, let's look at how Snowflake allows us to create a view and also query the JSON data directly using SQL.

> A **view** allows the result of a query to be accessed as if it were a table.

> Snowflake also supports **materialized views** in which the query results are stored as though the results are a table. This allows faster access, but requires storage space. Materialized views can be created and queried if you are using Snowflake Enterprise Edition (or higher).


```sql
-- create a view that will put structure onto the semi-structured data
create or replace view json_weather_data_view as
select
    v:obsTime::timestamp as observation_time,
    v:station::string as station_id,
    v:name::string as city_name,
    v:country::string as country,
    v:latitude::float as city_lat,
    v:longitude::float as city_lon,
    v:weatherCondition::string as weather_conditions,
    v:coco::int as weather_conditions_code,
    v:temp::float as temp,
    v:prcp::float as rain,
    v:tsun::float as tsun,
    v:wdir::float as wind_dir,
    v:wspd::float as wind_speed,
    v:dwpt::float as dew_point,
    v:rhum::float as relative_humidity,
    v:pres::float as pressure
from
    json_weather_data
where
    station_id = '72502';

```


In the above, SQL dot notation `v:temp` is used in this command to pull out values at lower levels within the JSON object hierarchy. 

`x::float` is a cast operator - used to cast x to the desired type. It is the same as `cast(x as float)`


### 5.7 Verify the view with the following query

```sql
select * from json_weather_data_view
where date_trunc('month', observation_time) = '2018-01-01'
limit 20
```


### 5.8 Use a Join Operation to Connect two Data Sets

We will now join the `JSON_weather_data_view` to our `CITIBIKE.PUBLIC.TRIPS` data to answer our original question of how weather impacts the number of rides.

Run the query below to join `WEATHER` to `TRIPS` and count the number of trips associated with certain weather conditions:

> Because we are still in the worksheet, the `WEATHER` database is still in use. You must, therefore, fully qualify the reference to the `TRIPS` table by providing its database and schema name.

```sql
select weather_conditions as conditions,count(*) as num_trips
from citibike.public.trips left outer join json_weather_data_view
on date_trunc('hour', observation_time) = date_trunc('hour', starttime)
where conditions is not null
group by 1 order by 2 desc;
```

## 6: Using Time Travel

Snowflake's powerful Time Travel feature enables accessing historical data, as well as the objects storing the data, at any point within a period of time. 

Some useful applications of Time Travel include:

- Restoring data-related objects such as tables, schemas, and databases that may have been deleted.
- Duplicating and backing up data from key points in the past.
- Analyzing data usage and manipulation over specified periods of time. 

### 6.1  Drop and Undrop a Table
First let's see how we can restore data objects that have been accidentally or intentionally deleted.

In the CITIBIKE worksheet, run the following DROP command to remove the JSON_WEATHER_DATA table:

```
drop table json_weather_data;
```

Then, verify that the table has been dropped:

```
select * from json_weather_data limit 10;
```

Now, restore the table using the `undrop` command:

```sql
undrop table json_weather_data;
```

Verify by running the following query:

```sql
select * from json_weather_data limit 10;
```

### 6.2 Roll Back Accidental Changes to Table 


In the following example, we use Time Travel to roll back an unintentional DML error that replaces all the station names in the table with the word "oops".

First, run the following SQL statements to switch your worksheet to the proper context:


```sql
use role sysadmin;
use warehouse compute_wh;
use database citibike;
use schema public;
```

Next, simulate an error with the following DML statement:
```sql
update trips set start_station_name = 'oops';
```

This will result in all start_station_name changed to `oops`. You can verify it with the following query:

```sql
select start_station_name as "station", count(*) as "rides"
from trips 
group by 1 order by 2 desc limit 20;
```

Normally we would need to scramble and hope we have a backup lying around. 

In Snowflake, we can simply find the query ID of the last UPDATE command and use it to restore the table to just before the query. 


### 6.3 obtain the query ID from the query history.

First run the following query to obtain the query id of update query (searching among the last five queries with query text containing 'update').
- [query_history_by_session](https://docs.snowflake.com/en/sql-reference/functions/query_history.html)(result_limit=>5): returns 5 historical queries meeting the critera
- query_text like 'update%' order by start_time desc  : most recent `update` queries. 

```sql
select query_id from table(information_schema.query_history_by_session (result_limit=>5))
where query_text like 'update%' order by start_time desc limit 1
```

Save query_id to a SQL variable `query_id:

```sql
set query_id =
(select query_id from table(information_schema.query_history_by_session (result_limit=>5))
where query_text like 'update%' order by start_time desc limit 1);
```



### 6.4 Restore the table using Time Travel

Then, use `BEFORE(statement=<query_id>)` to obtain a historical version of the table. In the following, we use that version of the table to recreate the table trips.

```sql
create or replace table trips as
(select * from trips before (statement => $query_id));
```

Run the previous query again to verify that the station names have been restored:
```sql
select start_station_name as "station", count(*) as "rides"
from trips 
group by 1 order by 2 desc limit 20;
```

## 7: Miscellaneous 

## 7.1 View Account Usage


Switch to the role of the user to AccountAdmin

```sql
use role accountadmin;
```

Then, go to **Admin** > **Usage**, you will see a usage report including (selected via one of the drop down menu):

- Credits: credits consumed by the virtual warehouses in the current account.
- Storage: Average amount of data stored in all databases, internal stages, and Snowflake Failsafe in the current account for the past month.
- Transfers: Average amount of data transferred out of the region (for the current account) into other regions for the past month.


## 7.2 Snowflake Data Marketplace

You can use Data Marketplace to find public datasets and use them for your analysis:

Make sure you're using the ACCOUNTADMIN role and, navigate to the Marketplace:

Type **COVID** in the search box, scroll through the results, and select **COVID-19 Epidemiological Data** (provided by **Starschema**).

When you're ready, click the **Get** button to make this information available within your Snowflake account.


## 7.3 Sharing data

Snowflake enables data access between accounts through the secure data sharing features. Shares are created by data providers and imported by data consumers, either through their own Snowflake account or a provisioned Snowflake Reader account. The consumer can be an external entity or a different internal business unit that is required to have its own unique Snowflake account.

With secure data sharing:

- There is only one copy of the data that lives in the data provider's account.
- Shared data is always live, real-time, and immediately available to consumers.
- Providers can establish revocable, fine-grained access to shares.

Due to a limitation of the trial account, we will not practice sharing our database. 
