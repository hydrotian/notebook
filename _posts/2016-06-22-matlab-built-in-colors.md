---
layout: post
title: Matlab built-in colors
date: 2016-06-22 12:05
author: hydrotian
comments: true
categories: [Matlab, Plotting]
---
<img class="alignnone size-full wp-image-1130" src="https://tianzhounote.files.wordpress.com/2016/06/capture2.png" alt="Capture" width="329" height="419" />

to get RGB values for default Matlab color order, use
`get(gca,'ColorOrder')`
or use `colormap` to get values for any built-in style

```matlab
rgb = colormap(parula(10)); % 10 is number of color
```
