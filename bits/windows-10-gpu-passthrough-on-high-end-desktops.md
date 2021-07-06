---
title: "Windows 10 GPU passthrough on high end laptops"
created: 2021-06-14 06:14:50
tags: #devops
keywords: vm, windows 10, linux, kvm, qemu, boxes
---

links
: [[windows-10-improving-the-performance-guest-on-qemu]]

# [Windows-10-GPU-passthrough-on-high-end-laptops][1]

```text
  Specifically on a Dell Precision 7450 (May 2020)
```

The purpose of this blog post is to provide a collated guide for the setup of a graphically accelerated virtual machine for the purpose of gaming or productivity on a Windows 10 Guest. The host machine is running Manjaro Linux and QEMU/KVM wrapped by libvirt as a virtualization provider.

In this setup we will pass the laptop’s Quadro T1000 to the virtual machine and leave the iGPU for the host. In a later guide we will see about dynamically binding GPU to the host and the guest using bumblebee so that we can use external monitors with both OSes.

This setup is meant to work on an external monitor and has only been tested using HDMI output but should work with all output ports.
Steps Summary

    Setup IOMMU
    Isolate graphics
    Configure the virtual machine
    Install the guest operating system
    Correct code 31 & code 43
    Optional performance tuning

## Prerequisites

You must have installed QEMU and all of its dependencies

```
$ sudo pacman -S qemu libvirt edk2-ovmf virt-manager ebtables dnsmasq
```

### Step 1 — Setup IOMMU

[See this link for a detailed explanation][2]

First we need to establish if IOMMU is enabled on your machine:

```
# Check whether IOMMU is enable on your system
$ sudo dmesg | grep -i -e DMAR -e IOMMU[    0.000000] ACPI: DMAR 0x00000000BDCB1CB0 0000B8 (v01 INTEL  BDW      00000001 INTL 00000001)
[    0.000000] Intel-IOMMU: enabled
...
```

If there is no output related to IOMMU as a result of running this command then you need to enable IOMMU as a kernel parameter

```
# Edit your grub file
$ sudo nano /etc/default/grub# Locate the line containing
GRUB_CMDLINE_LINUX_DEFAULT="..."# Add the following value like such
GRUB_CMDLINE_LINUX_DEFAULT="... intel_iommu=on ..."# Save the file, quit nano and update grub
$ sudo update-grub# Reboot
$ sudo reboot now
```

Save the following script to `~/Documents/iommu.sh` (or anywhere that suits your purpose)

```
#!/bin/bash
shopt -s nullglob
for g in /sys/kernel/iommu_groups/*; do
    echo "IOMMU Group ${g##*/}:"
    for d in $g/devices/*; do
        echo -e "\t$(lspci -nns ${d##*/})"
    done;
done;
```

Make it executable and execute it from your home

```
# From your home, execute
$ ./Documents/iommu.sh
IOMMU Group 10:
  00:1d.0 USB controller: Intel Corporation 7 Series/C210 Series Chipset Family USB Enhanced Host Controller #1 [8086:0e26] (rev 04)
IOMMU Group 13:
  06:00.0 VGA compatible controller: NVIDIA Corporation GM204 [GeForce GTX 970] [10de:13c2] (rev a1)
  06:00.1 Audio device: NVIDIA Corporation GM204 High Definition Audio Controller [10de:0fbb] (rev a1)
```

If you see several groups then your machine is correctly configured. Now locate your graphics card IOMMU group and note the IDs of the audio device and the card itself (`ex: 10de:13c2`).

Here is my output as an example:

```
IOMMU Group 1:
 00:01.0 PCI bridge [0604]: Intel Corporation Xeon E3-1200 v5/E3-1500 v5/6th Gen Core Processor PCIe Controller (x16) [8086:1901] (rev 0d)
 01:00.0 VGA compatible controller [0300]: NVIDIA Corporation TU117GLM [Quadro T1000 Mobile] [10de:1fb9] (rev a1)
 01:00.1 Audio device [0403]: NVIDIA Corporation Device [10de:10fa] (rev a1)
```

Here the id of the audio device is `10de:10fa` and the graphics card is `10de:1fb9` but they will differ if you have a different configuration.

Note that there is a third device which is the PCI Bridge but it shouldn’t be passed to the VM or isolated

## Step 2 — Isolating the GPU

Now we need to make sure that the operating system doesn’t load the GPU and that the card is free to be passed to the VM. Right now we will bind it to vifo-pci very early in the boot process as it is a lot simpler then configuring bumblebee and makes it easier to troubleshoot issues. That said, in a follow up blog, we will make it so we can share the GPU between the host and guest when it is not in use.

First we need to add the following kernel parameter to GRUB (make sure to update it with your device ids)

```
vfio-pci.ids=10de:10fa,10de:1fb9
```

Now add this in ``/etc/default/grub GRUB_CMDLINE_LINUX_DEFAULT`

```
# Edit your grub file
$ sudo nano /etc/default/grub# Add the following value like such
GRUB_CMDLINE_LINUX_DEFAULT="... vfio-pci.ids=10de:10fa,10de:1fb9 intel_iommu=on ..."# Save the file, quit nano and update grub
$ sudo update-grub
```

Now we need to tell the kernel to load vfio-pci before nouveau (which is the module that binds the GPU to the host).

```
$ sudo nano /etc/mkinitcpio.conf# Locate the line that sais
MODULES=(...)# And add those values (Make sure that nouveau is after vfio)
MODULES=(... vfio_pci vfio vfio_iommu_type1 vfio_virqfd nouveau ...)
```

Now we need to update the initrd image that GRUB uses to take those values in account:

```
# Check the name of the image that GRUP loads
$ sudo update-grub
Generating grub configuration file ...
Found theme: /usr/share/grub/themes/manjaro/theme.txt
Found linux image: /boot/vmlinuz-5.6-x86_64
Found initrd image: /boot/intel-ucode.img /boot/initramfs-5.6-x86_64.img
Found initrd fallback image: /boot/initramfs-5.6-x86_64-fallback.img
Found Manjaro Linux (20.0) on /dev/nvme0n1p3
Found memtest86+ image: /boot/memtest86+/memtest.bin
done
```

Since grub loads `/boot/initramfs-5.6-x86_64.img` as shown in the previous command output we will update this image:

```
$ sudo mkinitcpio -c /etc/mkinitcpio.conf -g /boot/initramfs-5.6-x86_64.img
==> Starting build: 5.6.11-1-MANJARO
  -> Running build hook: [base]
  -> Running build hook: [udev]
  -> Running build hook: [autodetect]
  -> Running build hook: [modconf]
  -> Running build hook: [block]
  -> Running build hook: [keyboard]
  -> Running build hook: [keymap]
  -> Running build hook: [encrypt]
  -> Running build hook: [openswap]
  -> Running build hook: [resume]
  -> Running build hook: [filesystems]
==> Generating module dependencies
==> Creating gzip-compressed initcpio image: /boot/initramfs-5.6-x86_64.img
==> Image generation successful
```

At this point we are ready to reboot and confirm that our GPU was correctly bound by the vfio-pci driver:

```
$ sudo reboot now
...# After reboot
$ sudo lspci -nnk
01:00.0 VGA compatible controller [0300]: NVIDIA Corporation TU117GLM [Quadro T1000 Mobile] [10de:1fb9] (rev a1)
 Subsystem: Dell TU117GLM [Quadro T1000 Mobile] [1028:0926]
 Kernel driver in use: vfio-pci
 Kernel modules: nouveau
```

If the kernel driver in use is `vfio-pci` then the GPU is correctly isolated.

## Step 3 — Setup of the virtual machine and guest

Basically you need to carefully follow the steps provided in this wiki but I will copy them here in case they get delete, moved or changed eventually.

If you are asked to create a default network, do it and leave it on the default option. Make sure to set it to autostart if you are given the option, otherwise you can do it later.

The most important part is to check the “Customize before install” before clicking “Finish” in virtual manager and setting the firmware to UEFI in the “Overview” panel
Make sure that the “Customize configuration before install is checked”
Make sure that you selected a UEFI Firmware

If you fail to select a UEFI then you will get a Code 43

Once that is done you must attach every device in the IOMMU group of your GPU otherwise the VM will refuse to start. You do not need to remove the Spice and Video QXL at this time as it will make it easier to diagnose issues until your card is recognized and has the correct drivers.

You can now proceed with the windows installation or configuration if you selected an existing or physical Windows installation

## Step 4 — Install Windows 10

Simply install windows as you normally would using the spice RDP. Once you have booted windows after the installation you can then connect your external monitor.

## Step 5— Correct code 31 and code 43

### Step 5.1 Correct code 31


Out of the box the GPU will have the incorrect subsystem vendor and devie ids. For a reason yet unknown it is necessary to assign the Intel iGPU subsystem Ids to the card in order for it to be recognized. If you know why this as such, kindly leave an comment about it.

For people who don’t have a Precision 7450 you may want to try setting it to your GPU subsystem ids.

To get your Dell Precision 7450, iGPU subsystem id run the following command:

```
$ lspci -nnk | grep "Subsystem: Dell UHD Graphics"
Subsystem: Dell UHD Graphics 630 (Mobile) [1028:0926]
```

In my case the vendor id is `1028` and the device is `0926`

Open a terminal and run the following command to edit your virtual machine configuration file. Replace your_vm_name with (well you’ve guessed it) the name you have given to your VM earlier (default should be win10).

```bsh
$ sudo EDITOR=nano virsh edit your_vm_name# Locate the following line
```

`Output`

```xml
<domain type='kvm'># Set it to
<domain type='kvm' xmlns:qemu='http://libvirt.org/schemas/domain/qemu/1.0'># Locate the last closing xml tag
</domain># Right before, add the following commands
  <qemu:commandline>
    <qemu:arg value='-set'/>
    <qemu:arg value='device.hostdev0.x-pci-sub-vendor-id=0x1028'/>
    <qemu:arg value='-set'/>
    <qemu:arg value='device.hostdev0.x-pci-sub-device-id=0x0926'/>
  </qemu:commandline>
```

The values provided by the command you ran earlier are in hexadecimal and this value expect decimals as such you can add `0x` before those values and QEMU will understand that you passed hexadecimal values.

At this point make sure that your OS doesn’t show a code 31 or 32 and your external monitor shows a small resolution windows session. If it doesn’t, try removing the QXL Video Device and the Spice Server.

You can now install your NVIDIA drivers.

### Step 5.2 — Correct Code 43

For some reasons, NVIDIA doesn’t consider your expensive Quadro card to be worthy of running in a VM and as such they have deployed some software checks in the drivers to crash the device if it detects certain features specific to virtual devices.

It will also fail if the machine doesn’t have a battery or has a seemingly simulated battery.

As such we need to hide the fact that windows is running in a VM and add a fake battery to the acpi table

Copy the following Base64 encoded string into a base64 decoder such as this one and download it as a file.

```text
U1NEVKEAAAAB9EJPQ0hTAEJYUENTU0RUAQAAAElOVEwYEBkgoA8AFVwuX1NCX1BDSTAGABBMBi5f
U0JfUENJMFuCTwVCQVQwCF9ISUQMQdAMCghfVUlEABQJX1NUQQCkCh8UK19CSUYApBIjDQELcBcL
cBcBC9A5C1gCCywBCjwKPA0ADQANTElPTgANABQSX0JTVACkEgoEAAALcBcL0Dk=
```

Rename that file and place it somewhere it will not be deleted

```bsh
mkdir ~/.vfio$ mv application.bin ~/.vfio/SSDT1.dat
```

Now add the following nodes to your <qemu:commandline>tag and make sure to replace your_username with your username.

```xml
<qemu:arg value="-acpitable"/>
<qemu:arg value="file=/home/your_username/.vfio/SSDT1.dat"/>
```

Once that is done your qemu commands should look like this

```xml
<qemu:commandline>
  <qemu:arg value='-set'/>
  <qemu:arg value='device.hostdev0.x-pci-sub-vendor-id=your_vendor_id'/>
  <qemu:arg value='-set'/>
  <qemu:arg value='device.hostdev0.x-pci-sub-device-id=your_device_id'/>
  <qemu:arg value='-acpitable'/>
  <qemu:arg value='file=/home/your_username/.vfio/SSDT1.dat'/>
</qemu:commandline>
```

Once that is done you may need to add the following values to your features tag depending on your GPU but it is not required for the Quadro T1000 so far.

```xml
<features>
  <hyperv>
    ...
    <vendor_id state='on' value='whatever'/>
    ...
  </hyperv>
  ...
  <kvm>
    <hidden state='on'/>
  </kvm>
  <ioapic driver='kvm'/>
</features>
```

Now restart your `libvirtd.service` and your VM and your GPU should be working.

### Step 6 — Performance tuning

Out of the box performance will not be optimal. You may experience stuttering, high CPU usage but it may still be usable. If you want to reach the next level however, you will need to do a few more things which are using optimized drivers for disks and network adapters, using a memory balloon to reclaim memory for the host and using CPU pinning.

[Here is an article on how to do all of that](https://medium.com/@Leduccc/improving-the-performance-of-a-windows-10-guest-on-qemu-a5b3f54d9cf5).

## Known issues

1. The VM takes a long time to reach OVMF splash screen (2–3 minutes) without a custom kernel (See how to compile a custom kernel here)
2. Enabling Hyper-V crashes the guest at boot without CPU features emulation
3. High CPU usage even when the VM Idles without CPU pinning or when using CPU features emulation (See how to do cpu pinning here)

**EDIT**:

Hyper-v can be fixed by passing in a CPU closely related to your model and setting the “vmx” feature to “required”. You may have to disable features that are missing on your CPU. QEMU will inform you when you are attempting to start a guest with CPU features that your host doesn’t support. You also need the correct hyper-v enlightenment. Finally make sure that you provide a topology that matches the number of cpus passed to your VM. In my case I assigned 12 cpus and my CPU has 8 physical cores with hyper-threading on each so I set my topology to 6 core with 2 threads each.

```xml
...
<features>
  ...
    <hyperv>
      <relaxed state='on'/>
      <vapic state='on'/>
      <spinlocks state='on' retries='8191'/>
    </hyperv>
  ...
<features>
...
<cpu mode='custom' match='exact' check='partial'>
  <model fallback='allow'>Skylake-Client</model>
  <topology sockets='1' cores='6' threads='2'/>
  <feature policy='require' name='vmx'/>
  <feature policy='disable' name='rtm'/>
  <feature policy='disable' name='hle'/>
</cpu>
...
<clock offset='localtime'>
  ...
  <timer name='hypervclock' present='yes'/>
  ...
</clock>
...
```

**EDIT 2**:

It is possible to fix the long time to reach the OVMF splash screen by compiling a custom kernel with the following configuration changes:

```yml
CONFIG_PREEMPT_VOLUNTARY=y
# CONFIG_PREEMPT=y
```

[The instructions on how to achieve this are outlined in this article][3]

**Final thoughts**

There are many ways that this procedure can go wrong, make sure to follow every step very carefully and comment if you are having issues!

**Sources**

I would like to give my thanks [Misairu-G][4] and [/u/keyhoad][5] for their invaluable work! If it hadn’t been for their work, I wouldn’t have been able to get this working at all.

[The base guide and reference for the procedure][6]

[An excellent guide for MUXed Laptops (Such as the Precision 7450)][7]

[A fix for the card being unrecognizable and the drivers failing to find compatible hardware][8]

[An indispensable investigation into the logic behind code 43 errors and a new solution that makes it possible to run this card in a virtual machine][9]

[Information about issues with nested virtualization and hyper-v on a Windows Guest][10]

[Information about issues with nested virtualization and hyper-v on a Windows Guest kvm][11]

[1]: [https://leduccc.medium.com/simple-dgpu-passthrough-on-a-dell-precision-7450-ebe65b2e648e] "GPU Passthrough on a Dell Precision 7450 and other high end laptops (May 2020)"
[2]: [https://wiki.archlinux.org/index.php/PCI_passthrough_via_OVMF#Setting_up_IOMMU]. "See this link for a detailed explanation"
[3]: [https://medium.com/@Leduccc/compiling-a-custom-linux-kernel-on-manjaro-f09aa103713f]. "The instructions on how to achieve this are outlined in this article"
[4]: [https://gist.github.com/Misairu-G] "Misairu-G"
[5]: [https://www.reddit.com/user/keyhoad/] "/u/keyhoad"
[6]: [https://wiki.archlinux.org/index.php/PCI_passthrough_via_OVMF#%22Error_43:_Driver_failed_to_load%22_on_Nvidia_GPUs_passed_to_Windows_VMs] "The base guide and reference for the procedure"
[7]: [https://gist.github.com/Misairu-G/616f7b2756c488148b7309addc940b28#hardware] "An excellent guide for MUXed Laptops (Such as the Precision 7450)"
[8]: [https://www.reddit.com/r/VFIO/comments/99cc4l/cant_install_nvidia_drivers/] "A fix for the card being unrecognizable and the drivers failing to find compatible hardware"
[9]: [https://www.reddit.com/r/VFIO/comments/ebo2uk/nvidia_geforce_rtx_2060_mobile_success_qemu_ovmf/?utm_medium=android_app&utm_source=share] "An indispensable investigation into the logic behind code 43 errors and a new solution that makes it possible to run this card in a virtual machine"
[10]: [https://www.reddit.com/r/homelab/comments/aj4cdt/has_anyone_successfully_gotten_hyperv_docker/] "Information about issues with nested virtualization and hyper-v on a Windows Guest"
[11]: [https://ladipro.wordpress.com/2017/02/24/running-hyperv-in-kvm-guest/] "Information about issues with nested virtualization and hyper-v on a Windows Guest kvm"

[//begin]: # "Autogenerated link references for markdown compatibility"
[windows-10-improving-the-performance-guest-on-qemu]: windows-10-improving-the-performance-guest-on-qemu.md "Windows 10 improving the performance guest on QEMU"
[//end]: # "Autogenerated link references"
