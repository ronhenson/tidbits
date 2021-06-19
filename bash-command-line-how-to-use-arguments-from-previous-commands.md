---
title: "BASH command line how to use arguments from previous commands"
created: 20210613191710
tags: [ linux ]
---

links
: [[bash-extracting-substring]]

## [[BASH-command-line-how-to-use-arguments-from-previous-commands](https://stackoverflow.com/questions/4009412/how-to-use-arguments-from-previous-command)

```bash
!^      first argument
!$      last argument
!*      all arguments
!:2     second argument

!:2-3   second to third arguments
!:2-$   second to last arguments
!:2*    second to last arguments
!:2-    second to next to last arguments

!:0     the command
!!      repeat the previous line
```
