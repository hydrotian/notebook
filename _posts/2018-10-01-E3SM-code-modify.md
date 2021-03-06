---
layout: post
title: ELM code modification
date: 2018-10-01 15:19
author: Tian
comments: true
categories: [Programming, E3SM]
tags: E3SM
---
This post is just a reference for myself about how did I modify the E3SM 2-way coupling source code.

## Some important variables

1. `adjust_f`: a local variable I created to define the ratio between the total irrigation supply water passed to ELM from MOSART across the irrigated gridcells and across all gridcells. It should be between 0 and 1. 

2. `pgwgt(p)`: area weight of current pft to current grid.

3. `irrig_rate(p)`: the name of the variable is pretty much self-described. It is the irrigation rate (mm/s) needed to fulfill the water deficit in the soil layers. It's calculated in `CanopyFluxesMod.F90` as the `deficit` (irrigation demand) for each day divided by the total seconds of the irrigation time period.

4. `qflx_irrig(p)`: equal to `irrig_rate(p)`during the irrigation period (mm/s). Outside of this period, it's zero.

5. `qflx_real_irrig(p)`: Actual irrigation rate. In one-way coupling, it's always been met and equal to `qflx_irrig(p)`. In two-way coupling, it's the sum of surface water irrigation plus groundwater irrigation.  If the sum is greater than `qflx_irrig(p)`, the exceeded part will go to `qflx_over_supply(p)`.

6. `qflx_surf_irrig(p)`: original sruface water irrigation supply at the gridcell level passed from MOSART to ELM projected to the pft level by the following equation:
```fortran
qflx_surf_irrig(p) = atm2lnd_vars%supply_grc(g)/adjust_f/pgwgt(p)
```
note that this variable could be larger than the irrigation demand at the pft level. The exceeded supply will be dumped to the ground as precipitation to guarantee the water balance in `BalanceCheckMod.F90`.

7. `qflx_over_supply(p)`: The over supplied irrigation water. Here the over supply will be add to the surface water irrigation to reduce the withdrawal of the groundwater if there's any. In another word, the groundwater withdrawal will be less than the `ldomain%f_grd(g)*qflx_irrig(p)` if there's over supply.

8. `qflx_over_supply_col`, `qflx_surf_irrig_col`,`qflx_grnd_irrig_col`: These are calculated around line 524 for the balance check purposes. Since the balance check is done at column level, these variables are projected from pft level to column level.

## surfrdMod.F90

Around **line 650** there are about 10 lines related to the surface water only or both surface ground, as well as the `ldomain%firrig` values. If all commented out, it would be surface water only, with `ldomain%firrig` = 0.7.

## CanopyHydrologyMod.F90

This is the core file where the irrigation is applied. 

#### Actual irrigation calculation

Around **line 456** there are a few lines about how to calculate the actual irrigation `qflx_real_irrig(p)`. When it's onw-way coupling, it equals to the demand `qflx_irrig(p)`. If two-way coupling, it's the sum of the surface water and groundwater irrigation. In either case, this vaule cannot be greater than the demand `qflx_irrig(p)`.

#### Irrigation application

If it's two way coupling, then around **line 464** there's
```fortran
qflx_prec_grnd_rain(p) = qflx_prec_grnd_rain(p) + qflx_real_irrig(p) + qflx_over_supply(p)
```
If it's one way coupling, then around **line 486**
```fortran
qflx_prec_grnd_rain(p) = qflx_prec_grnd_rain(p) + qflx_irrig(p)
```

## CanopyFluxesMod.F90

Around **line 447**, the `irrig_nsteps_per_day` is defined. This value does not have unit. It depends on the size of the time step `dtime` in second and the length of the irrigation period `irrig_length`. Currently the `irrig_length` is hard coded at **line 155** as 14400 secounds (4 hrs), starting at 6 o'clock everyday (defined by `irrig_start_time`).

Another important variable used here is the `deficit`, which is daily irrigation water need to be applied to the soil (mm). It depends on the calibratable term `ldomain%firrig(g)`.

## SoilHydrologyMod.F90

This is the code that takes care of the groundwater level in the soil. I updated the **lines 717-718** by replacing the `qflx_irrig(c)*ldomain%f_grd(g) ` with `qflx_grnd_irrig_col(c)` calculated in `CanopyHydrologyMod.F90` as these two are not the same anymore.

## WaterfluxType.F90

A number of new variables I created are defined here. Also around **lines 352 and 435**, the output variables in the netCDF file are also defined.  

## HydrologyDrainageMod.F90

I updated the `qflx_irr_demand(c)` in **line 281**. This variable is the surface water irrigation demand ready to be sent to MOSART. 

**Line 279** is commented out as the withdrawal of the local runoff is handled in MOSART, not here anymore. 
```fortran
! qflx_runoff(c) = qflx_runoff(c) - qflx_irrig(c)
```

## clm_varctl.F90

I created a flag `TwoWayCouplingFlag` to control one way or two way coupling at **line 133**. True is two way, false is one way. Need to put it in the namelist file in the future.

## BalanceCheckMod.F90

Around **line 323** I updated the equation for water balance error calculations:

```fortran
errh2o(c) = endwb(c) - begwb(c) - (forc_rain_col(c) + forc_snow_col(c) + 
qflx_floodc(c) + qflx_surf_irrig_col(c) + qflx_over_supply_col(c) - 
qflx_evap_tot(c) - qflx_surf(c)  - qflx_h2osfc_surf(c) - qflx_qrgwl(c) - 
qflx_drain(c) - qflx_drain_perched(c) - qflx_snwcp_ice(c) - 
qflx_lateral(c)) * dtime 
```

## Additional comments
- If switch between one-way and two-way, just turn the `TwoWayCouplingFlag` on and off. 
- If switch between surface only and surface + groundwater, change `surfrdMod.F90`.
