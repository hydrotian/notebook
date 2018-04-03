---
layout: post
title: ArcGIS shape file plot in Matlab
date: 2016-06-20 14:07
author: hydrotian
comments: true
categories: [ArcGIS, Matlab]
---
ArcGIS shapes such as points and polygon, or even raster files can be plotted in Matlab. Here is an example of reading and plotting HUC4 polygons in Matlab.

First step is reading the shape file that's generated in ArcGIS
```matlab
%read in shapefile
huc = shaperead('C:\HUC4.shp','UseGeoCoords', true, 'BoundingBox', [lonlim', latlim']);
```
then we get a struct like this, in which most of the columns are attribute table variables, with the first few columns showing the geometrical information about each polygon.

<img class="alignnone size-full wp-image-1080" src="https://tianzhounote.files.wordpress.com/2016/06/capture.png" alt="Capture" width="646" height="460" />

Sometimes the vertices are too many for each polygon, which makes the plotting process extremely slow. Here I am using a tool called <a href="http://www.mathworks.com/matlabcentral/fileexchange/34639-decimate-polygon">DecimatePoly</a> to reduce the total number of vertices 10 times less without making noticeable changes on the shape.

Next we will define the color of the polygon by one of the variables in the attribute table using makesymbolspec:
```matlab
faceColors = makesymbolspec('Polygon',{'INDEX',[1 lenS],'FaceColor',color_att});
%lenS is the number of polygon
%color_att is a 3-column matrix of RGB triplets with length of LenS
```
Next is to create a map frame to plot on:
```matlab
ax = usamap(latlim,lonlim); %plot frame using usamap as template 
axis off; framem on; gridm on; mlabel on; plabel on;
setm(gca,'MLabelLocation',10) %interval for meridians labeling
setm(gca,'PLabelLocation',5)  %interval for parallels labeling
```
Then plot the polygons
```matlab
geoshow(ax, huc, 'SymbolSpec', faceColors);
```
When plotting points on the map, use scatterm
```matlab
scatterm(lat,lon,s,c,'filled'); % s is vector defines the size of the dots
                                % c is RGB defines the color of the dots
```
<img class="alignnone size-full wp-image-1126" src="https://tianzhounote.files.wordpress.com/2016/06/untitled.png" alt="untitled" width="1600" height="800" />
