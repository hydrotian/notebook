---
layout: post
title: Using gdb to debug (segmentation error)
date: 2015-11-02 21:44
author: hydrotian
comments: true
categories: [Programming]
---
- compile the code and get the executable (e.g. vicNl)
- type `gdb`
- in gdb environment, type `file vicNl`
- `run -v` -v is the argument input for vicNl
- when it stops, type `bt` to back track the error
