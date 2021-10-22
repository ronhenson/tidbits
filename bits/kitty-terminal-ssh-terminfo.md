---
title: "Kitty Terminal Ssh Terminfo"
created: 2021-07-12 13:10:39
tags: devops, terminal
keywords: ssh, backspace key, no echo, keyboard
---

# Kitty Terminal Ssh Terminfo

When ssh into another box from `kitty` terminal, the backspace keys, arrow keys don't echo as should.

```bash
kitty +kitten ssh ronh@NakedWonderland
```

This will insert a .terminfo file in the home directory.  As lon as it is there, everything will work correctly.
