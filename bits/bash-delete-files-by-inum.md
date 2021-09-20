---
title: "Bash Delete Files By Inum"
created: 2021-08-09 10:52:32
tags: #shell, #devops
keywords: bash, find
---

# Bash Delete Files By Inum

if in a pickle where cannot access or delete a file because of characters in the filename, Can delete by using `inum`

```Bash
ls -li
find . -inum 1234567 -delete

or 

find . -inum 1234567 -exec rm -i {} ;
```
