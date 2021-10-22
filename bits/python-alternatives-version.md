---
title: "Python Alternatives Version"
created: 2021-09-15 06:43:39
tags: coding, python
keywords: python, alternative, version, globally, linux
---

# Python Alternatives Version

## How to switch between Python versions on Fedora Linux

To change python version globally first check whether python alternative version is already registered by alternatives command:

```bash
alternatives --list | grep -i python
```

No output means not alternative python version is configured yet. Register the two above listed python version with alternative command.

```bash
alternatives --install /usr/bin/python python /usr/bin/python3.4 2
alternatives --install /usr/bin/python python /usr/bin/python2.7 1
```
