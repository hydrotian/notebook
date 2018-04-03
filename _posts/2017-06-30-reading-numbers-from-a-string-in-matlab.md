---
layout: post
title: Reading numbers from a string in MATLAB
date: 2017-06-30 14:44
author: hydrotian
comments: true
categories: [Matlab, Programming]
---
I always have needs to find numbers from a formatted string using Matlab. For example, find the lats and longs for all the grid cells of a river basin based on a bunch of input files, something like `forcings_35.25_-100.75`. Then in Matlab you have two ways to read these numbers out.

using 'sscanf'. It's kinda stupid that you can't find the two numbers by

```matlab 
sscanf('forcings_35.25_-100.75','%s_%f_%f')
```
instead, you will get an 1d array that contains 22 numbers, with each number representing a single character in that string. What you need to do is using
```matlab 
sscanf('forcings_35.25_-100.75','forcings_%f_%f')
```
I guess the reason behind this is that sscanf can only output one numerical matrix that based on the first character it processes. So if you do
```matlab 
sscanf('35.25_-100.75_forcings','%f_%f_%s')
```
you will get the first two number you want, followed by 8 numbers representing the string "forcings".
    
Another way is to use "textscan". I feel safer using this method.
```matlab 
A = textscan('forcings_35.25_-100.75','%s %f %f', 'Delimiter', '_')
```
This way, you will end up getting a three cell array representing the three parts separated by "_" in the string. Then simply using cell2mat() to convert the numbers to regular array. To convert "forcings" to string, using
```matlab 
char(A{1})
```