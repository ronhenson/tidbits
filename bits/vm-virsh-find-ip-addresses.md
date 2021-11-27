---
title: "Vm Virsh Find Ip Addresses"
created: 2021-11-26 19:24:29
tags: vm
keywords: virtual, machine, vm, ip, addr, address, virsh
---

# Vm Virsh Find Ip Addresses

[How to find the IP address of a kvm virtual machine](https://ostechnix.com/how-to-find-the-ip-address-of-a-kvm-virtual-machine/)

## Net-dhcp-leases

```bash
sudo virsh net-list
# Sample output
Name      State    Autostart   Persistent
--------------------------------------------
 default   active   yes         yes
```

```bash
virsh net-info default
# Sample output
Name:           default
UUID:           ce25d978-e455-47a6-b545-51d01bcb9e6f
Active:         yes
Persistent:     yes
Autostart:      yes
Bridge:         virbr0
```

```bash
virsh net-dhcp-leases default
#output
Expiry Time           MAC address         Protocol   IP address           Hostname        Client ID or DUID
-----------------------------------------------------------------------------------------------------------------
 2021-11-26 20:19:17   52:54:00:03:5d:d6   ipv4       192.168.122.234/24   h03             01:52:54:00:03:5d:d6
 2021-11-26 19:51:54   52:54:00:29:de:e3   ipv4       192.168.122.10/24    win11           01:52:54:00:29:de:e3
 2021-11-26 20:19:13   52:54:00:f1:4a:7f   ipv4       192.168.122.50/24    fedora-server   01:52:54:00:f1:4a:7f
```

## domifaddr

```bash
virsh list
# output
 Id   Name                     State
----------------------------------------
 3    h03-fedora-workstation   running
```

```bash
sudo virsh list
# output
 Id   Name                     State
----------------------------------------
 3    h03-fedora-workstation   running
```

```bash
sudo virsh domifaddr h03-fedora-workstation
# output
 Name       MAC address          Protocol     Address
-------------------------------------------------------------------------------
 vnet2      52:54:00:03:5d:d6    ipv4         192.168.122.234/24
```

## Using arp command

```bash
arp -n
#output
192.168.122.234          ether   52:54:00:03:5d:d6   C                     virbr0
```
