# Using the Cloudera Virtual Machine (VM)

<!-- MarkdownTOC -->

- [Vocabulary](#vocabulary)
- [1. Start Up](#1-start-up)
- [2. Power Off the VM](#2-power-off-the-vm)
- [3. Destroy the VM](#3-destroy-the-vm)
- [4. Remove the Box](#4-remove-the-box)
- [FAQS](#faqs)
    - [1. The shared vagrant folder is not working](#1-the-shared-vagrant-folder-is-not-working)
    - [2. `vagrant ssh` is not working](#2-vagrant-ssh-is-not-working)
    - [3. Hive is extremely slow](#3-hive-is-extremely-slow)
    - [4. How to improve the performance of the VM \(Experimental\)](#4-how-to-improve-the-performance-of-the-vm-experimental)

<!-- /MarkdownTOC -->


<a id="vocabulary"></a>
## Vocabulary

- **VM**: The Cloudera virtual machine that runs CentOS
- **Host**: The computer the hosts the VM - which is the MSBA virtual desktop or your own laptop (if you install VM on your laptop).

<a id="1-start-up"></a>
## 1. Start Up

Open a **Git Bash** session, navigate to the *vagrant folder*,
```bash
cd c:/vagrant
```

Then execute the following to bring up the VM (if it is not already up)

```shell
# brings up the VM
vagrant up
```

This will start the VM, which may take a minute or two. After that you can access the VM in one of the following ways:

- **SSH into the VM**
    - In the Git Bash window,
    ```shell
    ssh cloudera@localhost -p 2222
    ```
    At the password prompt, type `cloudera`. 
- **Use VirtualBox to play the VM**
    
    - Open VirtualBox, play the machine that you've started to access its GUI interface.
- **Using Forwarded Web Ports**
    Open up a browser on your host machine, type the URL
    
    - [http://localhost:8888](http://localhost:8888): To access Hue (include Hive, HDFS file manager, Pig etc). Use "cloudera" for both user name and password.

<a id="2-power-off-the-vm"></a>
## 2. Power Off the VM
In the Git Bash window,

```shell
# power down the VM
vagrant halt
```

**In case you are unable to run the `vagrant halt`** (e.g. if it says the VM is being used by another process), you may try one of the two approaches. 

- **Kill the running vagrant process**: Run `Task Manager` from your Windows virtual desktop's start menu, find and kill the `ruby` process from there. Then try to run `vagrant up` again.
- **Restart your windows virtual desktop**: From Citrix Receiver, go to your desktops, click `details` near your virtual desktop, then click "Restart". This will do a restart of your windows Virtual Desktop.  

<a id="3-destroy-the-vm"></a>
## 3. Destroy the VM

If your VM has gone bad, you can restore to its pristine state by destroying it. This command stops the running machine and destroys all resources that were created during the machine creation process. Note that all the custom software and data that you added will be removed.

In the Git bash environment, at the vagrant folder, type:
```bash
vagrant destroy
```

The `destroy` command does not remove a box that has been installed on your computer during `vagrant up`. You will need `vagrant box remove` to completely remove the box file. 

<a id="4-remove-the-box"></a>
## 4. Remove the Box

By removing the box, you remove the image file associated with the VM. After this you need to repeat the "vagrant box add" step as in the initial provision. This is useful if you receive a new box file or want to remove/move the VM completely from your computer.

From the vagrant folder:
```bash
# remove the image file for the machine. 
vagrant box remove MSBA6330
```

<a id="faqs"></a>
## FAQS

<a id="1-the-shared-vagrant-folder-is-not-working"></a>
### 1. The shared vagrant folder is not working

The most common causes of this problem is that:

- Instead of starting the VM using `vagrant up` from command line, you started it from `VirtualBox`, which bypasses the necessary setup steps including creating a shared vagrant folder. To fix this, issue the following commands from your vagrant folder.

```
vagrant halt
vagrant up
```
Please remember you should start up and shut down the VM using commands `vagrant up` and `vagrant halt`.

Additionally, if the above does not solve your problem, it is likely that port forwarding is not working properly. See next.

<a id="2-vagrant-ssh-is-not-working"></a>
### 2. `vagrant ssh` is not working

Vagrant ssh and shared folder need port forwarding (from host port 22 to guest port 2222) to work. When port forwarding fails, you cannot do `vagrant ssh` or see shared folder. When this is the case, you should see error messages like the following when you do `vagrant up`: 

> Vagrant cannot forward the specified ports on this VM

The culprit of this could be the antivirus program on your machine which locks down certain ports on your computer. To fix that, you should tell your antivirus program to allow port 22 (for details, you make want to visit our TA/instructor).

<a id="3-hive-is-extremely-slow"></a>
### 3. Hive is extremely slow 

If your hive is extremely slow, it could be because hive's connection to zookeeper times out. To find this out, execute the following on your VM:

1. `cd /usr/lib/zookeeper/bin/`
2. Then run: `./zkServer.sh status`
3. If you see the following:
```
JMX enabled by default
Using config: /usr/lib/zookeeper/bin/../conf/zoo.cfg
Error contacting service. It is probably not running.
```
this means Zookeeper is stopped, then what you need to do is run: `sudo ./zkServer.sh start`. We don't yet know what causes zookeeper to stop. If this is recurring, it may be a good idea to bring your machine to see TA/instructor.

<a id="4-how-to-improve-the-performance-of-the-vm-experimental"></a>
### 4. How to improve the performance of the VM (Experimental)

Here are a few hacks that may help improve the performance of your VM. This has not proven to work. 

**Video configuration**

- **Video Memory** should be maximum value
- Enable **3D Acceleration** should be checked.

![Adjust Video Memory](http://idsdl.csom.umn.edu/c/bigdata18/img/video.png)

**Paravirtualization**

Notably, if the system is still slow, windows users could try manually setting the **Paravirtualization interface** to *Hyper-V*.

![Adjust Paravirtualization](http://idsdl.csom.umn.edu/c/bigdata18/img/virtualization.png)

<a id="further-readings-on-vagrant-and-git"></a>

