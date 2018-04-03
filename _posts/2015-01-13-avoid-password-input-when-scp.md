---
layout: post
title: Avoid password input when scp using "expect"
date: 2015-01-13 23:54
author: hydrotian
comments: true
categories: [Unix]
---
Sometimes I need to scp files from remote server, but I hate to input the (fixed) password over and over again. Here is a solution using "expect"

```bash
#!/usr/bin/expect
set name [linde $argv 0] # read name from argument
spawn scp id@server1:/data/location /data/destination # scp
expect &quot;*password:*&quot; {send &quot;your password&quot;}
interact
```

