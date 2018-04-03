---
layout: post
title: Tecplot in cmd
date: 2016-03-29 11:36
author: hydrotian
comments: true
categories: [CFD]
---
It's kinda weird to use command lines in Windows (maybe a lot of people do it, who knows) but the reason I'm using it is that in Windows based Matlab, the "system" function will just call cmd. So if I need to run something inside a Matlab loop, I should learn how to use cmd.

Here is the command line I used to open a data file and run a python script in Tecplot:
```cmd
C:\ /WAIT tec360 -b -datafile C:\something.plt -p C:\some\python.py
```
- `/WAIT` is to hold the program until it's done
- `-b` is batch mode, include this otherwise you will see the GUI
- `-datafile` is reading file
- `-p` is executing python script
##### Note that the python script must be put in the python path otherwise Tecplot won't find it.
