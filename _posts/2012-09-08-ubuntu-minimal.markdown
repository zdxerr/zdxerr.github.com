---
layout: post
title: Ubuntu Minimal
categories: [Linux, Ubuntu, installation]
---

This is a description of how to setup a basic working system, after installing [Ubuntu Minimal](https://help.ubuntu.com/community/Installation/MinimalCD).

First install X.org, Xfce to get a graphical interface, the GNOME Display Manger (GDM) for a graphical login and _network-manager_ to manage network connections:

`sudo aptitude install xorg xfce4 gdm network-manager`

Then type `startx` to run the graphical environment.

System Tools
============

[PCManFM](http://pcmanfm.sourceforge.net/) as a FileManager, [conky](http://conky.sourceforge.net/) for system monitoring and [HardInfo](http://hardinfo.berlios.de/) for hardware informations.

`sudo aptitude install pcmanfm conky hardinfo`

Users
=====

Create user:
`sudo adduser <username>`

Add user to certain groups:
`sudo usermod -aG dialout,cdrom,floppy,audio,video,plugdev,fuse,lpadmin <username>`

Updating/Upgrading
==================

`sudo aptitude update`

`sudo aptitude safe-upgrade`

`sudo aptitude full-upgrade`

Windows Network
===============

To browser and create windows network share you need samba (pyNeighborhood is a SMB/CIFS browsing utility):

`sudo aptitude install smbfs pyneighborhood`

Media Handling
==============

VLC for Video Playback, Exaile for Music, Chrome for web-browsing and Flash/Java plugins.

`sudo aptitude install vlc exaile google-chrome-stable flashplugin-nonfree sun-java5-plugin`