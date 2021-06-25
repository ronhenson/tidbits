---
title: "Fedora dnf installing deb packages"
created: 2021-06-13 19:05:46
tags: [linux, devops]
---

# Fedora dnf installing deb packages

## Convert .deb to .rpm **Doesn't always work**

```bsh
sudo dnf install alien
alien -r package.deb
sudo dnf install package.rpm
```
