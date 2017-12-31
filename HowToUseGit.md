Great Guide for git
------------------

* http://rogerdudler.github.io/git-guide/
* https://www.atlassian.com/git/tutorials/syncing


New Local and/or remote repositories and how local and remote repositories get associated.
------------------------------------------------------------------------------------------

Create New Local Repository
===========================

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


Create New Remote Repository on Github
=======================================

* Log in to your Github.
* From Repositories, push "New" !!


Another way to create a new local repositoty: Clone others' remote repository
=========================================

* In this case, newly created local git repository automatically knows where the remote repository is.
    + origin remote repository is the repository from which the current local repository is cloned.

``` bash 
git clone username@host:/path/to/repository
```

Associate local repository with remote repository
=================================================

### Add Remote Repository & Decide how you would like to call it.

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


Merge Commits
---------------------------

When merging commits happen?
============================

1. Between local repository and remote repository
    + From local to remote:   git push <remote_repository_identifier> <branch_name>
    + From remote to local:   git pull
        + You can fetch and merge remote changes.
        + Then, from which repository are Git going to pull commits in this case?
        + 
2. Between branches
    + git merge <branch>
        + Merge commits of the branch into the active branch (you are using).
        + The commits between branched timepoint and current timepoint need to be merged.


Affect Changes of local repository to the remote repository
============================================

* git push takes two arguments
    1. remote <repo_name_identifier>
    2. branch name
    3. u option here 
        + Without u option, next time you do "git pull", git will not know from which branch to pull commits and merge them to your local repository.
        + Meaning u option tells git that this is the branch you are working on!
        + (ref.) https://stackoverflow.com/questions/5697750/what-exactly-does-the-u-do-git-push-u-origin-master-vs-git-push-origin-ma

```bash
git push -u origin master
```



Usual Updates
=============

### Before updating you can check the status (Which one was changed).
``` bash
git status
```

### Update (existing) files (Commit to local, Push to remote)
``` bash
git commit -a -m "modify read me."
git push origin master
```

### New file (Add NewFile to Tracked files, Commit to local, Push to remote)
``` bash
git add <newfile>
git commit -a -m "add new file"
git push origin master
```

If you try to add all the files, instead use as follows.
``` bash
git add .
```

### Update files, add new files, delete files (DO EVERYTHING)
You will have to do git add -A to add all new files, changes and removed files. 
``` bash
git add -A
git commit -m "many changes"
git push origin master
```



Manage Branches
------------------

Conventions about branch names
=============================

* the main branch for release is usually called "master"
* the main branch for development is usually called "devel"
* (ref.) Brian's answer in https://stackoverflow.com/questions/273695/git-branch-naming-best-practices


How to create, switch and delete a new branch
==========================

* create a new branch named "feature_x" and switch to it using

```
git checkout -b <branch_name>
```

* Switch to another branch

```
git checkout <branch_name>
```

* Delete a branch

```
git branch -d <branch_name>
```


Trouble Shooting
----------------

When the commit is not recognized as your commit. Do the following commands again.
```
git config --global user.name '<username>'
git config --global user.email '<emailaddress>'
```

