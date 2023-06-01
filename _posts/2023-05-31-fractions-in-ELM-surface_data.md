---
layout: post
title: Fractions in ELM surface data
date: 2023-05-31 1:19
author: Tian
comments: true
categories: [Programming, E3SM, NetCDF]
tags: Null
---
Understanding fractions in the ELM V1 (CLM 4.5) surface and land use data can be very confusing. Part of this complexity may comes from ambiguous fraction variable naming. The prefix "PCT_"—presumably short for "percentage"—might lead one to expect that these variables add up to 100. However, that is not always the case. Because some PCTs relate to land units, while others correspond to Plant Functional Types (PFTs) or Crop Functional Types (CFTs). Moreover, while some PCTs in the landuse data remain constant, others fluctuate over time. After several rounds of investigation and deliberation, I have decided to write down these observations for future reference.

### Surface data
Let's begin with the surface data. Both E3SM v1 and v2 water cycle simulations use default ELM surface data with 17 Plant Functional Types (PFTs) representing vegetation. In this surface data, the sum of the following land unit fractions equals 100:

`PCT_GLACIER + PCT_URBAN + PCT_LAKE + PCT_WETLAND + PCT_NATVEG = 100`

If the surface data includes a crop land unit—such as the 15 PFT + 2 CFT (Crop Functional Type) configuration employed in E3SM v2 BGC simulations—then the formula alters slightly:

`PCT_GLACIER + PCT_URBAN + PCT_LAKE + PCT_WETLAND + PCT_NATVEG + PCT_CROP = 100`

It's crucial to note that **`PCT_URBAN`** possesses an additional dimension (`numurbl = 3`), signifying three building density classes: high, medium, and low. For the urban land unit, all these fractions must be summed.

In the case of 17 PFT surface data, **`PCT_NAT_PFT`** symbolizes the 17 plant functional types. It has a dimension of `natpft=17`, and their sum should equate to 100. These 17 PFTs are defined as follows in the source code:

```fortran
  !   0  => not vegetated
  !   1  => needleleaf evergreen temperate tree
  !   2  => needleleaf evergreen boreal tree
  !   3  => needleleaf deciduous boreal tree
  !   4  => broadleaf evergreen tropical tree
  !   5  => broadleaf evergreen temperate tree
  !   6  => broadleaf deciduous tropical tree
  !   7  => broadleaf deciduous temperate tree
  !   8  => broadleaf deciduous boreal tree
  !   9  => broadleaf evergreen shrub
  !   10 => broadleaf deciduous temperate shrub
  !   11 => broadleaf deciduous boreal shrub
  !   12 => c3 arctic grass
  !   13 => c3 non-arctic grass
  !   14 => c4 grass
  !   15 => c3_crop
  !   16 => c3_irrigated
```

For the 15 PFT + 2 CFT surface data, the last two PFTs are converted into CFTs. Consequently, **`PCT_NAT_PFT`** represents only 15 PFTs, which must also total 100. There is a new variable, **`PCT_CROP`**, with a dimension of `cft = 2`. This variable represents the two crop types, c3 rainfed crop and c3 irrigated crop, which should also sum to 100.

Some data has additional CFTs, such as one used in CLM 4.5 which has 15 PFTs and 10 CFTs. The additional crop types are defined as follows in the source code:

```fortran
  !   17 => corn
  !   18 => irrigated corn
  !   19 => spring temperate cereal
  !   20 => irrigated spring temperate cereal
  !   21 => winter temperate cereal
  !   22 => irrigated winter temperate cereal
  !   23 => soybean
  !   24 => irrigated soybean
```

### Landuse data
For the landuse data, it is improtant to know that some fractions remain constant over the time. These fraction variables are not nessassary to be included in the landuse data but I see in many cases people still add some or all of them to the landuse data for references. These constant variables are

```fortran
    ! This parameter specifies the order in which landunit areas are decreased when the
    ! specified areas add to greater than 100%. Landunits not listed here can never be
    ! decreased unless the forcings say they should be decreased. In particular, note
    ! that istice_mec doesn't appear here, so that the istice_mec area always will match
    ! the areas specified by GLC. In general, the code will NOT be robust if more than
    ! one landunit is excluded from this list. Meaning: since istice_mec is excluded from
    ! this list, all other landunits should appear in this list!
    integer, parameter :: decrease_order(8) = &
         (/istsoil, istcrop, isturb_md, isturb_hd, isturb_tbd, istwet, istdlak, istice/)
```


