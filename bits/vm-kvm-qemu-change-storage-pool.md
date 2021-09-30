---
title: "Vm Kvm Qemu Change Storage Pool"
created: 2021-09-02 11:38:20
tags: #devops
keywords: vm, kvm, qemu, virtual machine
---

# Vm Kvm Qemu Change Storage Pool

## List current pools

```bash
virsh pool-list
 Name          State    Autostart
-----------------------------------
 default       active   yes
 Downloads     active   yes
 gnome-boxes   active   yes
 tails         active   yes
```

## Destroying pool

```bash
virsh pool-destroy default
Pool default destroyed
```

## Undefine pool

```bash
virsh pool-undefine default
Pool default has been undefined
```

## Defining a new pool with name "default"

```bash
virsh pool-define-as --name default --type dir --target /home/ronh/data/vm
Pool default defined
```

## Set pool to be started when libvirt daemons starts

```bash
virsh pool-autostart default
Pool default marked as autostarted
```

## Start pool

```bash
virsh pool-start default
Pool default started
```

## Checking pool state

```bash
virsh pool-list
 Name          State    Autostart
-----------------------------------
 default       active   yes
 Downloads     active   yes
 gnome-boxes   active   yes
 tails         active   yes
```
