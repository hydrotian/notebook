---
layout: post
title: Shift a map 180 degree
date: 2013-02-04 13:00
author: hydrotian
comments: true
categories: [Programming, Matlab]
---
Sometimes I need to shift a map 180 degree back and forth when I'm processing TMPA 3B42RT and 3B42(RP) data. It's relatively easy of doing this when the map is in 2D matrix format but a bit complicated in 1D array format. Here's how I did it in **Matlab**. Note that the index numbering is different between **Matlab** and **Perl** (The first element of an array in Matlab is "Array(1)" but is "Array(0)" in Perl).

The first element of the array is the upper left cell in the map,  second element is the second cell in the first row of the map.

<img class="size-medium wp-image-76 aligncenter" style="font-size:1rem;line-height:1;" alt="1" src="http://tianzhounote.files.wordpress.com/2013/02/1.jpg?w=300" width="300" height="90" />

<img class="size-medium wp-image-77 aligncenter" style="font-size:1rem;line-height:1;" alt="2" src="http://tianzhounote.files.wordpress.com/2013/02/2.jpg?w=300" width="300" height="89" />

```matlab
for i = 1:nrow; %shift 180 degree, nrow is row number and ncol is column number
 map_new(((i-1)*ncol+(ncol/2)+1):(i*ncol)) = map (((i-1)*ncol+1):((i-0.5)*ncol));
 map_new(((i-1)*ncol+1):((i-0.5)*ncol)) = map (((i-1)*ncol+(ncol/2)+1):(i*ncol)); 
end
```

