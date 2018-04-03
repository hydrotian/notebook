---
layout: post
title: Log in to a password protected FTP
date: 2016-03-20 17:50
author: hydrotian
comments: true
categories: [Unix]
---
Some password protected FTP sites can be visited by anonymous users  but if you don't input user name and password the protected contents won't show up.

In web browser address bar, use
```bash
ftp://(user name）:（password）@（ftp_site_address)
```
In shell,
```bash
wget --user=XXX --password='XXX' ftp://ftp.XXX.edu/*
```
