---
layout: post
title: Git work flow UW version
date: 2013-09-26 17:06
author: hydrotian
comments: true
categories: [Git, Unix]
---
A great source: https://githowto.com/

1) On local machine, create an empty git repo in the working directory (e.g. project/)
and set default setting:
`$ git init`
Check the current git settings:
`$ git config –l`
If no default git settings are set up yet:
`$ git config --global user.name "Name"`
`$ git config --global user.email "Name@gmail.com"`
2) Add all files to the local git repo and make the first commit:
`$ git add --all`
`$ git commit –a -m "First commit"`
3) With any changes to the code, commit the changes
`$ git commit -m "Changes are made to ……"`
4) Create a bare repo outside of the working directory:
`$ cd ..`
`$ git clone --bare project project.git`
5) Push to my hydra directory
`$ scp -r project.git/ tizhou@hydra:/git/tizhou`
`$ cd project`
6) Link local and hydra repos, and check the remote connections
`$ git remote add origin tizhou@hydra:/git/tizhou/project.git`
`$ git remote -v`
7) If any future changes are made to the code in local directory project/, stage and
commit the change:
`$ git add –-all`
`$ git commit –a –m “Changes are made to …”`
8) Push the change to hydra
`$ git branch (Check the current working branch)`
`$ git push –u origin master`

#### Additional examples

- Clone a branch by providing the username and password in the same line

`$ git clone -b branch/name/here https://username:password@github.com/ACME-Climate/ACME.git`
check submodule list
`$ git submodule`
if there are submodules, clone the submodules by
`$ git submodule init`
`$ git submodule update`
Or, from git clone, add `--recursive` operation will automatically update the submodules

- Add aliases

This following command create a `git hist` aliases to check the commit history
`$ git config --global alias.hist "log --pretty=format:'%h %ad | %s%d [%an]' --graph --date=short"`

- Go back to old commits

find the hash of the target commit by checking the commit history, then
`$ git checkout    # something like "git checkout 911e8c9"`

- Revert the commit that already pushed to remote
1. make sure you are on the branch head
2. move the head one step backward
`$ git reset --hard HEAD~1`
3. do some modifications and commit
4. push the new commit to the origin
`$ git push origin --force`

