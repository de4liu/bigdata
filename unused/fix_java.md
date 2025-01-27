# How to fix the Java issue causing sqoop import to fail.

This is meant to be a one-time fix for the error "It seems as though you are running sqoop with a JRE" associated with Lab 1. The changes will be incorporated into the future version of the VM (2019-06-12)

1. On Windows virtual desktop (using git bash): `cd c:/vagrant`
2. `git pull`
3. Should prompt you for your username and password, this is simply your X500 and associated password. This will pull updated vagrant files.
4. Once the repo is up to date, you should (a) run a `vagrant up --provision`, if your VM is not running, or (b) `vagrant provision`, if the VM is already up. This will take several minutes.
5. Once it's done, you can proceed with your `sqoop import`. 