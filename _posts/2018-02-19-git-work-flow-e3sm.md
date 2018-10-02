---
layout: post
title: Git work flow E3SM
date: 2018-02-19 15:19
author: hydrotian
comments: true
categories: [Git, Programming, E3SM]
tags: E3SM
---
This is an instruction about how to clone the E3SM model from Github and how to create a case.

## Run E3SM

#### 1) Fork a branch to my own repo on Github
#### 2) Clone a branch from my own repository with submodules
```bash
$ git clone --recursive -b branch/name/here https://username:password@github.com/hydrotian/ACME.git
```
#### 3) Make changes to the code, add them,and commit
```bash
$ git add --all
$ git commit –a -m "comments to this commit"
```
#### 4) Push changes to my repo
```bash
$git push origin name/of/the/branch:name/of/the/branch
```
#### 5) Send pull request on Github

## Additional commands
#### check submodule list
`$ git submodule`
if there are submodules, clone the submodules by
```bash 
$ git submodule init
$ git submodule update
```
#### check remote and branch
```bash
$ git remote 
$ git branch -v
```
#### Add aliases
This following command create a "git hist" aliases based on "git log" to visually check the commit history
```bash
$ git config --global alias.hist "log --pretty=format:'%h %ad | %s%d [%an]' --graph --date=short"
```
#### Go back to old commits
find the hash of the target commit by checking the commit history, then
```bash
$ git checkout    # something like "git checkout 911e8c9"
```
#### Add ssh key to Github to avoid input password
- Type `cd ~/.ssh`.
- Within the `.ssh` folder, there should be these two files: `id_rsa` and `id_rsa.pub`. If not, create them by typing
```bash
ssh-keygen -t rsa -C "your_email@example.com"
```
- Open `id_rsa.pub`, copy everything and paste it into GitHub and/or BitBucket under the Account Settings, SSH Keys

## From old UW post 
A great source: https://githowto.com/

###### 1) On local machine, create an empty git repo in the working directory (e.g. project/) and set default setting:
```bash
$ git init
```
Check the current git settings:
```bash
$ git config –l
```
If no default git settings are set up yet:
```bash
$ git config --global user.name "Name"
$ git config --global user.email "Name@gmail.com"
```

###### 2) Add all files to the local git repo and make the first commit:
```bash
$ git add --all`
$ git commit –a -m "First commit"
```
###### 3) With any changes to the code, commit the changes
```bash
$ git commit -m "Changes are made to ……"
```
###### 4) Create a bare repo outside of the working directory:
```bash
$ cd ..
$ git clone --bare project project.git
```
###### 5) Push to my hydra directory
```bash
$ scp -r project.git/ tizhou@hydra:/git/tizhou
$ cd project
```
###### 6) Link local and hydra repos, and check the remote connections
```bash
$ git remote add origin tizhou@hydra:/git/tizhou/project.git
$ git remote -v
```
###### 7) If any future changes are made to the code in local directory project/, stage and commit the change:
```bash
$ git add –-all
$ git commit –a –m “Changes are made to …”
```
###### 8) Push the change to hydra
```bash
$ git branch (Check the current working branch)`
$ git push –u origin master
```