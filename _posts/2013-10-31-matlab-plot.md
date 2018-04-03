---
layout: post
title: Matlab plot
date: 2013-10-31 16:42
author: hydrotian
comments: true
categories: [Matlab, Plotting, Programming]
---
Plotting in Matlab is a very big topic. Here I just want to share some plotting functions I've been using to produce the maps.

If we have a 4*4 matrix and want to plot it as a 2d surface, two functions (`pcolor` and `imagesc`) can be considered. But these two functions yield different results:

create a matrix first:

```matlab
A = hadamard(4);
A = 
1  1  1  1
1 -1  1 -1
1  1 -1 -1
1 -1 -1  1
```
The` pcolor (A)` will cut the last column and row then flip upside down before plot. That's because the first cell (1,1) is considered as origin point (0,0) at lower left. So the plot looks like this (red is 1, blue is -1):

<a href="http://tianzhounote.files.wordpress.com/2013/10/untitled.png"><img class="alignnone size-medium wp-image-756" alt="untitled" src="http://tianzhounote.files.wordpress.com/2013/10/untitled.png?w=300" width="300" height="225" /></a>

But the `imagesc (A)` will give the as-is plot:

<a href="http://tianzhounote.files.wordpress.com/2013/10/untitled1.png"><img class="alignnone size-medium wp-image-757" alt="untitled" src="http://tianzhounote.files.wordpress.com/2013/10/untitled1.png?w=300" width="300" height="225" /></a>
