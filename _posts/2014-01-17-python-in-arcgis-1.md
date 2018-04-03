---
layout: post
title: Python in ArcGIS 1
date: 2014-01-17 02:14
author: hydrotian
comments: true
categories: [ArcGIS, Other, Python]
---
1. Python editor IDLE will be installed with ArcGIS. Right click .py file can find the IDLE option in the menu.

2. print
<pre>strName = "Tian"
print strName
print (strName)
print 10*strName
print 'Tian'
print "Tian"</pre>
3. in Python Shell, type into variable name gives the value of the variable. (kinda like matlab)

4. type(strName) gives the type of variable.

5. vars() gives all the variables exist.

6. list contains more variables
<pre>lstMonths=['May','Jun','July']
lstMonths[0]   #shows the 1st element of the list "May"
len(lstMonths) #length of the list, like matlab
lstMonths[1:]  #all the elements except the 1st one
lstMonths[:-1] #all the element except the last one
lstYears = range (2000,2014,2) #doesn't contain the last one, 2 is the increment
lstYears.index(2008) #shows the index of this element
lstYears.count(2008) #counts how many 2008 in the list</pre>
7. Loops
<pre>lstMonths=['May','Jun','July']
for i in lstMonths:
    print i

# or
for i in range(0, len(lstMonths)-1):
    print(lstMonths[i])</pre>
8. Modules
First, create a module, save it as "testmodule.py"
<pre>#test module

#function do the square
def valueSquared(myVar):
 fltSquared = myVar*myVar
 return (fltSquared)</pre>
Second, use this module:
<pre>import testmodule
dir (testmodule) #see what function in this module
help (testmodule.valueSquared) #show the comment above this function
testmodule.valueSquared(2.0) #will return 4.0</pre>
Another way to import the module:
<pre>from testmodule import*
valueSquared(2.0) #then you don't need to put the module name (prefix) all the time</pre>
9. OS module
OS module operates files:
<pre>import os
os.getcwd() #returns the current working dir
os.chdir('C:/temp') #changes dir
os.listdir('C:/temp') #list files</pre>
10. glob module
<pre>import glob
glob.glob('C:/temp/*.py') #glob can find the files using wildcard</pre>
