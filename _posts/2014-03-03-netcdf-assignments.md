---
layout: post
title: NetCDF plotting assignments
date: 2014-03-03 12:03
author: hydrotian
comments: true
categories: [NetCDF, Plotting]
---
So the assignment is <a href="http://www.hydro.washington.edu/~nijssen/computing_workshops/goodplot_badplot/good_plot_bad_plot_assignment.html">here</a>.

**Problem 1.** Time series of annual mean precipitation (pr) and temperature (tas) over your area compared with the global average for the historic period. (I am going to abbreviate this as historic: ts of annual mean pr and tas for area and globe).

1. download the nc files and then cut the south hemisphere (our region) out using `cdo sellonlatbox` or `cdo elindexbox`
```bash
cdo selindexbox,1,192,1,48 tas_Amon_MPI-ESM-LR_rcp85_r1i1p1_200601-210012.nc temp_rcp85_SH.nc
```
2. find the global and area annual mean using `cdo fldmean` and `cdo yearmean`, then convert the prec unit from kg/m^2s (mm/s) to mm/yr by multiply 31536000 and the temp unit from K to C
```bash
cdo fldmean prec_SH.nc prec_SH.mean.nc
cdo yearmean prec_SH.mean.nc prec_SH.annual.mean.nc
cdo mulc,31536000 prec_SH.annual.mean.nc prec_SH.annual.mean.mm_yr.nc
cdo addc,-273.15 temp_SH.annual.mean.nc temp_SH.annual.mean.C.nc
```
3. output the data to ascii using <span style="text-decoration:underline;">cdo output</span> and then plot them in Excel or whatever
```bash
cdo output prec_SH.annual.mean.mm_yr.nc &gt; prec_SH.annual.mean.mm_yr.txt
```

**Problem 2.** historic: ts of annual anomaly of pr and tas for area and globe. The anomaly should be calculated relative to the period 1970-1999.

1. find the mean values for 1970-1999 using `cdo seldate` and `cdo timmean`
```bash
cdo seldate,1970-01-01,1999-12-31 prec_global.annual.mean.mm_yr.nc prec_global.annual.mean.7099.nc
cdo timmean prec_global.annual.mean.7099.nc out.nc
```
2. then I can find the mean prec and temp for 70-99:
```
 South Hemisphere mean prec 70-99: 1052.65 mm/yr
 South Hemisphere mean temp 70-99: 13.53 C
 Global mean prec 70-99: 1068.36 mm/yr
 Global mean temp 70-99: 14.08 C
```
3. subtract the mean value to get the anomaly time series
`cdo addc,-1052.65 prec_SH.annual.mean.mm_yr.nc prec_SH.anom.nc`

**Problem 3.** climate change: ts of annual anomaly of pr and tas for area and globe for RCP4.5 and RCP8.5. The anomaly should be calculate relative to the period 1970-1999.

see problem 2

**Problem 4. ** Historic: seasonal mean 2D map of pr and tas for your area for DJF, MAM, JJA, SON for the period 1970-1999. This should be 4 panels on one slide.
`cdo yseasmean $infile $outfile`
**Problem 5.** Climate change: seasonal mean 2D map of pr and tas for your area for DJF, MAM, JJA, SON for the periods 2020-2049 and 2070-2099 for RCP4.5 and RCP8.5. This should be 4 panels on one slide for each period and each RCP.

**Problem 6.** Climate change: repeat 5. as an anomaly to the period 1970-1999.

**Problem 7.** Climate change: 2D plots for RCP4.5 and RCP8.5 for your area with a mask that shows the areas in which mean annual temperature change and precipitation change are both in the upper 10% (basically show areas that are expected to experience both extreme temperature and precipitation change).

1. calculate the annual mean for periods 1970-1999 (historical) and 2070-2099 (RCP 4.5 and RCP8.5)

2. find the temp and prec relative differences between two time periods using `cdo sub` and `cdo div`

3. find the 90 percentile thresholds for both temp and prec using `cdo fldpctl` then find the grid cells larger than the thresholds using `cdo gtc`. Then find the intersection.
```bash
cdo sub mean.seasonal.SH.prec_rcp85_global.7099.nc mean.seasonal.SH.prec_global.7099.nc out1.nc
cdo div out1.nc mean.seasonal.SH.prec_global.7099.nc out2.nc
cdo fldpctl,90 out2.nc out3.nc
value=`cdo output out3.nc | less`
vv=`echo $value`
cdo gtc,$vv out2.nc outprec.nc

cdo sub mean.seasonal.SH.temp_rcp85_global.7099.nc mean.seasonal.SH.temp_global.7099.nc out1.nc
cdo div out1.nc mean.seasonal.SH.prec_global.7099.nc out2.nc
cdo fldpctl,90 out2.nc out3.nc
value=`cdo output out3.nc | less`
vv=`echo $value`
cdo gtc,$vv out2.nc outtemp.nc

cdo add outprec.nc outtemp.nc out.nc
cdo eqc,2 out.nc export.nc
```
