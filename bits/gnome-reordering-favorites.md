---
title: "Gnome Reordering Favorites"
created: 2021-07-06 09:25:14
---

tags: #devops
keywords: gnome, favorites, dock, docking, order

# Gnome Reordering Favorites

## Get Gnome favorites example

```bsh
gsettings get org.gnome.shell favorite-apps
```

  Output:

```bash
['firefox.desktop', 'code-insiders.desktop', 'google-chrome-unstable.desktop', 'org.gnome.Nautilus.desktop', 'appimagekit-pcloud.desktop', 'keepass.desktop', 'org.gnome.Boxes.desktop', 'anki.desktop', 'kitty.desktop']

```

## Set Gnome favorites in order example

```bash
gsettings set org.gnome.shell favorite-apps "['kitty.desktop', 'firefox.desktop', 'code-insiders.desktop', 'org.gnome.Nautilus.desktop', 'org.gnome.Boxes.desktop', 'anki.desktop', 'google-chrome-unstable.desktop', 'keepass.desktop'
```
