---
title: "Vm Kvm Qemu Change Storage Pool"
created: 2021-09-02 11:38:20
tags: vm
keywords: vm, kvm, qemu, virtual machine
# Vm Kvm Qemu Change Storage Pool

---
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

## list details of default pool

```bash
virsh pool-info default
Name:           default
UUID:           c0e50315-ecf9-4892-83b9-976a16ae25fd
State:          running
Persistent:     yes
Autostart:      yes
Capacity:       931.51 GiB
Allocation:     269.19 GiB
Available:      662.32 GiB
```

## display the path of the default storage pool using `grep`

```bash
virsh pool-dumpxml default | grep -i path
    <path>/home/ronh/data/vm</path>
```

## list all the existing VM images that are stored in the default storage pool

```bash
virsh vol-list default | grep "/home/ronh/data/vm"
 dev10-win-vol2.gcow2           /home/ronh/data/vm/dev10-win-vol2.gcow2
 dev10-win.gcow2                /home/ronh/data/vm/dev10-win.gcow2
 dev11-win.gcow2                /home/ronh/data/vm/dev11-win.gcow2
 h50-fedora-server.gcow2        /home/ronh/data/vm/h50-fedora-server.gcow2
 h51-fedora-workstation.gcow2   /home/ronh/data/vm/h51-fedora-workstation.gcow2
 tails-amd64-4.21.iso           /home/ronh/data/vm/tails-amd64-4.21.iso
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

## OR edit the VM's XML file and change the path

The following commandopen the XML file with the default editor

```bash
sudo virsh pool-edit default
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
