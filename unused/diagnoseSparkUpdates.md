
### How to Fix the Spark Update issues

For those still have issue with PySpark update, here is what you do to diagnose and fix the issue. 

1. if pyspark does not run at all, it means that your startup script has not installed **anaconda2** on your vm. 
	1. See if there is a folder `/home/cloudera/anaconda2` on your VM. if you don't see them, you need to re-install the spark updates. You first want to make sure you have the four files we ask you to download in your vagrant folder. 
	1. then you want to make sure you have the latest vagrant file. To verify, you can open the vagrant file to see if it has lines such as `config.vm.network "forwarded_port", guest: 8888, host: 8888`. If not, you need to first pull the latest update for the vagrant folder from the git repository. After you do a `git pull`, make sure the vagrant file is up to date.
		- If you do a `git pull` do not get any update with port forwarding. Please check for your current git branch (you git bash should say **"(master)"**, if yours say **"deployment_1..."**, then you were in the wrong branch. Then you do a `git checkout master` to switch to the master branch, then do a `git pull` to fix the issue.
		- If the `git pull` produces an error, it means the folder has some changes that prevent it from getting up to date. What you should do is to do a `git clone https://github.umn.edu/deliu/vagrant.git` to create a new copy of the vagrant folder somewhere else (cannot be inside the current one), the copy everything inside the currently cloned folder (including a hidden `.git` subdirectory) into your current vagrant folder. This will override your local repository and give you the latest. After this you can delete the newly cloned vagrant folder to avoid confusion. 
	1. After you have both of the above, you can force a re-installation by doing a `sudo rm /tmp/deployment_2_spark` in the VM. then do a reboot cycle (vagrant halt then vagrant up).
1. if the pyspark runs, but enters the command line environment without providing a URL/token. It means that the VM has not been configured to use Jupyter Notebook (the default is ipython, rather than ipython notebook). Please do the step ii above and do a `vagrant provision` after that. 

### Further Notes

- Please also note how to use the URL/token [after you get it](https://github.umn.edu/deliu/msba6330/blob/master/installSparkUpdates.md#step-4-start-and-test-pyspark).
- If you still fail to bring up the Jupyter notebook in your host computer, but you it is available in your VM. you can use your VM's GUI to do things for now. 
- If none works, you should schedule a time with the professor/TA. In the mean time, you may attempt to bring up the [experimental VM](https://github.umn.edu/deliu/sparknb/blob/master/README.md) as an alternate solution (but you need to upload the needed data files). 






