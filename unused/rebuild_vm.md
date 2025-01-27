# How to Reduce the Size of the Cloudera VM

The VM size will grow after a while and it does not automatially shrink even if you delete some files. One relatively simple way of shrinking its size is 
to destroy and rebuild it.

1. Back up your personal files. If you already have a copy of your personal files (pig, hive, spark codes etc) else where, then you can skip this step. Some useful commands shown below:

```bash
# view a sumary of directory size
du -sh <dirname>

# back up all script files with certain extensions to the shared vagrant folder
# note: the following script is only backing files with these extensions, you're still responsible for backing up everything that you may need.

cd ~
zip -r /vagrant/myscript.zip . -i '*.hql' '*.ipynb' '*.py' '*.pig' '*.sh' --exclude=*anaconda2*
```

Make sure you have a proper `myscript.zip` in your c:/vagrant folder in your windows virtual Desktop. 

2. Destroy the cloudera VM. In `gitbash`, at `/c/vagrant`, run:

```bash
vagrant destroy
```

3. Reinitialize the VM

```
vagrant up
```

