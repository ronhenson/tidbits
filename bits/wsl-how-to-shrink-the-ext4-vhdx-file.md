---
title: "WSL how to shrink the ext4 vhdx file"
created: 2021-06-13 18:52:22
tags: devops
keywords: wsl, windows 10, microsoft, vhdx
---

# WSL how to shrink the ext4 vhdx file

# **Make sure the wsl is not running**

## Should backup before starting

```powershell
wsl.exe --list --verbose   #  ffind out what is running
wsl.exe --terminate <name> # will stop if needed
```

```powershell
diskpart.exe
DISKPART> select vdisk file="D:\wsl\ubuntu-2\ext4.vhdx"
DISKPART> compact vdisk
  100 percent completed
Diskpart succesfully compacted the virtual disk file.
DISKPART>exit
```

links
: [[mwin]]

[//begin]: # "Autogenerated link references for markdown compatibility"
[mwin]: mwin.md "Mwin"
[//end]: # "Autogenerated link references"
