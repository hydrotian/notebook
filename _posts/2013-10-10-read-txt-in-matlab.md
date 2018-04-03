---
layout: post
title: Read txt in Matlab
date: 2013-10-10 11:45
author: hydrotian
comments: true
categories: [Matlab, Programming]
---
`textread`: read whole file;

`dlmread`: set the delimiter, set the reading range;

`fgetl`: read line by line;

e.g.

```matlab
fid=fopen('xx.txt');
  for i=1:40;
    a=fgetl(fid);                             %read as string
    b(i,:)=(sscanf(a,'%f"))';             %convert to array
  end;
fclose(fid);</pre>
```
The output will be a "40*n" matrix.