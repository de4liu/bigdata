# Git Cheatsheet


**Note:** you may need to prefix git commands with `sudo` when you use git in the VM environment.
`sudo` give you the necessary privilege for certain git operations.

clone the repository

```shell
# accept default folder name [vagrant]
git clone https://github.umn.edu/deliu/vagrant

# use your own folder name
git clone https://github.umn.edu/deliu/vagrant myfoldername

# On VM, you need to provide your userid and add sudo, you'll be asked your U of M password
sudo git clone https://myuserid@github.umn.edu/deliu/vagrant
```

Add files to the repository

```shell
git add file1.txt
git add file2.gif
git add *.py
```

Add all (new and changed) files to the repository

```shell
git add -A
```

Review your repository status

```shell
git status
```

Commit the changes (the description is required)

```shell
git commit -m "Description of my change"
```

Push them back to the remote repository

```shell
git push
```

Get latest version - you should do this before you make your changes

```shell
git pull
```

To undo changes you have done to the files (that you have not committed). 

```shell
git checkout
```
