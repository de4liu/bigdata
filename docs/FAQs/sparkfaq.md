# Common Issues with PySpark

<!-- MarkdownTOC -->

- [Common Issues with PySpark](#common-issues-with-pyspark)
    - [What if the data file I try to upload through Jupyter Notebook exceeds 25 MB?](#what-if-the-data-file-i-try-to-upload-through-jupyter-notebook-exceeds-25-mb)
  - [if you encounter an error, `The root scratch dir: /tmp/hive on HDFS should be writable`](#if-you-encounter-an-error-the-root-scratch-dir-tmphive-on-hdfs-should-be-writable)
  - [Unable to instantiate org.apache.hadoop.hive.ql.metadata.SessionHiveMetaStoreClient](#unable-to-instantiate-orgapachehadoophiveqlmetadatasessionhivemetastoreclient)

<!-- /MarkdownTOC -->


<a id="what-if-the-data-file-i-try-to-upload-through-jupyter-notebook-exceeds-25-mb"></a>
### What if the data file I try to upload through Jupyter Notebook exceeds 25 MB?

Jupyter Notebook allows you to upload files no larger than 25MB. If the file exceeds the 25 MB, you can use the wget approach (if the data file has a downloadable link). 

To use `wget` from jupyter notebook, use `New > Terminal`, then you have access to the bash terminal, where you can run:

```bash
wget url_to_data_file
``` 

If the file is already your disk (but no in cloudera VM), you can upload to cloudera VM using the shared vagrant folder (e.g. your MSBA desktop's c:/vagrant is shared with Cloudera VM's /vagrant). 

1. copy your file to `c:/vagrant` on your MSBA desktop
2. open bash from Cloudera VM. Type
```
ls /vagrant/
```
to verify that your file exists in the vagrant folder on your VM. 
3. Copy or move your file to the intended directory, e.g.: 
```
cp /vagrant/file  ~/file
```
This will copy your file to your home directory. 


<a id="if-you-encounter-an-error-the-root-scratch-dir-tmphive-on-hdfs-should-be-writable"></a>
## if you encounter an error, `The root scratch dir: /tmp/hive on HDFS should be writable`

If you encounter an error, `The root scratch dir: /tmp/hive on HDFS should be writable`. Open a terminal and run the following comamnd: 

```
sudo chmod 777 /tmp/hive
```

<a id="unable-to-instantiate-orgapachehadoophiveqlmetadatasessionhivemetastoreclient"></a>
## Unable to instantiate org.apache.hadoop.hive.ql.metadata.SessionHiveMetaStoreClient

If at any point you see this error, `AnalysisException: u'java.lang.RuntimeException: java.lang.RuntimeException: Unable to instantiate org.apache.hadoop.hive.ql.metadata.SessionHiveMetaStoreClient;`. 

Please
- removing the *.lck file from hive `metastore_db`
```
# assuming you're running this from your home directory from cloudera vm
rm  metastore_db/*.lck
```
- Terminate all other running jupyter notebooks (from your jupyter home, go to Running tab, then terminate). 

If the above does not work still, try to restart the kernel (from your current jupyter notebook's menu, kernel > restart). 
