---
title: "Windows 10 improving the performance guest on QEMU"
created: 2021-03-01 10:10:00
tags: #devops
keywords: vm, windows 10, qemu, kvm, boxes, tuning
---

# [Windows 10 improving the performance guest on QEMU](https://leduccc.medium.com/improving-the-performance-of-a-windows-10-guest-on-qemu-a5b3f54d9cf5)

Updated: 2021–03–26

So you’ve just followed my [GPU Passthrough on a Dell Precision 7450 and other high end laptops][GPU Passthrough on a Dell Precision 7450 and other high end laptops](:/13e79020a83a44ad8f2cf296719856bf) and you want to go a little further and reach near native performance for your games and apps. Well this guide is a collection of interventions that I used to reach great performance on my VMs.

Without tuning your VM, you may experience stuttering and high CPU usage but it may still be usable. That said, if you want to get closer to bare metal speeds, you will need to do a few more things such as using optimized drivers for disks and network adapters, using a memory balloon to reclaim memory for the host, VM enlightenment and CPU pinning.

## Step 1 — CPU mode, topology and enlightenment

The CPU mode you pick will be one of the biggest factor in CPU related performance. If you disable all CPU emulation and pass the CPU as-is to the VM using the “host-passthrough” mode then your performance will be as close to bare metal as can be for CPU bound tasks.

Note that the topology that you pass to your guest must match your physical CPU. I have a a single socket (defined with the *sockets* attribute) motherboard with a single die (defined with the *dies* attribute). It has hyper-threading enabled, so each core has two *threads* (as defined with the threads attribute). And finally the amount of core that I want to pass to the VM is configured using the cores attribute. Please note that this is significant and the amount of threads *(cores * threads / 2*7=14)* must match the vcpu and *cputune* tags that you will define later. In my case I only want to pass seven physical cores to the VM and keep one for the host. So since I’m passing a total of 14 cores I will have to set my vcpu tag to 14 later.

```xml
<domain type='kvm' ...>
  ...
  <cpu mode='host-passthrough' check='none'>
    <topology sockets='1' dies='1' cores='7' threads='2'/>
  </cpu>
  ...
</domain>
```

Enlightenments will define what features your guest will be able to support such as hibernation, sleep, hyper-v (for nested virtualization) but may also impact performance. I found that enabling some hyper-v features helped with my use case. Also I wanted to have hibernation enabled so I had to enable the right features in the *clock, pm* and *features tags*

Most importantly, when it comes to performance enabling vapic and *hypervclock* and disabling *hpet* helped with reducing idle CPU usage. Without these settings your CPU could rest in the 15% usage when your VM is running without any workload.

```xml
<domain type='kvm' ...>
  ...
  <features>
    <acpi/>
    <apic/>
    <hyperv>
      <relaxed state='on'/>
      <vapic state='on'/>
      <spinlocks state='on' retries='8191'/>
    </hyperv>
    ...
  </features>
  <pm>
    <suspend-to-mem enabled='yes'/>
    <suspend-to-disk enabled='yes'/>
  </pm>
  <clock offset='localtime'>
    <timer name='hpet' present='no'/>
    <timer name='hypervclock' present='yes'/>
  </clock>
  ...
</domain>
```

## Step 2— CPU Pinning

The easiest and most significant way to improve performance so far is to use CPU pinning in conjunction with a host-passthrough CPU mode. The process involves setting some cores to be dedicated to virtualization processes such as I/O, some for your host and others to be dedicated to your virtual machine. It is also possible to isolate the cores so that your host does not use them but I didn’t find that it was necessary so long as I didn’t run anything intensive on the host while my VMs are running.

First you have to specify the amount of I/O threads that you will be using in the *iothreads* tag. In my case a single I/O thread was sufficient. After defining how many threads are dedicated to IO you will need to assign it a matching amount of cores but we will go over how to do this later. The same goes for the threads assigned to your VM but you will be using the vcpu tag to defined how many threads the VM will have.

In my case I have a total of 16 threads on 8 physical cores (2 threads per physical cores) and I decided to reserve my first physical core for the host and other emulation tasks. As such the vcpu 0 and 8 are not assigned to the VM.

Intel CPUs have their CPUsets numerated in a peculiar way. The first virtual core of each cores are occupying the lower half of the cpuset range and the second virtual cores are occupying the upper half.

### Physical core #1

- Virtual core 1 = 0
- Virutal core 2 = 8

### Physical core #2

- Virtual core 1 = 1
- Virutal core 2 = 9

### Physical core #8

- Virtual core 1 = 7
- Virtual core 2 = 15

In my case I decided to assign the physical cores 2 to 8 to the VM using the *vcpuin* tags. And then the emulator and io threads to the first physical core using the *emulatorpin* and *iothreadpin* tags.

```xml
<domain type='kvm' ...>
 <vcpu placement='static'>14</vcpu>
 <iothreads>1</iothreads>
 <cputune>
    <vcpupin vcpu='0' cpuset='1'/>
    <vcpupin vcpu='1' cpuset='2'/>
    <vcpupin vcpu='2' cpuset='3'/>
    <vcpupin vcpu='3' cpuset='4'/>
    <vcpupin vcpu='4' cpuset='5'/>
    <vcpupin vcpu='5' cpuset='6'/>
    <vcpupin vcpu='6' cpuset='7'/>
    <vcpupin vcpu='7' cpuset='9'/>
    <vcpupin vcpu='8' cpuset='10'/>
    <vcpupin vcpu='9' cpuset='11'/>
    <vcpupin vcpu='10' cpuset='12'/>
    <vcpupin vcpu='11' cpuset='13'/>
    <vcpupin vcpu='12' cpuset='14'/>
    <vcpupin vcpu='13' cpuset='15'/>
    <emulatorpin cpuset='0,8'/>
    <iothreadpin iothread='1' cpuset='0,8'/>
 </cputune>
</domain>
```

## Step 3— Using Virtio drivers

- Using virtio drivers is a must to improve the performance of your machine. When used in combination with CPU pinning and IO threads, using the proper drivers may improve the performance of your machine overall. So far we are using an emulated network adapter and an emulated SATA disk which is not optimal for performance as emulating physical devices is much more demanding that running drivers that are optimized for virtualization.
- To gain from this change you must have configured your emulator and io threads because when you aren’t using dedicated cores, the I/O tasks may be distributed across many cores, some of which may already be busy processing tasks for your game or productivity software. We want to avoid that if possible.
- To install the virtio drivers on your guest, we will first need to configure Windows to boot in safe mode, turn off the guest, switch the disk and NIC to virtio in libvirt-manager, boot in Windows and install the drivers using the virtio driver iso. This ISO can be found here.
- To boot in safemode, run the following command on your windows guest using the command prompt:

```bsh
bcdedit /set {default} safeboot network
```

- Once you are booted in safe mode you can install the drivers by opening the Computer Management and then selecting the Device Manager. There should be several unrecognized devices. Right click, select install driver and select them from the ISO.
- Once you are done you can return to normal boot by using:

```bsh
bcdedit /deletevalue {default} safeboot
```

Using virtio drivers when possible in conjunction with CPU pinning greatly improved my overall performance and solved issues such as games writing to disk or downloading large files causing stutter.

## Step 4— Enabling TRIM support for you raw images

- To enable TRIM support select your VirtIO disk in libvirt-manager and open the Advanced Options drawer and then the Performance Options and set “Discard Mode” to “unmap”.
- This will improve performance of some tasks such as deleting lots of files. Also it will prolong the life of your SSD.
- Another way to greatly improve performance is to use a physical device such as an NVME drive which can then be passed with PCI Passthrough. In which case you do not need to enable trim support. This will greatly improve your I/O performance at the cost of not being able to clone your disk as easily, you will also lose the possibility of hibernating and snapshotting your VM.

## Step 5— Improving the boot time of your machine

Using QEMU 5 and kernel 5.6, it takes exponentially longer to boot your machine the more RAM you add. With 24 Gb passed to one of my guest I experienced wait times of 2 to 3 minutes which are unacceptable. To fix this issue it is however necessary to recompile your kernel with Preemption set to optional.

```yml
CONFIG_PREEMPT_VOLUNTARY=y
# CONFIG_PREEMPT=y
```

[The instructions on how to achieve this are outlined in this article](https://medium.com/@Leduccc/compiling-a-custom-linux-kernel-on-manjaro-f09aa103713f).

When using a custom kernel I now have boot times of around 10–15 seconds.

## Conclusion

- As you have seen there are many things that can be done to improve your guest performance. I personally stopped there because I was satisfied with my performance but there are still more steps that could be taken. Notably the use of hugepages has helped many users and the use of core isolation which prevents the hosts from running tasks on core passed to your VM.
- If you have any other suggestion do not hesitate to comment and I will your performance tuning interventions to this guide.

links
: [[mwin]]
: [[wsl-how-to-export-and-import-to-another-folder]]

[//begin]: # "Autogenerated link references for markdown compatibility"
[mwin]: mwin.md "Mwin"
[wsl-how-to-export-and-import-to-another-folder]: wsl-how-to-export-and-import-to-another-folder.md "WSL how to export and import to another folder"
[//end]: # "Autogenerated link references"
