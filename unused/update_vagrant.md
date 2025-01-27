# Keep your Vagrant folder up to date

We often use vagrant folder to add new datasets and change configurations to your VM environment. It is important that you get the latest updates for your vagrant folder

1. CD to your vagrant folder in a command window

2. type `git pull` to get updates on the folder.

3. Do a `vagrant provision` if the VM is running, or do a `vagrant up` if it is powered off, to apply the update. 

### Re-apply the updates

Vagrant update script will add a flag once it applies the update, so that it won't do again. The flags are directory names such as `/tmp/delopyment_1`, `/tmp/deployment_1_hotfix`  If for some reason that you believe the updates need to be re-applied, you can remove the corresponding directories. e.g.

```shell
sudo rm -r -f /tmp/delopyment_1
```
Then follow the above steps to reapply the update.
