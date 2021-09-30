---
title: "Vm Mount Gcow2 File"
created: 2021-09-26 09:40:18
tags: #tags
keywords: template
---

# Vm Mount Gcow2 File

- Load nbd module

```bash
sudo modprobe nbd
```

- Connect image to the first nbd device

```bash
sudo qemu-nbd -c /dev/nbd0 ~/image-file.img # ( can be a gcow2 file)
```

- Mount image

```bash
mkdir /mountpoint
sudo mount /dev/nbd0p1 /mountpoint/
```

- Un-Mount image

```bash
sudo umount /mountpoint/
```

- Disconnect image

```bash
sudo qemu-nbd -d /dev/nbd0
```
