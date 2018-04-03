---
layout: post
title: Link and Copy
date: 2013-10-11 13:03
author: hydrotian
comments: true
categories: [Programming, Unix]
---
`ln -s` creates links to certain files

`cp` copys the original file while `cp-d` copys the link

```bash
$ ln -s file1 file2              #generates a link (file2) links to file1
$ cp file2 file3                 #copys the original file (file1) to file3
$ cp -d file2 file3              #copys the link file (file2) to file3
```
