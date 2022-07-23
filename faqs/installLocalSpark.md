# Install Spark Locally on your Own Computer



We mainly used a virtual machine for Machine Learning/Data Science tasks on Github to setup a local Spark environment.


Below are the steps

1. First visit [the git repository for the Spark Virtual Machine (VM)](https://github.com/paulovn/ml-vm-notebook) and take a look what this virtual machine offers.
2. Set up the Spark directory on your computer by either using git clone (if you have git installed on your machine), or by downloading a copy of the above git repository. To avoid trouble, we advise you use a directory name without spaces. I recommend use `c:/spark` for windows machines.
3. Install the latest version of vagrant at  [https://www.vagrantup.com/downloads](https://www.vagrantup.com/downloads).
4. Install the VirtualBox 6.0 version at [https://www.virtualbox.org/wiki/Download_Old_Builds_6_0](https://www.virtualbox.org/wiki/Download_Old_Builds_6_0). Please note that the newest version of VirtualBox may not be supported by vagrant yet. 
5. Provision the virtual machine from a command window by CD the directory you have the Spark vm files first (e.g. `c:/spark`), then execute the following command:
```shell
# brings up the VM
vagrant up
```

This will initialize and start the VM, which will take several minutes (subsequent booting time will be shorter). It downloads a base VM from the Internet and then augments it with the script specified in  the `Vagrantfile` in the Spark folder.

6. If the provision succeeded, you can bring up a PySpark notebook by entering localhost:8008 in a browser window. Enter **"vmuser"** for both **user name** and **password**. 

You may upload a new notebook using the upload button from the jupyter notebook. The uploaded notebook will be located at  `c:\spark\vmfiles\IPNB\` (assuming your spark folder is `c:/spark`) on your computer. 

Please note that when you open the notebook, you will most likely need to **manually specify the kernel** to be **"PySpark (py3)"** to use Spark functionalities. 


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