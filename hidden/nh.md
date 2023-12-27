---
layout: post
title: NH
date: 2023-10-06 1:19
author: Tian
comments: true
categories: [Blog, Unix, Git]
tags: Null
---
Recently, while managing my Jekyll blog hosted on GitHub Pages, I stumbled upon a peculiar issue. Every time I added a new blog post, the entire content would appear on the homepage instead of just the title. After some digging, I realized the problem stemmed from the way line endings were being handled. As I dove deeper, I learned how widespread this issue is for developers collaborating across different operating systems. Here's the full story and how you can prevent similar hiccups in your projects.

### The Line Ending Culprit
Windows uses a combination of a carriage return (CR) and a line feed (LF) as a line ending (denoted as CRLF), while Unix-like systems (e.g., Linux and macOS) use only a line feed (denoted as LF). This difference can lead to unexpected behavior when files are shared between different platforms, as was the case with my Jekyll blog.

### Configuring Git to Handle Line Endings
Git has a setting called core.autocrlf that helps manage the line endings:

- On Windows, you can set it to true. This converts LF endings into CRLF when you check out code:

```bash
git config --global core.autocrlf true
```
- On macOS and Linux, you can set it to input. This converts CRLF endings to LF when you commit a file:

```bash
git config --global core.autocrlf input
```
### .gitattributes: A Permanent Solution
While adjusting core.autocrlf can be helpful, a more foolproof approach is to use a .gitattributes file. This file ensures consistent line endings across all platforms, overriding individual core.autocrlf configurations:

```arduino
* text=auto
```

By implementing this, Git will handle line endings automatically for every user, regardless of their personal Git settings or operating system. This means Git will convert CRLF to LF on commit and check out.

### Checking Line Endings in Your Files
To inspect line endings, you can use:

```bash
git show HEAD:your-file-name | grep -U $'\015'
```

This command will display lines from the file that have CRLF line endings. If the command returns without any output, it means your file uses only LF line endings.

### Conclusion
The challenge of managing line endings in Git is a common one, especially when working across different platforms. By understanding the underlying issue and setting up Git appropriately, you can ensure a smooth collaboration experience and avoid unexpected display issues, like the one I encountered with my Jekyll blog.