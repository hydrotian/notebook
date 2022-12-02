---
layout: post
title: SSH configuration
date: 2022-12-02 15:19
author: Tian
comments: true
categories: [Other, Programming, E3SM]
tags: Null
---
Some HPCs such as Argonne's LCRC machines require SSH keys to get connected. It's a little tricky if you still want to view the remote directories from software like WinSCP on Windows. Here's how:

## On Mac
Mac is relativly easy. Just follow the instruction to create a public SSH key using OpenSSH and upload it (`id_rsa.pub`) to your account then you can log in from terminal or Microsoft Code. The command is
```linux
cd ~/.ssh
ssh-keygen -b 4096 -t rsa
```

## On Windows
You can follow the same way above to generate a public SSH key in windows powershell. The generated public key can be used in XShell to get you connected

But in WinSCP you have to use PUTTY to generate a private key. The instruction is [here](https://www.lcrc.anl.gov/for-users/getting-started/ssh/putty/). Basically, WinSCP has a PUTTY key generator which will generate a file (*.ppk). You will need to upload the OpenSSH style key to the account and use the *.ppk to login from WinSCP.


