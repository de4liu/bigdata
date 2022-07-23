# Install Course VM on your Laptop

<!-- MarkdownTOC -->

- [Preface](#preface)
    - [About the VM](#about-the-vm)
    - [Vacabulary](#vacabulary)
    - [Prerequisites](#prerequisites)
- [Install and Configure VM](#install-and-configure-vm)
    - [1. Download and Install `Vagrant`, `VirtualBox`, `Git`](#1-download-and-install-vagrant-virtualbox-git)
    - [2. Clone the vagrant folder](#2-clone-the-vagrant-folder)
    - [3. Obtain the MSBA.box file](#3-obtain-the-msbabox-file)
    - [4. Provision the virtual machine](#4-provision-the-virtual-machine)
    - [5. Install Guest Additions](#5-install-guest-additions)
- [FAQs](#faqs)
    - [1. Virtualization \(VT-x\) is disabled on your host machine](#1-virtualization-vt-x-is-disabled-on-your-host-machine)

<!-- /MarkdownTOC -->

<a id="preface"></a>
## Preface

** Please note that we recommend you use the VM on our MSBA virtual desktop instead of your own laptop. Proceed only if you want to 

Vagrant is a tool we will be using for this course for easily managing the virtual machines (VMs) that we use for the homework, labs, and exams in this course. 

This tool is also used widely in industry as a way for automating VM environment configurations. More information about Vagrant can be found at [this official introduction](https://www.vagrantup.com/intro/index.html). Vagrant works with most VM providers including VirtualBox, VMware, AWS etc. We choose to work with a VirtualBox VM because its cross-platform availability and it is free of charge.

<a id="about-the-vm"></a>
### About the VM 
The VM we use is built from Cloudera's QuickStart VM with CDH (Cloudera Distributed Hadoop) 5.10.0 . It is designed to satisfy most of our course needs. The components in CDH 5.10.0 include:

- Apache Hadoop 2.6.0
- Apache Sqoop 1.4.6
- Apache Pig 0.12.0
- Apache Hive 1.1.0
- Apache Spark 1.6.0
- Apache Impala 2.7.0
- and many others

For a more comprehensive list of components included, you can read the [CDH 5.10.x packaging information](https://goo.gl/Tt6jSK).

<a id="vacabulary"></a>
### Vacabulary

- **VM**: The cloudera virtual machine that runs CentOS
- **Host**: The computer the hosts the VM - which is the MSBA virtual desktop (if you use provisioned virtual desktop) or your own laptop (if you install VM on your laptop).


<a id="prerequisites"></a>
### Prerequisites

The hardware/OS requirements are:

- **8GB RAM or above**: we recommend to allocate 4GB RAM for this VM if you can
- **20GB of available disk space or more**: the packaged box is about 8GB, and expanded box image is about 11GB
- **64-bit Operation System**: the VM does not work with 32 bit systems.

You need the following software to run the VM:

- **Vagrant**: about 280MB
- **VirtualBox**: available for both Mac and Windows platforms
- **MSBA.box**: the VM packaged in a box file, ~8 GB
- **vagrant folder**: You should clone the folder from our github repository (see below). 
- **Git**: we use git for getting `vagrant folder` updates, and use `gitbash` for SSH into the VM.

<a id="install-and-configure-vm"></a>
## Install and Configure VM

<a id="1-download-and-install-vagrant-virtualbox-git"></a>
### 1. Download and Install `Vagrant`, `VirtualBox`, `Git`

- [Vagrant](https://www.vagrantup.com/downloads.html): download it from [its homepage](https://www.vagrantup.com/downloads.html); choose your OS and follow the installation wizard.
- **VirtualBox**: Download from this [download link](http://download.virtualbox.org/virtualbox/5.1.26/VirtualBox-5.1.26-117224-Win.exe). Obtain the platform packages built for your OS and follow the installation wizard.
- [Git](https://git-scm.com/download/win): download from [here](https://git-scm.com/download/win) and follow the installation wizard. When prompted to choose your PATH environment, choose the last choice ("Run Git and included Unix tools from Windows Command prompt" - so you can use unix tools in windows command.


<a id="2-clone-the-vagrant-folder"></a>
### 2. Clone the vagrant folder

Open up a command window, and type the following. If you don't know how to open a windows command window. Here is a [short tutorial](https://canvas.umn.edu/courses/78221/pages/windows_cmd). 

```shell
# for Windows: 
git clone https://github.umn.edu/deliu/vagrant  c:/vagrant

# For linux/MacOS users: include your umn id as part of the url.
git clone https://your_umn_id@github.umn.edu/deliu/vagrant ~/vagrant
```
The above will create a `vagrant` folder at `c:\vagrant` (on Windows) or `~/vagrant` (on other OS). We will refer to this special folder as the `vagrant folder`.

The folder contains important files including `Vagrantfile` and other data or script files. 

<a id="3-obtain-the-msbabox-file"></a>
### 3. Obtain the MSBA.box file

Obtain the `MSBA.box` file (~ 8GB):

- The quickest and most reliable way of obtaining `MSBA.box` is to obtain a USB copy with the file on it (we distribute at the beginning of the course).
- If you are on the campus network, you may attempt to download it from [Google drive (8GB, campus network advised)](https://drive.google.com/open?id=1-UgmHyPhn2akh3FGF4-Zj5D6Kmvg1qK_), but it will take a long time and be prone to failure. 

Place `MSBA.box` in the vagrant folder you create in the previous step.

<a id="4-provision-the-virtual-machine"></a>
### 4. Provision the virtual machine


Open a **Git Bash** session, navigate to the *vagrant folder*,
```bash
cd c:/vagrant
```
Then execute the following command:
```shell
# replace the MSBA.box file path with your own
vagrant box add MSBA6330 c:/vagrant/MSBA.box
```

Then run the following:

```shell
# brings up the VM
vagrant up
```

This will initialize and start the VM, which may take several minutes (subsequent booting time will be shorter).

<a id="5-install-guest-additions"></a>
### 5. Install Guest Additions

Open up VirtualBox, your should see a running VM instance. Since the VM is runing, you can "Show" this VM, i.e. to connect to via GUI (graphical user interface).

The next important step is to install the `Guest Additions`, which allows you to share clipboard (copy/paste) between the host machine and the VM.

At VM's GUI, go to "Menu > Device > Insert Guest Additions CD Image". 

- Accept the default options. 
- At one point, it will prompt you for **password, enter "cloudera"**. 
- During the script, if it asks you whether you want to continue, type the answer "yes". 

After all is done, reboot the VM using  Git bash.

```shell
cd c:/vagrant
vagrant halt  #equivalent to power down
vagrant up  
```
After the VM is up, you should go to VirtualBox's GUI, "Menu > Device > Shared Clipboard ", and check **Bidirectional**. 

Test the copy/paste function between VM and the host after the VM restarts.

<a id="faqs"></a>
## FAQs

<a id="1-virtualization-vt-x-is-disabled-on-your-host-machine"></a>
### 1. Virtualization (VT-x) is disabled on your host machine 

During the VM installation, you may fail to bring up the VM with an error message like one of the following:

> VT-x is disabled in the BIOS for all CPU modes.
> Binary translation is incompatible with long mode on this platform. Long mode will be disabled in this virtual environment.
> This virtual machine is configured for 64-bit guest operating systems. However, 64-bit operation is not possible.

The likely cause is that Virtualization (VT-x) is not enable on your machine. VirtualBox will not play the VM if the virtulization (`VT-x`) is disabled on your host machine. 

To enable virtualization, you need to access the **BIOS** of your machine. Depending on the machine model, it may requires you to long press a function key such as F8, F12, and F2 during boot up. You should search for "how to access BIOS [YOUR COMPUTER MODEL]" for instructions specific to your computer model. After getting into to BIOS setup, you should look for an configuration that mentions `Intel Virtual Technology`, `VT-x`, `Virtualization` from the menu, then enable it. 

<!-- Try the VMplayer again. You may also need to do this: In VMware player, go to the Cloudera virtual machine->virtual machine settings->processors->virtualization engine->select "Intel VT-x or AMD-V". -->

<a id="2-the-shared-vagrant-folder-is-not-working"></a>

