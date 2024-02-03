---
title: "Create a minimal, local Debian VM with QEMU"
date: 2024-01-28T08:31:19-05:00
draft: true
tags:
- qemu
---

I occasionally need to spin up a local, short-lived VM on my workstation in order to test something on Linux. For example, I was recently testing a Kubernetes backup utility, so I created a local VM and installed K3s in it.

Vagrant and VirtualBox are my go-to for this. I'm up and off to the races with just a few commands.

```sh
vagrant init hashicorp/bionic64
vagrant up
vagrant ssh
```

But after seeing how [easy it is to spin up a local VM with QEMU](https://drewdevault.com/2018/09/10/Getting-started-with-qemu.html), I decided to give that a try. I'll go ahead and warn you it's not as simple as the three commands above.

In this post I will walk through the steps to deploy the Debian cloud image to a local VM using QEMU as the hypervisor.

> **📝 Software versions**
> - macOS 12.7
> - Debian 12 "nocloud" image
> - QEMU emulator version 8.2.0

## Install QEMU

Install QEMU from Homebrew. See [Download QEMU](https://www.qemu.org/download/) for installation instructions for other platforms.

```sh
brew install qemu
```

## Prepare the image

Download the "nocloud" image, which [Getting Debian](https://www.debian.org/distrib/) indicates is appropriate for use with a local QEMU virtual machine. See [Debian Official Cloud Images](https://cloud.debian.org/images/cloud/) to find images for other versions and architectures.

```sh
curl -Lo debian.qcow2 \
    https://cloud.debian.org/images/cloud/bookworm/latest/debian-12-nocloud-amd64.qcow2
```

Display information about the image you just downloaded.

```sh
qemu-img info debian.qcow2
```

Output:

```plain {hl_lines=[3]}
image: debian.qcow2
file format: qcow2
virtual size: 2 GiB (2147483648 bytes)
disk size: 352 MiB
cluster_size: 65536
…
```

You'll notice that the image's virtual size, highlighted in the output above, is far too small to install anything meaningful on the VM. Let's resize the image to give us some breathing room.

```sh
qemu-img resize debian.qcow2 16G
```

Output:

```plain
Image resized.
```

## Launch the virtual machine

Launch a QEMU VM and attach this image.

```sh
qemu-system-x86_64 \
    -m 2048 \
    -smp 2 \
    -drive file=debian.qcow2,media=disk,if=virtio \
    -nic user,model=virtio \
    -nographic \
    -accel hvf
```

Let's review those options.

- `qemu-system-x86_64`: If you downloaded an image with a different architecture, use the variation of `qemu-system-*` that matches that architecture.
- `-m 2048`: Assign the VM 2048 megabytes of RAM.
- `-smp 2`: Allows the VM to use 2 CPUs.
- `-drive file=debian.qcow2,media=disk,if=virtio`: Mount the drive image. This will get mounted at `/dev/vda`.
- `-nic user,model=virtio`: Give the guest VM access to the host network and internet via NAT. Other machines on the host network, including the host itself, **cannot directly reach the guest**. If you need to reach the guest from other machines, check out the [`tap` networking backend](https://wiki.qemu.org/Documentation/Networking#Tap).
- `-nographic`: Don't open the VM in a new window. Use the current terminal instead.
- `-accel hvf`: If you're host is macOS and the architecture of the VM matches the architecture of your machine, [use this accelerator](https://wiki.qemu.org/Hosts/Mac#Notes) to speed up execution inside your VM. If your host machine is Linux, you may be able to [use `--enable-kvm` instead](https://wiki.qemu.org/Features/KVM).

When prompted for a login, enter `root` with no password.

```sh
localhost login: root
```

## Expand the filesystem

If you run `df`, you'll notice that the file system size is only 1.9G.

```sh {hl_lines=[3]}
df -h
```

Output:

```plain {hl_lines=[4]}
Filesystem      Size  Used Avail Use% Mounted on
udev            965M     0  965M   0% /dev
tmpfs           197M  444K  197M   1% /run
/dev/vda1       1.9G  954M  786M  55% /
tmpfs           984M     0  984M   0% /dev/shm
…
```

Let's extend the partition to take advantage of the additional disk space.

```sh
growpart /dev/vda 1
```

Output:

```plain
CHANGED: partition=1 start=262144 old: size=3930112 end=4192255 new: size=33292255 end=33554398
```

And then resize the file system.

```sh
resize2fs /dev/vda1
```

Output:

```plain
resize2fs 1.47.0 (5-Feb-2023)
[  689.793229] EXT4-fs (vda1): resizing filesystem from 491264 to 4161531 blocks
Filesystem at /dev/vda1 is mounted on /; on-line resizing required
old_desc_blocks = 1, new_desc_blocks = 2
[  689.897230] EXT4-fs (vda1): resized filesystem to 4161531
The filesystem on /dev/vda1 is now 4161531 (4k) blocks long.
```

Check out the file system disk usage again and you'll see that we're taking advantage of the full 16G now.

```sh
df -h
```

Output:

```plain {hl_lines=[4]}
Filesystem      Size  Used Avail Use% Mounted on
udev            965M     0  965M   0% /dev
tmpfs           197M  444K  197M   1% /run
/dev/vda1        16G  954M   14G   7% /
tmpfs           984M     0  984M   0% /dev/shm
…
```

## Reuse the image

Alright, so that wasn't as easy as those three Vagrant commands. But we can save time spinning up future VMs based on this image file.

Shutdown the VM and then copy the image file each time you want to spin up a new VM. The new VM will already have the expanded file system set up. In the example below, I copied the image and spun up a new VM based on `testbox.qcow2`.

```sh
cp debian.qcow2 testbox.qcow2

qemu-system-x86_64 \
    -m 2048 \
    -smp 2 \
    -drive file=testbox.qcow2,media=disk,if=virtio \
    -nic user,model=virtio \
    -nographic \
    -accel hvf
```

If you want to continue using Vagrant, but use QEMU as your hypervisor, check out [Vagrant Libvirt Provider](https://github.com/vagrant-libvirt/vagrant-libvirt) or [Vagrant QEMU Provider](https://github.com/ppggff/vagrant-qemu) to use QEMU as the Vagrant provider.
