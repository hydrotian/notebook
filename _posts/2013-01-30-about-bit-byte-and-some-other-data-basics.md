---
layout: post
title: Bit, byte, and some other things about binary files
date: 2013-01-30 17:18
author: hydrotian
comments: true
categories: [Programming]
---
So here are some basics about the binary format in data computing and transferring.

1. Bit is the smallest unit, representing 0 or 1.

2. Seems like the smallest data storing unit is 1 byte, which is 8 bit. There are 8 slots for 0 or 1 in a byte. It can store 2^8 = 256 different integer numbers (for unsigned integer, it's 0 to 255)

3. Sometimes we need more binary slots for higher data precision. Then we have 16bit short, 32bit float, etc. 32 bit float stores data like this:
`sAAAAAAA BBBBBBBB BBBBBBBB BBBBBBBB`
where s = sign bit, encoded as 0 =&gt; positive, 1 =&gt; negative
A = 7-bit binary integer, the characteristic
B = 24-bit binary integer, the mantissa.

4. Beware of the writing and reading orders, i.e. Big or Little-Endian when reading and writing data. Different CPU follows different orders to read and write these binary data. If a float file is written by Big-Endian cpu (like IBM or something) but read by Little-Endian cpu (like Intel) you will easily get things screwed up.

5. There's a [very nice website](http://www.binaryconvert.com/index.html) to help you understand the binary file basics.