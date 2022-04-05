---
title: "gpg symmetric syntax"
created: 2022-04-03 05:01:30
tags: devops
keywords: gpg, encryption, decryption
---

# [GPG symmetric syntax](https://tutonics.com/2012/11/gpg-encryption-guide-part-4-symmetric.html)

## AES256 Cipher


```bash
gpg --symmetric --cipher-algo AES256 file.txt
```

```bash
gpg -o filename --symmetric --cipher-algo AES256 file.txt
```

```bash
gpg -o original_file.txt -d file.txt.gpg
```
