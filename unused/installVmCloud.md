# Install the MSBA6330 Virtual Machine on your MSBA virtual desktop

In this tutorial you will setup your MSBA virtual machine. We assume that you already know how to access your MSBA virtual desktop. If not, please
follow [this instruction](CitrixVirtualMachines.pdf) to set up the access your MSBA virtual desktop. 

<iframe width="560" height="315" src="https://www.youtube.com/embed/pD-zAcFHIKo" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe>

1. Connect to your MSBA virtual desktop

1. Open a command window. In this case, I am using `git bash`.
2. Navigate to the vagrant folder.
    ```
    cd c:/vagrant
    ```
3. Create a virtual machine box called MSBA6330 by importing the machine image file shared on a network drive. This will take a few minutes to finish.
    ```
    vagrant box add "file:////files.umn.edu/CSOM/COURSES/Courses/Summer 2019/MSBA/MSBA6330/VM/MSBA6330.box" --name MSBA6330
    ```
4. Initialize and start the virtual machine. 
    ```
    vagrant up
    ```
    This will take several minutes on the first run.
5. SSH into the VM via port 2222. When asked if you will continue to connect, answer "yes". When prompted for password, type "cloudera"
    ```
    ssh cloudera@localhost -p 2222
    ```
    or, alternatively, you can do the following from the vagrant directory. 
    ```
    vagrant ssh
    ```

Now you can work on the VM from the command line, or attach a GUI for the VM using VirtualBox (click the "play" button). Notice that you must do a "vagrant up" to properly start the VM. Directly start the VM from the VirtualBox Manager can cause some issues (e.g. lose shared folder)

Next time you need to start the VM, you just need to navigate to `c:/vagrant` directory and run a `vagrant up` from that directory. 

<a id="enable-copy-pasting-on-cloudera-vm"></a>
## Enable Copy Pasting on Cloudera VM

If you choose to use Cloudera VM's GUI. You should perform the extra step of enabling copy/pasting between cloudera VM and its host machine. 

By default, you cannot copy/paste between Cloudera VM and other machines (including your laptop or virtual desktop/windows host). You need to take two steps.

- At the GUI for cloudera VM, from Devices > Insert Guest Addition CD Image, and run the guest addition installation script.
- From Devices > Shared Clipboard , Choose `Bidirectional`. 

Below is a video demonstration of enable copy-paste on the VM:

<iframe width="560" height="315" src="https://www.youtube.com/embed/LrEBTRPIkCY" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe>

