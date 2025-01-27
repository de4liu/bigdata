
# Edit files in Linux

This tutorial explains how you can edit files in Linux. 

## Approach 1. Using Linux command line tools.

In our environment, you can use command-line based text editors including `nano` and `vi` to edit something small. `vi` is briefly explained. `nano` is relative intuitive to learn and use.

You can type:

`nano file.txt`

to open or create a file. Edit it, and then follow the shortcut at the bottom of your screen for file operations, including

- Ctrl+O: write out (i.e. Save)
- Ctrl+X: exit Nano 

If you are using Gitbash for this, you can use your mouse to select a block of text (which is automatically copied), and right click to paste.

## Approach 2. Using Linux's Gedit tool

Open Virtualbox, click play to open a GUI on your running VM.

From the menu, go to Applications > Accessories > Gedit text editor

Then you can edit and save. Keep in mind that your user name and home directory is "cloudera". 

Gedit is versatile; it can be used edit txt files and scripts (such as python)

## Approach 3. Using Windows tools

You can take advantage of the fact that we have a shared folder between window host and the VM. This shared folder is at `c:/vagrant` on the windows side, and `/vagrant` on the Linux side. the files you put in this folder can be seen from both sides.

Use notepad++ to edit a text file or script file and save to `c:/vagrant`.

To ensure compatibility, it is best for you to save the text file with Linux line endings (by default it is windows line endings). To do that, go to Edit > EOL conversion > Unix (LF) to set it to use Linux line endings. See more [about line ending conversion here](line_endings.md).

Once you have the file in the vagrant folder, you can easily copy or move it to a new location in Linux. e.g.

`cp myscript.py ~/training_materials/analyst/myscript.py`

The same approach can also be used for moving data files between the two systems.