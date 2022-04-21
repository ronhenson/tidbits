---
title: "Vm Assigning Ip Address Dhcp"
created: 2021-11-26 20:08:25
tags: vm
keywords: virsh, virtual, machine, vm, ip address
---

# Vm Assigning Ip Address Dhcp

[assigning QEMU VMs static IPs with libvirt](http://www.ebadf.net/2012/03/03/libvirt-static-ip/)


```bash
sudo virsh net-dumpxml default
<network>
  <name>default</name>
  <uuid>53d5f56a-3767-41b1-96a3-5d4df7134c70</uuid>
  <forward mode='nat'>
    <nat>
      <port start='1024' end='65535'/>
    </nat>
  </forward>
  <bridge name='virbr0' stp='on' delay='0'/>
  <mac address='52:54:00:eb:bf:7e'/>
  <ip address='192.168.122.1' netmask='255.255.255.0'>
    <dhcp>
      <range start='192.168.122.100' end='192.168.122.254'/>
      <host mac='52:54:00:f1:4a:7f' name='fedora-server' ip='192.168.122.50'/>
      <host mac='52:54:00:29:de:e3' name='win11' ip='192.168.122.10'/>
    </dhcp>
  </ip>
</network>
```

```bash
sudo virsh net-list --all
 Name      State    Autostart   Persistent
--------------------------------------------
 default   active   yes         yes
```

```bash
sudo virsh net-dhcp-leases default
 Expiry Time           MAC address         Protocol   IP address           Hostname        Client ID or DUID
-----------------------------------------------------------------------------------------------------------------
 2021-11-26 20:49:17   52:54:00:03:5d:d6   ipv4       192.168.122.234/24   h03             01:52:54:00:03:5d:d6
 2021-11-26 20:19:13   52:54:00:f1:4a:7f   ipv4       192.168.122.50/24    fedora-server   01:52:54:00:f1:4a:7f
```

```bash
sudo virsh net-update default add ip-dhcp-host "<host mac='52:54:00:03:5d:d6' ip='192.168.122.51' />" --live --config
Updated network default persistent config and live state
```

```bash
sudo virsh net-dumpxml default
<network>
  <name>default</name>
  <uuid>53d5f56a-3767-41b1-96a3-5d4df7134c70</uuid>
  <forward mode='nat'>
    <nat>
      <port start='1024' end='65535'/>
    </nat>
  </forward>
  <bridge name='virbr0' stp='on' delay='0'/>
  <mac address='52:54:00:eb:bf:7e'/>
  <ip address='192.168.122.1' netmask='255.255.255.0'>
    <dhcp>
      <range start='192.168.122.100' end='192.168.122.254'/>
      <host mac='52:54:00:f1:4a:7f' name='fedora-server' ip='192.168.122.50'/>
      <host mac='52:54:00:29:de:e3' name='win11' ip='192.168.122.10'/>
      <host mac='52:54:00:03:5d:d6' ip='192.168.122.51'/>
    </dhcp>
  </ip>
</network>
```
