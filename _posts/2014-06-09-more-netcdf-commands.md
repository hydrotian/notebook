---
layout: post
title: More NetCDF commands
date: 2014-06-09 10:44
author: hydrotian
comments: true
categories: [NetCDF, Unix]
---
1. NetCDF file compression
```bash
ncks -L 2 in.nc out.nc    # 2 is the compression ratio, could be 2-9
```
2. Split file by dimension (e.g. time, lat, lon)
```bash
ncks -d time,0,30 in.nc out.nc
ncks -F -d time,1,31 in.nc out.nc  #equivalent
```
3. Merge time using NCO
```bash
ncks --mk_rec_dmn time in.nc out.nc
ncrcat *.nc out.nc
```

