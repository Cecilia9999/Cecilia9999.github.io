---
title: Git Command
date: 2020-04-16 21:39:58
categories:
  - tools
tags:
  - git
---

Almost everyday you may use git to manage your project. This article is for recording some commands I often use. All commands posted here are checked on web and are updated continuously. Let's see it!
<!--more-->

Firstly, I would like to put the official [documentation of Git]() here and emphasize how **important** it is to check documentation whenever you have confusions and problems.
The followings is a summary of some git commands I often use or I've met before.

# Get a Git repository
From GitHub Documentation:
> You typically obtain a Git repository in one of two ways:
>  1. You can take a local directory that is currently not under version control, and turn it into a Git repository, or
>  2. You can clone an existing Git repository from elsewhere.

##  1. Git clone a remote repository to local
Choose a local site to download remote repository
``` bash
# if not specify local path <dir>, download remote repo
# with same name into current path  
$ git clone <remote url> <dir>  
```
Then you can change in local repository. After edition, let's submit to remote repository. Enter into your local repository and track files
``` bash
$ git add <files>   # git add . :  add all files
```
After add files, you can use ``` git status ``` to check tracking files. If correct, then commit and submit to remote repo
``` bash
$ git commit -m "<comments>"   

# submit current branch local repo (origin) to remote branch
$ git push <shortname> <local branch>:<remote branch>    
```
details of **git push** command see **_Push to remote repository_**  

Note that you can create new local branch by
``` bash
$ git checkout -b <new branch>
```
or change to an existed local branch by
``` bash
$ git checkout <branch>
```
If you want to recall ``` git add <files> ```
``` bash
# if not specify <files>, recall all files git add most recently
$ git reset HEAD <files>  
```

## 2. Make a local repository and submit to remote
Enter into your local project and initialize your project as a new repo by
``` bash
$ git init
```
Then you will see ``` .git ``` exists and remember this file means it is a git repository. Track and commit project
``` bash
$ git add <files>  
$ git commit -m "<comments>"   
```
Before you push to remote repository, don't forget to **map** local and remote repository,
(actually use a shortname instead of writing a long url of remote everytime)
``` bash
$ git remote add <shortname> <remote url>
```
We often use **origin** as a default *<shortname>*
``` bash
$ git remote add origin git@git://...
```
Then you can submit local repository to your remote successfully
``` bash
$ git push origin <remote branch>    
```

# Push to remote repository
**push** : from local to remote
``` bash
$ git push <shortname> <local branch>:<remote branch>
```
If remote branch is not existing, a new one will be created. Be careful there is no space between _\<local branch\>_ and _\<remote branch\>_.
There are some shorthands:

* no _\<remote branch\>_, the local branch will be pushed to where local _HEAD_ refers to
``` bash
$ git push <shortname> <local branch>
```
* no _\<local branch\>_, an empty local branch will be pushed to remote branch
``` bash
$ git push <shortname> :<remote branch>
```
which, actually, means **delete** the _\<remote branch\>_
``` bash
$ git push <shortname> --delete <remote branch>
```

For the first time when remote repository is empty, add arg _-u_
``` bash
$ git push -u <shortname> <local branch>:<remote branch>
```


# Pull from remote repository
**pull** : from remote to local
``` bash
$ git pull origin <remote branch>
```
or
``` bash
$ git pull --rebase origin <remote branch>  # recommend
```

# Git remote
1) Remove current mapping remote
``` bash
$ git remote rm <shortname>
```
2) Map local to remote
``` bash
$ git remote add <shortname> <remote url>
```
3) Check all remote
``` bash
$ git remote
$ git remote -v   # show urls of all remote
```
4) Rename remote
``` bash
$ git remote rename <old shortname> <new shortname>
```


# Git branch
1) Check branch
``` bash
$ git branch      # check all existing branches
$ git branch -a   # check all local and remote branches
$ git branch -v   # check commit infos of all branches
```
2) Create branch
``` bash
$ git branch <new branch>  # not change HEAD reference
```

3) Delete branch
``` bash
$ git branch -d <branch>  # del a branch (already fully merged)
$ git branch -D <branch>  # force to del a branch   
```


# Git checkout
> _git checkout_ will also update HEAD to set the specified branch as the current branch.

1) Switch to a branch
``` bash
$ git checkout <branch>
```

2) Create new branch and switch to it
``` bash
$ git checkout -b <new branch>           
$ git checkout -b <new branch> <branch>  # create new br based on br
```
_Compare_: ```git checkout -b ``` will create new reference(new branch) for current commit and meanwhile update _HEAD_ to refer to this new branch; As for ``` git branch ```, it also creates a new branch which refers to new commit, but leaves HEAD detached (_HEAD_ not changes).
