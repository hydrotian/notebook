---
layout: post
title: Compile FORTRAN in Eclipse with netcdf
date: 2017-11-14 02:26
author: hydrotian
comments: true
categories: [Programming]
---
I like interpreted languages such as Matlab or Python, mostly because I'm more like a software user, not developer. These languages usually come with an Integrated Development Environment (IDE) for easy code editing and debugging. However, for compiled languages such as Fortran, there isn't many choices for IDEs but [Eclipse](https://www.eclipse.org/) has some potential to be a good alternative.

##### Steps to compile a Fortran program in Eclipse, in Windows system, using NetCDF library:
- Eclipse is a programming platform. To compile Fortran code in Eclipse, a compiler is still needed. I chose cygwin gfortran as it's easy to configure by the installation wizard.
- After installing gfortran in cygwin, install netcdf in cygwin too.
- Create a new Fortran project in Eclipse and import the code files.
- In properties for the project, select "internal builder" under "Fortran Build", then in "Tool Chain Editor" choose "cygwin GCC" and "CDT internal builder". In "Select Tools", add "GNU Fortran Compiler" and "GNU Fortran Linker"
- In "Settings", define Include locations. Under "GNU Fortran Compiler" -&gt; "Directorories", add Include paths (-I) as "usr/include" (assuming the NetCDF is installed in this directory"
- Same place, define the NetCDF library locations. Under "GNU Fortran Linker" -&gt; "Libraries", add "netcdff" for "-l" and "usr/lib" for "-L"
- Try compile the code. Basically the command is something like

```bash
gfortran XX.f90 -L/usr/lib -lnetcdff -I/usr/include
```
##### Important Notes:
- 1) may need to change file suffix from `F90` to `f90` .
- 2) put the file name `XX.f90` after `gfortran`, followed by `-L, -l, and -I`. I tried to put -L first, won't work.
- 3) the paths `/usr/lib` and `/usr/include` are "sudo" Linux paths used in cygwin system. Do not use the absolute windows path (e.g. C:\cygwin64\usr\include) to compile the code. Won't work.
- 4) installing NetCDF library yourself (instead of configuring in cygwin) might be a pain. But this [video](https://www.youtube.com/watch?v=psK7wdBo_SU) might help.