---
layout: post
title: Interactive Maps for E3SM Streamflow Diagnostics
date: 2024-05-02 1:19
author: Tian
comments: true
categories: [Plotting, E3SM]
tags: Null
---
[E3SM_diags](https://github.com/E3SM-Project/e3sm_diags) is a robust tool for E3SM output visulizations. However, current (v2) support for streamflow (MOSART) output is limited to [annual mean flow and seasonality index](https://docs.e3sm.org/e3sm_diags/_build/html/main/available-parameters.html#set-specific-parameters). 

Here I'd like to introduce a new tool - [E3SM Streamflow Diags](https://github.com/hydrotian/E3SM_Streamflow_Diags) for interactive visulization for E3SM river outputs. This tool is specifically designed to compare the seasonal cycles of E3SM streamflow outputs with observations at global river gauges. Hopefully it can be integrated into future E3SM-diags releases. The key features of this tool include:

- Automatic Data Extraction and Georeferencing: Automatically extracts streamflow data from E3SM raw output files and georeferences gauge locations to corresponding latitude and longitude grid cells. This process includes comparing drainage areas between the model output and gauge data.
- Interactive Map Visualization: Displays gauge locations on a [html-based interactive map](https://portal.nersc.gov/cfs/e3sm/tizhou/2024_Tutorial//extendedOutput.v3.LR.historical_0101/extendedOutput.v3.LR.historical_0101.html). The gauges are represented by color-coded dots that indicate the goodness-of-fit (default metric: Nash-Sutcliffe Efficiency, NSE) and are sized according to the magnitude of discharge. I use the [Esri Hydro Reference Overlay](https://www.arcgis.com/home/item.html?id=9f86716d941c4410b0b406d911754b2c) as my basemap, where the major river names are highlighted for users to varify the gauge names.
- Detailed Time Series Analysis: By clicking on any gauge dot on the map, users can access pre-generated time series plots that compare simulated streamflow against observational data.
- Comparative Analysis: Allows users to select two different simulations for [direct intercomparison](https://portal.nersc.gov/cfs/e3sm/tizhou/2024_Tutorial//comparisons/GRFR_vs_extendedOutput.v3.LR.historical_0101.html). In this feature, the gauge dots are color-coded to indicate the difference in performance (NSE) between the two simulations.
- Viewer page generation: Allows users to automatically generate a [summary HTML page](https://portal.nersc.gov/cfs/e3sm/tizhou/2024_Tutorial.viewer.html) that lists links to all diagnostic pages for easy navigation.
