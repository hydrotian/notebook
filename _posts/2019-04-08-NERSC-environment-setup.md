---
layout: post
title: NERSC environment setup
date: 2019-04-08 15:19
author: Tian
comments: true
categories: [Unix, Programming]
tags: Null
---
Recently I have been working on [NERSC](https://www.nersc.gov/) quite a lot. One thing that bothers me was that the default environment setup of NERSC user sucks. For example, it doesn't have color scheme for `ls` command, the prompt is too long when you go deep into a few sub-directories because it shows the full path.

To fix this, I modified the `.bashrc.ext` file in the home directory (The system doesn't allow you to change the `.bashrc` file).

## Three lines added at the bottom of this file

##### let the ls showing the color scheme automatically
```bash
alias ls='ls --color=auto'
```
##### make the prompt shorter by just showing the user (u) and host (h) , and the current directory (W)
a nice [article](https://www.cyberciti.biz/faq/bash-shell-change-the-color-of-my-shell-prompt-under-linux-or-unix/) about this.
```bash
export PS1="\[$(ppwd)\]\u@\h:\W>"
# if change W to w it will show the full path
```
##### pre-load some modules
```bash
module load python cray-netcdf
```
## Some paths on NERSC
```bash
/global/cscratch1/sd/tizhou # cscratch
/global/project/projectdirs/acme/tizhou # project dir (E3SM)
/global/project/projectdirs/m2422/tizhou # project dir (Maoyi, Pflotran)
/global/project/projectdirs/acme/www/tizhou # public directory
/scratch1/scratchdirs/tizhou # edison scratch, retire soon
```
The public directory will be available [here](https://portal.nersc.gov/project/acme/tizhou/)