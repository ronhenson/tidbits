---
title: "Gnome Setting Screen Idle Delay"
created: 2021-08-26 14:55:28
---

tags: #devops
keywords: gnome, settings, config

# Gnome Setting Screen Idle Delay

Gnome settings gui only provides maximum of `15 minutes` or `never` idle-delay before blanking screen.

To set more, time from the command line set idle-delay in seconds

Following example sets idle-delay to one hour.

```bash
gsettings set org.gnome.desktop.session idle-delay 3600
```
