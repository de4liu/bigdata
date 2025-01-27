# how to Enable networking in your clouder VM

1. Open the gitbash on your windows virtual desktop, run the following:

```bash
cd /c/vagrant/
cp /z/environment  /c/vagrant/environment
ssh cloudera@localhost -p2222
```

2. After logging into your VM, run the following command
```bash
sudo cp /vagrant/environment /etc/environment
```

3. To verify that you have configure the environment correction, run command

```
cat /etc/environment

# sample output:
export http_proxy=http://proxy.oit.umn.edu:3128
export https_proxy=https://proxy.oit.umn.edu:3128
export no_proxy=.local,.umn.edu,localhost
```

4. Then you can reboot the VM for the proxy to take effect. To reboot the VM, `exit` the ssh and run:

```
vagrant reload
```