---
layout: post
title: Simple way to estimate a grid cell area based on latitude
date: 2014-12-01 16:50
author: hydrotian
comments: true
categories: [Other]
---
Sometimes I need to convert some flux terms from length unit to volume. For example, 10 mm precipitation over a 0.25 degree grid cell is ? m3 water.The only unknown term here is the area of the grid cell. One easy way to approximate it is to treat this grid cell as a flat rectangle (this problem could be extremely complex if all the details are considered). The length of the latitudinal side remains the same no matter where the grid cell is, but the length of the longitudinal side varies with changing latitude (i.e. largest at equator, zero at poles).

Anyway, the area is
`a^2*cos(alpha)`
where **a** is the side length when the gird cell is at equator

**alpha** is the latitude (degree)
