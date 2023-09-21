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

It's crucial to note that **PCT_URBAN** possesses an additional dimension (`numurbl = 3`), signifying three building density classes: high, medium, and low. For the urban land unit, all these fractions must be summed.

In the case of 17 PFT surface data, **PCT_NAT_PFT** symbolizes the 17 plant functional types. It has a dimension of `natpft=17`, and their sum should equate to 100. These 17 PFTs are defined as follows in the source code:

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

For the 15 PFT + 2 CFT surface data, the last two PFTs are converted into CFTs. Consequently, **PCT_NAT_PFT** represents only 15 PFTs, which must also total 100. There is a new variable, **PCT_CFT**, with a dimension of `cft = 2`. This variable represents the two crop types, c3 rainfed crop and c3 irrigated crop, which should also sum to 100.

Some data has additional CFTs, such as one used in later vision of ELM which has 15 PFTs and 36 CFTs. The additional crop types are defined as follows in the source code:

```fortran
17 -> corn                               
18 -> irrigated_corn                     
19 -> spring_temperate_cereal            
20 -> irrigated_spring_temperate_cereal  
21 -> winter_temperate_cereal            
22 -> irrigated_winter_temperate_cereal  
23 -> soybean                            
24 -> irrigated_soybean
25 -> cassava                            
26 -> irrigated_cassava                  
27 -> cotton                             
28 -> irrigated_cotton                   
29 -> foddergrass                        
30 -> irrigated_foddergrass              
31 -> oilpalm                            
32 -> irrigated_oilpalm                  
33 -> other_grains                       
34 -> irrigated_other_grains             
35 -> rapeseed                           
36 -> irrigated_rapeseed                 
37 -> rice                               
38 -> irrigated_rice                     
39 -> root_tubers                        
40 -> irrigated_root_tubers              
41 -> sugarcane                          
42 -> irrigated_sugarcane                
43 -> miscanthus                         
44 -> irrigated_miscanthus               
45 -> switchgrass                        
46 -> irrigated_switchgrass              
47 -> poplar                             
48 -> irrigated_poplar                   
49 -> willow                             
50 -> irrigated_willow  
```

### Landuse data

In the first time step of January 1 of a transient run, changes in land unit weights can potentially come from two sources: Changes in the area of the crop land unit come from the landuse dataset, and changes in the area of the glacier land unit come from the ice sheet model. The areas of other land units are then adjusted so that the total landunit area remains 100%. Since we don't usually turn on the ice sheet model in most of our simulations, the glacier fraction remains constant. Therefore the only source of the area weight change comes from the crop fraction (i.e. **PCT_CROP**). If the total land unit area of crops has decreased, then the natural vegetated landunit(i.e. **PCT_NATVEG**) is increased to fill in the abandoned land. If the total land unit area of glaciers and crops has increased, then other land unit areas are decreased in a specified order until the total is once again 100%. The order of decrease is: natural vegetation, crop, urban medium density, urban high density, urban tall building district, wetland, lake.

Down below is the comments in the source code explaining the **decrease_order** variable:

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

Reference: [CLM5 Technical Note Chapter 27](https://escomp.github.io/ctsm-docs/versions/release-clm5.0/html/tech_note/Transient_Landcover/CLM50_Tech_Note_Transient_Landcover.html#)
