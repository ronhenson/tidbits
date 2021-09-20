---
title: "VM Tail Tor Os Install"
created: 2021-08-19 18:23:45
tags: #devops
keywords: vm, tor, virtual machine
---

# [VM Tail Tor Os Install](https://fedoratips.wordpress.com/2013/05/29/run-tails-in-a-virtual-machine-for-anonymous-internet-access/)

Utilize tails through the onion / tors network.  Does not write anything to disk

- [Download tails iso](https://tails.boum.org/install/vm-download/index.en.html)

```bash
qemu-kvm -m 2048 -cdrom /data/vm/tails/tails-amd64-4.21.iso
```
