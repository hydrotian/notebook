---
layout: post
title: Python Bootcamp
date: 2014-07-31 00:42
author: hydrotian
comments: true
categories: [Python]
---
1. clear all the variables in Sypder
```python
import sys
sys.modules[__name__].__dict__.clear()
```
2. Some tips (python notebook)
```python
                  #shift + enter to run lines
                  #triple quotes for multi-line string
                  #underscore denotes result from last operation
!ls               #see what's in dir
range?            #check info about "range"
something.        #press Tab to see options
x,y = y,x         #easy swap, multipal assign
bool(-1)          #see Boolean data type
str(13)           #Matlab num2str(13) equivalent
print(type(True)) #see the data type, will return bool
x=range(4)        #return [0,1,2,3]
```
3. Keep in mind that in
Python2: `5/2=2; 5/2.0=2.5;`
Python3: `5/2=2.5;`
4. Getting input from user
```python
faren = float(input("enter a temperature (in F): "))
print "Temperature in C is " + str((faren - 32) / 1.8)faren = float(input("enter a temperature (in F): "))
print "Temperature in C is " + str((faren - 32) / 1.8)
```
5. The indexing

<a href="https://tianzhounote.files.wordpress.com/2014/07/screen-shot-2014-09-22-at-4-49-13-pm.png"><img class="alignnone  wp-image-830" src="http://tianzhounote.files.wordpress.com/2014/07/screen-shot-2014-09-22-at-4-49-13-pm.png?w=300" alt="Screen Shot 2014-09-22 at 4.49.13 PM" width="335" height="153" /></a>

<a href="http://nbviewer.ipython.org/gist/anonymous/96fa78e3280c5ada1269">http://nbviewer.ipython.org/gist/anonymous/96fa78e3280c5ada1269</a>

6. The data structures

<a href="http://nbviewer.ipython.org/gist/hydro-tian/70491b27495b90c4aa48">http://nbviewer.ipython.org/gist/hydro-tian/70491b27495b90c4aa48</a>

7. Magic line
```python
%timeit        #timing one line
%%timeit       #timing the whole cell
%%bash         #turn whole cell to bash, same as ruby,html,etc.
%%file text.py #put in the 1st line, will save and overwrite the content in the cell to this file
%debug         #debug mode
%matplotlib inline # put plots in the cell
%lsmagic       #list other available magic lines
```
8. Matplotlib 101

<a href="http://nbviewer.ipython.org/gist/hydro-tian/7df6738e7afa778ad5e6">http://nbviewer.ipython.org/gist/hydro-tian/7df6738e7afa778ad5e6</a>
