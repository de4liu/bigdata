
## Config Git 

After you install Git, it takes a few simple steps to begin using it:

At the command line:

```shell
# configure your name
git config --global user.name "myname"
# email
git config --global user.email "myemail@umn.edu"
# when you type your password, it is cache for long time
git config --global credential.helper "cache --timeout=99999"
```