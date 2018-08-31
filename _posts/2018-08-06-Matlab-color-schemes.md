---
layout: post
title: Matlab color schemes
date: 2018-08-06 15:19
author: Tian
comments: true
categories: [Matlab]
tags: Null
---
Today I saw a great [blog](http://kellyakearney.net/2015/12/18/cptcmap.html) that use GMT's color setup file (.cpt) in Matlab

- First download the cptcmap.m and then link it to Matlab.
- Second put the .cpt files into a folder which is also linked to Matlab. The .cpt files can be found in [cpt-city](http://soliton.vm.bytemark.co.uk/pub/cpt-city/)
- Then in Matlab use cptcmap to override the Matlab color scheme.
```matlab
cptcmap('GMT_globe', 'mapping', 'direct');
```
- I also don't like the default font used in Matlab, change it to "Segoe UI Semilight" to make it look better.
```matlab
set(gca,'fontname','Segoe UI Semilight')  % Set the font to Segoe UI Semilight
```