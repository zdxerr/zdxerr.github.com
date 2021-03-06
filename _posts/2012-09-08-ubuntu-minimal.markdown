---
layout: post
title: Ubuntu Minimal
categories: [Linux, Ubuntu, installation]
---

This is a description of how to setup a basic working system, after installing [Ubuntu Minimal](https://help.ubuntu.com/community/Installation/MinimalCD).

First install X.org, Xfce to get a graphical interface and _wicd_ to manage your network connections:

`sudo aptitude install xorg xfce4 wicd`

Then type `startx` to run the graphical environment (if this does not work, add `exec startxfce4` to your `.xinitrc` in your home directory).

## System Tools

[conky](http://conky.sourceforge.net/) for system monitoring and [HardInfo](http://hardinfo.berlios.de/) for hardware informations.

    sudo aptitude install xfce4-terminal conky hardinfo

## Users

Create user:

    sudo adduser <username>

Add user to certain groups:

    sudo usermod -aG dialout,cdrom,floppy,audio,video,plugdev,fuse,lpadmin <username>

## Updating/Upgrading

    sudo aptitude update
    sudo aptitude safe-upgrade
    sudo aptitude full-upgrade

## Windows Network

To browser and create windows network share you need samba (pyNeighborhood is a SMB/CIFS browsing utility):

`sudo aptitude install smbfs pyneighborhood`

## Media Handling

_Xarchiver_ for compressed files, _VLC_ for Video Playback, _Epdfview_ for PDFs, _Transmission_ for torrents, _Chrome_ for web-browsing and Flash/Java plugins.

    sudo aptitude install xarchiver vlc epdfview transmission chromium-browser flashplugin-nonfree flashplugin-installer chromium-codecs-ffmpeg chromium-codecs-ffmpeg-extra

### Image Viewer

To compile and install _[ViewNior](http://xsisqox.github.com/Viewnior/download.html)_:

    sudo aptitude install build-essential intltool libgdk-pixbuf2.0-dev libgtk2.0-dev
    tar zxvf viewnior-1.3.tar.gz
    cd cd viewnior-1.3
    ./configure
    make
    sudo make install


## Tools

### Latex

Install [texlive](http://www.tug.org/texlive/) from the website because the distribution contained in Ubuntu is old.

### Sublime Text 2

1. Download [Sublime Text 2](http://www.sublimetext.com/2).
2. Untar the archive `tar xf Sublime\ Text\ 2\ Build\ 2181\ x64.tar.bz2`.
3. Move the extracted folder to `/usr/lib` with `sudo mv Sublime\ Text\ 2 /usr/lib/`.
4. Create a shortcut for the terminal `sudo ln -s /usr/lib/Sublime\ Text\ 2/sublime_text /usr/bin/sublime`.
5. Create a desktop file in `/usr/share/applications` using `sudo sublime /usr/share/applications/sublime.desktop` containing:

        [Desktop Entry]
        Version=1.0
        Name=Sublime Text 2
        # Only KDE 4 seems to use GenericName, so we reuse the KDE strings.
        # From Ubuntu's language-pack-kde-XX-base packages, version 9.04-20090413.
        GenericName=Text Editor

        Exec=sublime
        Terminal=false
        Icon=/usr/lib/Sublime Text 2/Icon/48x48/sublime_text.png
        Type=Application
        Categories=TextEditor;IDE;Development
        X-Ayatana-Desktop-Shortcuts=NewWindow

        [NewWindow Shortcut Group]
        Name=New Window
        Exec=sublime -n
        TargetEnvironment=Unity

6. Associate _Sublime Text_ with all file formats you want in the `defaults.list` using `sudo sublime /usr/share/applications/defaults.list`.

### Development

    sudo aptitude install haskell-platform
