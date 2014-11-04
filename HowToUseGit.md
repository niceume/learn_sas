Create New Repository
---------------------

``` bash
mkdir <new_dir>
cd <new_dir>
touch README.md
git init
git add README.md
git commit -m 'First commit'

git config --global user.name '<username>'
git config --global user.mail '<emailaddress>'
```


Add Remote Repository
---------------------
* When you use Github, you should prepare new repository from the browser.
``` bash
git remote add <repo_name_identifier> https://github.com/githubusername/reponame.git
```
* repo_name_identifier's name is usually origin.


Affect Changes to the remote repository
---------------------------------------
``` bash
git push -u origin master
```


Usual Updates
-------------
## Before updating you can check the status (Which one was changed).
``` bash
git status
```

## Update file (Commit to local, Push to remote)
``` bash
git commit -a -m "add read me."
git push origin master
```

## New file (Add NewFile to Tracked files, Commit to local, Push to remote)
``` bash
git add <newfile>
git commit -a -m "add new file"
git push origin master
```

If you try to add all the files, instead use as follows.
``` bash
git add .
```