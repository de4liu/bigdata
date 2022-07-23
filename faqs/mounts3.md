# How to Mount s3 folder to databricks

Databricks community edition has 10G limitation on how much data you can upload to databricks DBFS. If you have a bigger files you can upload to Amazon's S3 and mount it on Databricks.

## 1. Create an Amazon S3 policy 

Go to AWS's IAM control panel > **Policy** > Create policy > Go to JSON (tab),  > paste the following policy and change `msbabigdata` (in two places) to your own bucket name. This policy gives the policy holder full access to your s3 folder.



```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "s3:ListBucket"
            ],
            "Resource": [
                "arn:aws:s3:::msbabigdata"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "s3:PutObject",
                "s3:GetObject",
                "s3:DeleteObject",
                "s3:PutObjectAcl"
            ],
            "Resource": [
                "arn:aws:s3:::msbabigdata/*"
            ]
        }
    ]
}
```

Then give the policy a name "s3databricks", and **Create policy.**

## 2. Create an IAM user and attach the policy

Go to IAM > Users > Add user 

Give a user name (e.g. `s3databricks`), check "**Programmatic access**", "Next: Permissions"

Click "**Attach existing policies directly**", search for "**s3databricks**" that you created, check this policy, then click "**Next:tags**"

Accept the defaults in the next few screens, until you "**Create user**". 

Download the credential ".csv" and save it in a safe place (you can only download it once). You will need the Access key ID, and Secret later. You can show the Secret and copy it for later use. 

## 3. Mount the s3 bucket in Databricks

In a Databricks notebook, paste the following and replace the `ACCESS_KEY`
and `SECRET_KEY` with the Access key ID and Secret you obtained in the above. 

Also replace the `AWS_BUCKET_NAME` with your bucket name. You may choose your own mount folder name.

```python
ACCESS_KEY = "xxx"
SECRET_KEY = "xxxxx"
ENCODED_SECRET_KEY = SECRET_KEY.replace("/", "%2F")
AWS_BUCKET_NAME = "msbabigdata"
MOUNT_NAME = "msbabigdata"
try:
  dbutils.fs.unmount( "/mnt/%s" % MOUNT_NAME)
except:
  pass
dbutils.fs.mount("s3a://%s:%s@%s" % (ACCESS_KEY, ENCODED_SECRET_KEY, AWS_BUCKET_NAME), "/mnt/%s" % MOUNT_NAME)

```

*Remark: in general you do not want to directly provide your scecret in your code , but the safer approach uses a Databricks API that is disabled in the community edition.*


Note that we did an unmount first in case you want to remount it.

Then you may test your mounted s3 folder using DBFS utility:

```
%fs ls /mnt/msbabigdata
```

Later you may reference folders/files in this folder using

"/mnt/msbabigdata/..."

e.g. 

`myRDD = sc.textFile("/mnt/msbabigdata/adirdata/ratings_2013.txt")`





