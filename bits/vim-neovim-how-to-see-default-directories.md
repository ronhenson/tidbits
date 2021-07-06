---
title: "Vim Neovim how to see default directories"
created: 2021-06-13 19:36:33
tags: #ide
keywords: vim, neovim, nvim, editor, edit
---
# Vim Neovim how to see default directories

`nvim =u NONE`

then

`:echo &rtp`

# To see all default directories before init script

`nvim --cmd "echo &rtp"`
