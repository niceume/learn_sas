Great Guide for git
=======================

* http://rogerdudler.github.io/git-guide/
* https://www.atlassian.com/git/tutorials/syncing


Overall concept of git
==========================

1. Working place (=workplace or working tree)
    + "git checkout" switches to the specified branch.
2. Index (List to detect changes in these files. Holds associations between workplace file and objects in repository.)
    + "git add" updates this part.
3. Local Repository (Repository holds objects and tags.)
    + "git commit" updates this part from staged files.
    + "git merge" combines the specified branch with the active branch.
4. Locally managed remote repository
    + "git fetch" updates this part by synchronizing with remote repository.
    + Git does not look at remote repository itself every time. Instead, it uses this locally managed remote repository.
5. Remote Repositry
    + "git push" updates this part.


New Local and/or remote repositories and how local and remote repositories are managed locally.
=============================================================================================

Create New Local Repository (init)
-----------------------------------

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
--------------------------------------

* Log in to your Github.
* From Repositories, push "New" !!


Another way to create a new local repositoty: Clone others' remote repository (clone)
--------------------------------------

* In this case, newly created local git repository automatically knows where the remote repository is.
    + origin remote repository is the repository from which the current local repository is cloned.

``` bash 
git clone username@host:/path/to/repository
```

Add Remote Repository Infoation to local & Decide how you would like to call it. (remote add)
---------------------------------------

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
* 
    + git fetch update the locally managed remote repository information.

``` bash
git remote add <repo_name_identifier> https://github.com/githubusername/reponame.git
```

* When you want to see all the remote repositories that you registared and named.
    + git remote
* When you want to see the detail of a specific remote repository
    + git remote show <repo_name_identifier>



Manage Local Branches 
============================

Conventions about branch names
----------------------------

* the main branch for release is usually called "master"
* the main branch for development is usually called "devel"
* (ref.) Brian's answer in https://stackoverflow.com/questions/273695/git-branch-naming-best-practices


How to create a branch and switch to it (checkout -b)
-----------------------------

* create a new branch named "feature_x" and switch to it using

```
git checkout -b <branch_name>
```

How to switch to another branch (checkout)
----------------------------

* If you have another branch in your local repository.
    * Using checkout, you can switch to another branch.

```
git checkout <branch_name>
```

How to delete a branch (branch -d)
----------------------------

* Delete a branch

```
git branch -d <branch_name>
```

List all the local/remote branches (branch)
------------------------------

```
git branch  # shows all the local branches
git branch -r  # shows all the remote branches
```


How to associate local and remote branches, called tracking (branch --track)
=================================

* By associating local and remote branches
    + git push / git pull do not need to require remote branch names.

```
git branch --track <local_branch_name> <remote_repo>/<remote_branch_name>
git branch --track branch-name origin/branch-name
```

* In some other cases, this associations are established automatically. 
    1. Local branch is created based on the remote branch. 
        + git checkout -b 
    2. Local repository is created based on the remote repository
        + git clone 
    3. Push commits to a remote branch with -u option.
        + git push -u origin hogehoge


Merge Commits
==============================

When does merging commits happens?
-------------------------------------

1. Between local repository and remote repository
    + 1a:  From local to remote:   git push <remote_repository_identifier> <branch_name>
    + 1b:  From remote to local:   git pull
        + You can fetch and merge remote changes.
        + Then, from which repository are Git going to pull commits in this case? Answer: The associated remote branch.
2. Between branches
    +  git merge <from_branch>
        + Merge commits of the branch into the active branch (you are using).
        + The commits between branched timepoint and current timepoint need to be merged.


Case 1a:  Merge commits of local repository and the remote repository
---------------------------------------

* git push -u <repo_name_idenntifier> <branch_name>
    1. u option here 
        + Without u option, next time you do "git pull", git will not know from which branch to pull commits and merge them to your local repository.
        + Meaning u option tells git that this is the branch you are working on!
        + (ref.) https://stackoverflow.com/questions/5697750/what-exactly-does-the-u-do-git-push-u-origin-master-vs-git-push-origin-ma
    2. remote <repo_name_identifier>
    3. branch name

```bash
git push -u origin master
```

Case 1b:  Merge commmits of remote repository to the local repository
---------------------------------------

* (ref.) https://stackoverflow.com/questions/292357/what-is-the-difference-between-git-pull-and-git-fetch
* git pull merges the commits that have been done for remote branch to local branch.
    * In short, "git pull" is equal to "git fetch" followed by "git merge"

```bash
git checkout master  # switch to local master branch
git pull
## or
git checkout master  # switch to local master branch
git fetch 
git merge original/master
```

Case 2: Merge commits 
---------------------------------------

* Merge is usually used between local and remote branches.
    + For example, commits that have been done for origin/master is merged to master (at local). 

```bash
# You are using local master branch.
git merge origin/master
````

* Of course, merge can work for local branches

```bash
git merge iss53
# (ref.) https://git-scm.com/book/en/v2/Git-Branching-Basic-Branching-and-Merging
```


Obtaining remote information without merging
---------------------------------------

* "git fetch" download objects and tags to the local. (But not merged with local repository yet.)

```
git fetch
```


How to control index and staged files
========================================

## About 
    1. Tell git to monitor files. (index)
    2. Git monitor the files and detect their changes. Those files are staged. (staged files)
        + When files under Git monitoring get some changes, they are put on stage.
    3. Staged files can be committed.


## Add new files for Git to monitor changes.

```
git add <filename>  # Add specific file
# or
git add .  # Adds all the files in the working tree.
```

## Remove files to be monitored by Git. Remove all the history of this file.

```
git rm --cached <path_to_filename>
# or git rm --cached -r <dir> # works recursively on a folder and all files in it

# Edit .gitignoer
# And add this <path_to_filename> or <path_to_folder/> to your .gitignore file.
```

## Tell git new files, updated files and delted files. 

```
git add -A 
# will add all changes, including deletions, to the index.
# (ref.) https://stackoverflow.com/questions/6262192/how-to-handle-a-bunch-of-deleted-files-under-git
```


## Stop monitoring changes from now on. 

1. Later maybe you will monitor again
2. or other people want to commit changes.)

```
git update-index --assume-unchanged <path_to_filename>
# Bring the file to be monitored.
git update-index --no-assume-unchanged <path_to_filename>
```

* (ref.) https://stackoverflow.com/questions/936249/how-to-stop-tracking-and-ignore-changes-to-a-file-in-git


Think about pull request
================================

* When you receive a pull request or send a pull request, there are some things you need to think about.
    1. Maybe your working branch is out-of-date.
        + Because people work on a branch which may be branched long long time ago.
        + Even if you want to merge, it may fail.
        + Before sending pull request, it's better to solve this problem. You need to confirm that the changes (commits) can work for the current (HEAD) main branch.
    2. Maybe you are including too many commits in the pull request.
        + Development may be done based on a lot of commits and a lot of trial and errors. Only one commit to solve a problem is rare.
        + For other people, maybe your trial and erros are not important. 
        + Before seinding a pull reques, it's better to make those commits tidy. Combine them into some pieces, which help reviewers read your commits rapidly.
    
* For each concern, 
    1. When the branch was branched long time ago, "rebase" is required. 
    2. When the number of commits is a lot, "squash" is required. As to be mentioned later, "git rebase -i " also does this work.
    
* In conclusion,
    + Do "git rebase -i" before pull request.


Squash
-------------------

＊ Several Commits can be combined into one.
    + This is called "squash".
* The following command squash all the commits done for the current branch. 
    + People say if you wanna combine specific commits into one, use git rebase -i. 
        + But I think git rebase does not only combine specific commits, but also does rebase the commits on the branch.
    
```
git merge --squash
```


Rebase
----------------------

* Rebase can move your previous commits to new commits from the new base. 
* (WARNING) Never rebase your public repository. Never rewrite the history of repository already on public.

```
git rebase -i
```


How to rollback what you did
===============================

* In Git, "git revert" or "git reset" is used. 
    1. If you want to do the opposite thing to the last commit,
        + Use "git revert"
        + Unless the commit is public yet.
        + You are not deleting a hisotry of commit. You are just doing the opposite thing.
    2. If you want to make INDEX or repository to some previous state, 
        + Use "git reset"
        + This can be used to rollback your INDEX and local repository.

* Please keep in mind that 
    1. Workplace is the latest.
    2. INDEX has the next latest.
    3. Repository even at HEAD holds the most out dated information.  


Commands in Practice
-----------------------

* If you want to revert your INDEX without repository change,
    + You can use the HEAD.
    + You can do this using "git reset --mix HEAD"
* If you want to revert your INDEX with repository change,
    + You can specify older <commit_id>s to which you want HEAD to move.
    + You can do this using "git reset --mix <commit_id>"
* If you want to revert your commits, 
    + you need to revert HEAD in your repository.
    + In this case, you need to specify older <commit_id>s
        + You cannot usually use HEAD. The reason is that  it's already up-to-date for the repository. Nothing reverts.
    + You can do this using "git reset --soft <commit_id>"
* If you want to revert your commits along with you workplace and INDEX.
    + You can do this using "git reset --hard <commit_id>"
* If you want to revert your workplace and INDEX to the position corresponding to HEAD in your repository 
    + You can do this using "git reset --hard HEAD"
* If you just want to revert you workplace
    + You can use "git checktout -b newbranch"


* (ref1.) https://stackoverflow.com/questions/2530060/can-you-explain-what-git-reset-does-in-plain-english
* (ref2.) https://www.kaitoy.xyz/2016/01/01/git-revert-reset/ (In Japanese)


How to get to know commit_ids
-------------------------------

* In this case using "git reflog" is the easist way. 
    + The reflog is an ordered list of the commits that HEAD has pointed to

> understanding the reflog means you can't really lose data from your repo once it's been committed. If you accidentally reset to an older commit, or rebase wrongly, or any other operation that visually "removes" commits, you can use the reflog to see where you were before and git reset --hard back to that ref to restore your previous state. Remember, refs imply not just the commit but the entire history behind it."

* (ref.) https://stackoverflow.com/questions/17857723/whats-the-difference-between-git-reflog-and-log



Usual Steps to Version Contorol by Git
======================================

## Before following the following steps

* Prepare blank repository.
    + Create a new repository on Github or Bitbucket. 
    + Never create README file.
    + Just create blank repository.
        + It's really hard to merge two repositories from different origins.

## Start Git

```
# At the top directory
git init
```

## Add remote repository. 

```
git remote add origin https://example.com/path/to/repo.git/
```

## Add files to be monitored by git

* If you use "git add . ", ".gitignore" file needs to be edited beforehand.

```
git add . 
# or
git add <newfile>
```

* You can see which files are being monitored and being ignored.

```
git status --ignored
```

* If you don't like the result, you can reset this step.

```
git reset  
# This is equal to "git reset --mixed HEAD"
#  
# What this does here is
# 1. Rollback index: This is necessary because we want list of monitoring files to be rolled back.
# 2. HEAD position should stay there. Index will be rolled back using the information of current repository at HEAD.
#         + Current HEAD position does not know newly added files.
#         + So it's nice to recreate INDEX based on this HEAD repository information. 
```

## Commit changes of monitored files to local. Push to remote.

* Be careful before you are commiting.
    +  Commiting is a comming, after which you usually never retun.

``` bash
git status   # You can see the changes to be commited.
git commit -a -m "modify read me."
```


## Push to remote.

```
git push -u origin master
```


## From Another Client, Start this project

* You can create local repository from remote repository.
* "git clone" can do this work.
    + "git clone" creates a new directory with the name of <repo_name>.
    + Expand sources under this directory.

```
git clone  https://xxxxxxxx.org/<user>/<repo_name>.git
```


## Trouble shooting in Github.

When the commit is not recognized as your commit. Do the following commands again.
```
git config --global user.name '<username>'
git config --global user.email '<emailaddress>'
```
