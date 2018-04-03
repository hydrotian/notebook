---
layout: post
title: Delineate basins based on flow directions
date: 2013-10-30 12:42
author: hydrotian
comments: true
categories: [ArcGIS]
---
1. Download the flow direction file (FDR) from <a href="http://files.ntsg.umt.edu/data/DRT/upscaled_global_hydrography/">here</a> (Wu, et. al. 2012) then import it into ARCGIS.
<a href="http://tianzhounote.files.wordpress.com/2013/10/capture.png"><img class="alignnone size-medium wp-image-745" alt="Capture" src="http://tianzhounote.files.wordpress.com/2013/10/capture.png?w=300" width="300" height="132" /></a>

2. Using "Basin" tool to identify the basins and then convert the generated raster to polygons.
<a href="http://tianzhounote.files.wordpress.com/2013/10/capture1.png"><img class="alignnone size-medium wp-image-746" alt="Capture" src="http://tianzhounote.files.wordpress.com/2013/10/capture1.png?w=300" width="300" height="125" /></a>

3. Select the desired basin and output the polygon as a single file (Mekong Basin here).
<a href="http://tianzhounote.files.wordpress.com/2013/10/capture2.png"><img class="alignnone size-medium wp-image-747" alt="Capture" src="http://tianzhounote.files.wordpress.com/2013/10/capture2.png?w=225" width="225" height="300" /></a>

4. Use "extract by mask" to extract the flow direction for the desired basin.
<a href="http://tianzhounote.files.wordpress.com/2013/10/capture3.png"><img class="alignnone size-medium wp-image-748" alt="Capture" src="http://tianzhounote.files.wordpress.com/2013/10/capture3.png?w=235" width="235" height="300" /></a>

5. Export the flow direction as ascii
