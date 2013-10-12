---
layout: post
title: Arch Linux
categories: [Linux, Arch, installation]
---

This is a description of how to setup a basic system with [Arch Linux](https://www.archlinux.org/) based on this [guide][guide]. Infos on the Thinkpad T410 can be found [here][t410].

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

    Altenative:

        syslinux-install_update -iam

11. Setup ramdisk

        mkinitcpio -p linux

0. Set root password

        passwd

12. Exit system

        exit
        exit

13. Unmount

        umount /mnt/boot
        umount /mnt/home
        swapoff /dev/sda2
        umount /mnt

    Set legacy BIOS bootable on partition 1:

        sgdisk /dev/sda --attributes=1:set:2

        reboot

14. Configure network

        dhcpcd

    or for wifi

        pacman -S net-tools wireless_tools wpa_supplicant wpa_actiond dialog

    [Install specific driver](https://wiki.archlinux.org/index.php/Wireless_Setup#Drivers_and_firmware)

        wifi-menu <interface>

    use [netcfg](https://wiki.archlinux.org/index.php/Netcfg) for network profiles

15. Add a user

        useradd -m -g users -s /bin/bash <name>
        passwd <name>

16. Install sudo and make user a sudoer

        pacman -S sudo

    In `/etc/sudoers` add

        <name> ALL=(ALL) ALL

0. Log in

        exit

0. Set default keyborad layout in `/etc/vconsole.conf`

        KEYMAP=de-latin1-nodeadkeys

17. Enable auto network configuration

        sudo systemctl enable dhcpcd.service

18. Install _X_

        sudo pacman -S xorg-server xorg-xinit xorg-server-utils

0. Install _mesa_ (for 3d-graphics)

        sudo pacman -S mesa

    or _nvidia driver_

        sudo pacman -S nvidia

    or `nvidia-utils-bumblebee` and `nvidia-bumblebee` for optimus and start

        gpasswd -a <username> bumblebee
        exit

        systemctl enable bumblebeed
        reboot

    or for [Intel](https://wiki.archlinux.de/title/Intel)


0. Virtualbox utils

        sudo pacman -S virtualbox-guest-utils

    Add to `/etc/modules-load.d/virtualbox.conf`

        vboxguest
        vboxsf
        vboxvideo

0. ZSH

        sudo pacman -S zsh
        chsh -s $(which zsh)

    settings in `.zshrc`

        autoload -U compinit promptinit
        compinit
        promptinit

        # This will set the default prompt to the walters theme
        prompt walters

0. _xfce_

        sudo pacman -S xfce4 xfce4-goodies

    copy

        cp /etc/skel/.xinitrc ~/.xinitrc

    add line

        exec startxfce4

[guide]: http://wideaperture.net/blog/?p=3851 "A Guide to Installing Arch in VirtualBox"
[t410]: https://wiki.archlinux.org/index.php/Lenovo_ThinkPad_T410
