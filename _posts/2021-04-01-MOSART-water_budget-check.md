---
layout: post
title: MOSART water budget check in coupled E3SM simulations
date: 2021-04-01 15:19
author: Tian
comments: true
categories: [E3SM, Programming]
tags: Null
---
This is a guide about how to check MOSART water budget between two components in E3SM

## Introduction

In current E3SM versions (V1 and V2), the river component, MOSART, only does the internal water balance check to make sure`input - output = delta storage` at every coupling time step (3 hourly). It does not average the water budget terms on monthly or annual basis in the log files. This makes it difficult to do water balance check between the river component and the coupler, in which the water budgets from each model component are summarized at monthly time scale in the log file like this:

```fortran
(seq_diag_print_mct) NET WATER BUDGET (kg/m2s*1e6): period =  monthly: date =  19800201     0
                           atm            lnd            rof            ocn         ice nh         ice sh            glc        *SUM*  
         wfreeze     0.00000000     0.00000000     0.00000000     0.00000000     0.00000000     0.00000000     0.00000000     0.00000000
           wmelt     0.00000000     0.00000000     0.00000000     0.00000000     0.00000000     0.00000000     0.00000000     0.00000000
           wrain   -29.57235627     5.41307635     0.00000000     0.00000000     0.00000000     0.00000000     0.00000000   -24.15927992
           wsnow    -2.05972140     0.98147580     0.00000000     0.00000000     0.00000000     0.00000000     0.00000000    -1.07824559
           wevap     0.00000000    -2.82794138     0.00000000     0.00000000     0.00000000     0.00000000     0.00000000    -2.82794138
         wrunoff     0.00000000    -0.75014053     0.36133891     0.00000000     0.00000000     0.00000000     0.00000000    -0.38880162
         wfrzrof     0.00000000    -0.18369858     0.00000000     0.00000000     0.00000000     0.00000000     0.00000000    -0.18369858
           *SUM*   -31.63207767     2.63277166     0.36133891     0.00000000     0.00000000     0.00000000     0.00000000   -28.63796710
```

The `rof` column indicates the river component (MOSART) which has only one positive value **0.36133891** at row `wrunoff`. This means that river is taking water from the coupled system at a monthly averaged rate of **0.36133891** (kg/m2s*1e6)  . This rate is calculated based on the total surface area of the earth, including both ocean and land. 

If we have the same value calculated from the river component, we can say that the coupler handles the water budget terms correctly, in another word, there's no water conservation issue when passing water from river to the coupler. Such check is getting more and more necessary as more features are added to MOSART.

Before we introduce the monthly budget table into MOSART, there's a `quick and dirty`way to do the check. Of course, it involves some efforts.  

## Steps to do the water budget checks

### 1. turn on a few flags in MOSART source code so that it could output the information needed for the check

- In `\mosart\src\riverroute\RtmMod.F90`, find `output_all_budget_terms` and change it to `.true.`.
- In the same file, find `budget_write` and change it to `.true.`.
- compile the model.

### 2. make sure the `BUDGETS` is `.true.` in `env_run.xml` so that the coupler will output the budget table above.

### 3. run the model for at least a few months

### 4. find the budget terms from the MOSART log file

- The water balance equation for any given time period in MOSART is

  `volume change - input + output - other = 0`

  where `volume change` is the water volume change in river channels, hillslopes, floodplains, and reservoirs during the period; `other` is a lag volume term of the channel outflow caused by the MOSART subcycling scheme which should also be accounted for the total volume change during the period. To calculate the monthly water budget and to compare it with the budget in the coupler, we need to find out the **initial volume** and the **final volume** of the month, as well as the **total "other volume"** over the month from the MOSART log file.

- The water budget for the very first time step in the log file is like this (note that the water budget of the first time step is at 10800 second (3 hr) of the first day of the first month):
```fortran
  (Rtmrun) MOSART BUDGET diagnostics (million m3) for   19800101 10800
  (Rtmrun)-----------------------------------------------------------------
  (Rtmrun)  tracer =    1
  (Rtmrun)   dvolume wh    =    1              0.000000
  (Rtmrun)   dvolume wt    =    1              0.000000
  (Rtmrun)   dvolume wr    =    1              0.000000
  (Rtmrun)   dvolume dstor =    1              0.000000
  (Rtmrun) * dvolume total =    1              0.000000
  (Rtmrun) x dvolume check =    1              0.000000 (should be zero)
  (Rtmrun) x volume   init =    1              0.000000
  (Rtmrun) x volume  final =    1              0.000000
  (Rtmrun) x volumeh  init =    1              0.000000
  (Rtmrun) x volumeh final =    1              0.000000
  (Rtmrun) x volumet  init =    1              0.000000
  (Rtmrun) x volumet final =    1              0.000000
  (Rtmrun) x volumer  init =    1              0.000000
  (Rtmrun) x volumer final =    1              0.000000
  (Rtmrun) x storage  init =    1              0.000000
  (Rtmrun) x storage final =    1              0.000000
  ...
```
- The water budget for the first time step of the second month (or first step after the final step of the first month):
```fortran
(Rtmrun) MOSART BUDGET diagnostics (million m3) for   19800201 10800
(Rtmrun)-----------------------------------------------------------------
(Rtmrun)  tracer =    1
(Rtmrun)   dvolume wh    =    1            172.580688
(Rtmrun)   dvolume wt    =    1            281.340362
(Rtmrun)   dvolume wr    =    1            -44.947927
(Rtmrun)   dvolume dstor =    1              0.000000
(Rtmrun) * dvolume total =    1            408.973123
(Rtmrun) x dvolume check =    1              0.000000 (should be zero)
(Rtmrun) x volume   init =    1         491655.966962
(Rtmrun) x volume  final =    1         492064.940085
(Rtmrun) x volumeh  init =    1          46749.098846
(Rtmrun) x volumeh final =    1          46921.679533
(Rtmrun) x volumet  init =    1          70426.281867
(Rtmrun) x volumet final =    1          70707.622229
(Rtmrun) x volumer  init =    1         374480.586249
(Rtmrun) x volumer final =    1         374435.638322
(Rtmrun) x storage  init =    1              0.000000
(Rtmrun) x storage final =    1              0.000000
(Rtmrun)----------------
...
```
- From the two tables we can see the **initial volume is 0** and the **final volume is 491655.966962** (million m3) for the first month.

-  The "other volume" term is written at each time step, we can extract it from the log file using 
  `grep 'sum other     =    1' rof.log.XXXXXX > other.txt`

- We will get a text file that lists the "other volume" at each time step:
 ```fortran
(Rtmrun)   sum other     =    1              0.000000
(Rtmrun)   sum other     =    1             -0.000000
(Rtmrun)   sum other     =    1             -0.000001
(Rtmrun)   sum other     =    1             -0.000021
(Rtmrun)   sum other     =    1             -0.000207
(Rtmrun)   sum other     =    1             -0.001263
(Rtmrun)   sum other     =    1             -0.003941
(Rtmrun)   sum other     =    1             -0.007846
(Rtmrun)   sum other     =    1             -0.012761
(Rtmrun)   sum other     =    1             -0.019006
(Rtmrun)   sum other     =    1             -0.027986
...
...
 ```
- Then there's a tricky step. We need to sum the "other volume" over the time period we are examining. For example, if we are examining the first month, which is  January (31 days), then we need to sum the first 8*31 = 248 values to get the total "other volume" for this month.  In this case, the **total "other volume"** over the first month is **-2023.689434** (million m3).

### 5. Calculate the monthly water budget from MOSART component
- Now we can convert the values to monthly water budget using the values from the steps above. To be consistent with the budget table, the unit of water budget will be converted to kg/m2s*1e6:

```matlab
R = 6.37122e6; % earth radius (m)
area = 4*pi()*R^2; % earth surface area (m2)
rof_beginS = 0 % initial volume (million m3)
rof_endS = 491655.966962 % final volume (million m3)
sum_other = -2023.689434 % total other volume (million m3)
days = 31 

budget = (rof_endS - rof_beginS - sum_other)/area/(days*24*3600)*1e15; % kg/m2s*1e6
```

  Using the values we extract from the first month above, we can calculate the `budget` is **0.361338907794895**

### 6. Compare it with the value from the coupler budget table

From the coupler budget table of the same month (the one showed in the introduction section), the value is  **0.36133891**, equals to the value calculated from the MOSART component. The check is passed!


