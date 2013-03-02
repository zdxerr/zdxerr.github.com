---
layout: post
title: Arch Linux
categories: [Linux, Arch, installation]
---

This is a description of how to setup a basic system with [Arch Linux](https://www.archlinux.org/) based on this [guide][guide].

1. Change keyboard layout:

    loadkeys de

2. Create partitions using `gdisk /dev/sda`.

    Type `n` to create these partitions with the default configurations except the mentioned ones:

    1. _boot_

        - Last sector: `+250M`

    2. _swap_

        - Last sector: `+2G`
        - Hex code or GUID: `8200`

    3. _root_

        - Last sector: `+15G`

    4. _home_

    Type `p` to confirm and `w` to write changes to the disk.

3. Make filesystems

        mkfs -t ext4 /dev/sda1
        mkswap /dev/sda2
        mkfs -t ext4 /dev/sda3
        mkfs -t ext4 /dev/sda4

4. Mount

        swapon /dev/sda2
        mount /dev/sda3 /mnt
        cd /mnt
        mkdir boot home
        mount /dev/sda1 boot
        mount /dev/sda4 home

5. Install

        pacstrap /mnt base base-devel

6. Generate `fstab`

        genfstab -p /mnt >> /mnt/etc/fstab
        more /mnt/etc/fstab

7. Bootloader

        pacstrap /mnt syslinux

    Configure it inside the new system.

        arch-chroot /mnt
        bash

    Write to `/etc/locale.conf`:

        LANG="en_US.UTF-8"

    Uncomment in `/etc/locale.gen`:

        en_US.UTF-8 UTF-8
        en_US ISO-8859-1
        locale-gen

8. Timezone

        ln -s /usr/share/zoneinfo/Europe/Berlin /etc/localtime

9. Set `/etc/hostname`

10. Copy bootloader files

        cd /boot/syslinux/
        cp /usr/lib/syslinux/menu.c32 .
        cp /usr/lib/syslinux/vesamenu.c32 .
        cp /usr/lib/syslinux/chain.c32 .
        cp /usr/lib/syslinux/hdt.c32 .
        cp /usr/lib/syslinux/reboot.c32 .
        cp /usr/lib/syslinux/poweroff.com .

        extlinux --install /boot/syslinux

        dd conv=notrunc bs=440 count=1 if=/usr/lib/syslinux/gptmbr.bin of=/dev/sda

        mkinitcpio -p linux

[guide]: http://wideaperture.net/blog/?p=3851 "A Guide to Installing Arch in VirtualBox"
