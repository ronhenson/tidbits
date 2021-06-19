---
title: "Fedora dnf installing deb packages"
created: 20210613190546
tags: [ linux, devops ]
---

links
: [[placeholder]]

# Fedora dnf installing deb packages

## Convert .deb to .rpm **Doesn't always work**

```bsh
sudo dnf install alien
alien -r package.deb
sudo dnf install package.rpm
```
