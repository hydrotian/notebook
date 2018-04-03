---
layout: post
title: Clip a basin in NetCDF
date: 2014-01-15 14:02
author: hydrotian
comments: true
categories: [NetCDF, Unix]
---
Problem: I want to clip a basin (e.g. Nena) from a global netcdf file (e.g. global.nc) and output a new .nc file (nena.nc). What I have is a shapefile of this basin.

Step1: Generate a set of coordinates of the basin boundary.
In ArcGIS, use "Feature Vertices To Points" and then "Export Feature Attribute to ASCII" to output the coordinates file (lena.txt), which looks like

```
 113.25000000 52.25000000 1
 113.25000000 52.50000000 2
 113.00000000 52.50000000 3
 113.00000000 52.25000000 4
 112.75000000 52.25000000 5
 112.75000000 52.50000000 6
 112.50000000 52.50000000 7
 112.50000000 52.25000000 8
 ......
```
then use "cut" in shell to cut the last column.

Step2: Using cdo to clip the basin.
```bash
$ cdo maskregion,lena.txt global.nc lena.nc
```
