---
title: "Vim Neovim installing latest release on Fedora"
created: 2021-06-13 19:34:51
tags: [vim]
---

# Vim Neovim installing latest release on Fedora

# Download atest `nvim-appimage` to `~/bin`

I extract since nvim extension for vscode didn't like the appimage

```bsh
cd ~/bin
chmod 740 ./nvim-appimage
./nvim-appimage --version # make sure I have the version expected 
./nvim.appimage -- appimage-extract
# create a folder squashfs-root
# save existing 
mv neovimfs to_output_name
mv squashfs-root neovimfs
# Might want to check permissions
# Done
```
