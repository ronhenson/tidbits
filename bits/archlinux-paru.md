---
title: "Archlinux Paru"
created: 2021-12-11 17:09:48
tags: tags
keywords: template
---

# [Archlinux Paru](https://www.linuxfordevices.com/tutorials/linux/paru-arch-linux)

To install paru, first we need to install some dependencies with :

```bash
sudo pacman -S base-devel --needed
```

Next up we need to install paru, we need to clone the [github repository](http://git clone https://aur.archlinux.org/paru.git) with :

```bash
git clone https://aur.archlinux.org/paru.git
```

Now, we need to [build the package](https://www.linuxfordevices.com/tutorials/debian/build-packages-from-source) using the PKGBUILD file :

```bash
cd paru
makepkg -si
```

## Useful Paru Commands

| Command | Description |
| ------- | ----------- |
| `paru <target>` | Interactively search and install <target> |
| `paru` | Alias for paru -Syu |
| `paru -S <target>` | Install a specific package |
| `paru -Sua` | Upgrade AUR packages |
| `paru -Qua` | Print available AUR updates. |
| `paru -Gc <target>` | Print the AUR comments of <target> |
| `paru -Ui` | Build and install a PKGBUILD in the current directory |
