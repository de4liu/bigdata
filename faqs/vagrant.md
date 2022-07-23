# Vagrant: Introduction & Quick Reference

<!-- MarkdownTOC -->

- [1. Introduction to Vagrant](#1-introduction-to-vagrant)
- [2. Essential Commands](#2-essential-commands)
- [3. More on how to use vagrant](#3-more-on-how-to-use-vagrant)

<!-- /MarkdownTOC -->

<a id="introduction-to-vagrant"></a>

<a id="1-introduction-to-vagrant"></a>
## 1. Introduction to Vagrant

A good start point for begining to understand the important role of vagrant in DevOps is this short video

<!-- https://www.youtube.com/watch?v=wlogPKBEuUM -->

Vagrant in 5 minutes

<iframe width="560" height="315" src="https://www.youtube.com/embed/wlogPKBEuUM" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe>

Additionally, this [blog article](https://medium.com/@JohnFoderaro/how-to-set-up-a-local-linux-environment-with-vagrant-163f0ba4da77) does a good job of explaining why Vagrant can make a difference. 

Some important concepts to know:

- **Box**: Instead of building a virtual machine from scratch, which would be a slow and tedious process, Vagrant uses a base image to quickly clone a virtual machine. These base images are known as "boxes" in Vagrant, and specifying the box to use for your Vagrant environment is always the first step after creating a new Vagrantfile.

- **Vagrantfile**: Every vagrant folder has special file `Vagrantfile`. The primary function of the Vagrantfile is to describe the type of machine required for a project, and how to configure and provision these machines. Vagrantfiles are called Vagrantfiles because the actual literal filename for the file is Vagrantfile (casing does not matter unless your file system is running in a strict case sensitive mode).

<a id="2-essential-commands"></a>
## 2. Essential Commands

Once you have a general working knowledge, the following commands are likely to be the primary ones that you'll want to know how to use. To run this command, we recommend you open up a Git Bash window, and type the command in our vagrant folder `c:/vagrant`

- `vagrant up` - Starts your virtual machine.
- `vagrant status` - Tells you if your virtual machine is running.
- `vagrant halt` - Shuts down your virtual machine. 
    + After your first bootstrap, a vagrant up only takes about one minute to complete.

- `vagrant ssh` - Logs you into your virtual machine over SSH and provides a terminal. Though our IT person recommends us to use `ssh cloudera@localhost p -2222`.

Sometimes, you may want to resume your VM quickly, you can try:

- `vagrant suspend` - Saves the state of your virtual machine. (Similar to putting a computer to sleep)
- `vagrant resume` - Restores a suspended virtual machine. (Similar to waking it up from sleep.) 
    + After your first vagrant up, a suspend/resume operation only takes a few seconds.


If you VM has gone bad, one of these may be useful:
- `vagrant destroy` - Destroys your virtual machine to the state of its base image. Similar to a clean reinstall of your computer. 
    + *Be careful with this command*. After you destroy a virtual machine, a vagrant up takes the full ~20 minutes to complete.  Destroying your virtual machine means that you'll need to wait through lengthy boostrap process (configurations, installing software, etc) the next time that you vagrant up.
- `vagrant box remove MSBA6330` - This one really get rid of virtual machine altogether (think of burning it to the ashes). You'd have to reinstall the VM from the base image MSBA.box. 


<a id="3-more-on-how-to-use-vagrant"></a>
## 3. More on how to use vagrant

Of course, the [Vagrant's documentation online](https://www.vagrantup.com/docs/) is where you find details about vagrant related commands. 

- This youtube play list gives you a good look at how to use vagrant to build a VM and managing it, including configuring the vagrantfile  [Vagrant Tutorial Series for Beginnners](https://www.youtube.com/playlist?list=PLXtILKFopTrP5t7YzI6poVqOHK_eSfpfD) 
- This LearnCode.Academy video on [Vagrant Tutorial - Running a VM For Your Local Development Environment](https://www.youtube.com/watch?v=PmOMc4zfCSw)  



