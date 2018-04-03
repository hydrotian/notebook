---
layout: post
title: Interpolate values along a line from a surface in ArcGIS
date: 2016-07-22 15:44
author: hydrotian
comments: true
categories: [ArcGIS]
---
This is kinda stupid and I'm sure there are much more elegant ways to do this. But for a quick and dirty job it's good enough to me.

Anyways, what I want to do is to draw a line on a surface (could be anything, DEM, velocity field from CFD, or precipitation map) and extract the values along this line then output as a table. It can be easily done using ArcGIS 3D Analyst toolbar then you get something like this:

<img class="alignnone size-full wp-image-1150" src="https://tianzhounote.files.wordpress.com/2016/07/capture.png" alt="Capture" width="635" height="350" />

By clicking right mouse this data can be saved in a file and job done.

However, I just found that if you need to output data along the same line for different surfaces, this toolbar thing could become very annoying and you may need to draw multiple lines manually, which make them not exactly the same.

So here is another way to do this. First create a polyline, then in "Editor" tool, split it to equal length segments. Second, in 3D Analyst Tools -&gt; Functional Surface -&gt; Add Surface Information, select the surface and polyline just created, and choose Z_MEAN as output property. Run it, then you get the mean value of each segment in the attribute table.

Final trick, in attribute table, select some features, then right click the very left column and hold "shift" at the same time, there's a copy option to allow you paste the table in Excel.
