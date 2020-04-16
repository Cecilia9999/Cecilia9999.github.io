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
$ git clone <ssh of remote repo> <dir>  
```
Then you can change in local repository. After edition, let's submit to remote repository. Enter into your local repository and track files
``` bash
$ git add <files>   # git add . :  add all files
```
After add files, you can use ``` git status ``` to check tracking files. If correct, then commit and submit to remote repo
``` bash
$ git commit -m "<comments>"   
# submit current branch local repo (origin) to remote branch
$ git push origin <remote branch>    
```
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
Before you push to remote repository, don't forget to **map** local and remote repository
``` bash
$ git remote add origin <ssh/https of remote repo>
```
Then you can submit local repository to your remote successfully
``` bash
$ git push origin <remote branch>    
```

# Pull local repository
``` bash
$ git pull origin <remote branch>
```
or
``` bash
$ git pull --rebase origin <remote branch>  # recommend
```
