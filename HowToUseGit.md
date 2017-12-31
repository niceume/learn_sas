Create New Repository
---------------------

``` bash
mkdir <new_dir>
cd <new_dir>
touch README.md
git init              # Initialize local directory. Make it Git repository.
git add README.md     # Add new file to the local repository.
git commit -m 'First commit'   # Commit the changes of the following files. (In this case only README.md)

git config --global user.name '<username>'
git config --global user.email '<emailaddress>'
```


Add Remote Repository & Decide how you would like to call it.
---------------------
* Git is assuming the situation where there are local repository and remote repository.
    + Remote repository can be easily shared by a lot of people.
* When you use Github, the respository there must work as a remote repository.
    + You should prepare new repository on Github from your browser.
    + From your local machine, this repository is viewed as a remote repository.
* <repo_name_identifier> is usually "origin" by convention.
    + You can freely choose different names.
* You need to tell your local repository where its remote repository is and how it's called.
    + In the followin example, 
    + The URI of remote repository is https://github.com/githubusername/reponame.git .
    + You call it as <repo_name_identifier>

``` bash
git remote add <repo_name_identifier> https://github.com/githubusername/reponame.git
```

* When you want to see all the remote repositories that you registared and named.
    + git remote
* When you want to see the detail of a specific remote repository
    + git remote show <repo_name_identifier>


Affect Changes of local repository to the remote repository
---------------------------------------



```bash
git push -u origin master
```


Usual Updates
-------------
## Before updating you can check the status (Which one was changed).
``` bash
git status
```

## Update (existing) files (Commit to local, Push to remote)
``` bash
git commit -a -m "modify read me."
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

## Update files, add new files, delete files (DO EVERYTHING)
You will have to do git add -A to add all new files, changes and removed files. 
``` bash
git add -A
git commit -m "many changes"
git push origin master
```



Trouble Shooting
----------------

When the commit is not recognized as your commit. Do the following commands again.
```
git config --global user.name '<username>'
git config --global user.email '<emailaddress>'
```

