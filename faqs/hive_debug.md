## How to Fix Hive metastore db error

If you encounter this error:

`Unable to instantiate org.apache.hadoop.hive.ql.metadata.SessionHiveMetaStoreClient`

It could be either:

- You have other Hive instances (e.g. a Hive shell) running that locks the access to Hive metastore.
- Hive metastore is corrupt or not initialized properly.
  
Please note that because Hive's metastore database is powered by a single-user embedded database called Derby, you cannot run two Hive instances at once. Be sure to close other instances of Hive (or Hive Shell) before you run Hive.


Please take the follow steps:

1. Close all other Hive instances (hive shells) and try again. This includes closing all notebooks and terminals but the current one. The second icon (a circle with a square hole) on the left panel will show all the running notebooks and terminals with a "Shut down" button.
2. If the error persists, you can try to reinitialize Hive by running this hive setup script `~/MSBA6330/uploads/setup_hive.sh`. Note that, after this, you also need to reinstall the databases that you need (see relevant lab instructions)
3. You may restart the course server (by going to menu: file > hub control panel > stop server )



