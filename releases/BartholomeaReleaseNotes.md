---
title: Releases BartholomeaReleaseNotes
permalink: /Releases/BartholomeaReleaseNotes/
---

Tanglu 2.0 Release Notes
========================

We are proud to announce the release of Tanglu 2.0 (Bartholomea annulata). For this release, we support the KDE Plasma flavour. A GNOME flavour is available as a preview as well.

__TOC__

Upgrading
---------

### From Tanglu 1.0

To upgrade from Aequorea to Bartholomea, follow the [upgrade guide](/AequoreaBartholomeaUpgrade "wikilink").

### From Debian

Upgrading from Debian is possible, but only for experienced users who upgrade from Debian 7 (Wheezy). The preferred way to get Tanglu is a fresh installation, and we don't really support an upgrade from Debian, so if you do it, make a backup first. An upgrade from Ubuntu is not possible.

Installing Tanglu
-----------------

The recommended method to install Tanglu is using the d-i installation method. To install Tanglu, select the "Install Tanglu" option from the live-cd boot menu.

### UEFI Support

Tanglu 2 contains highly experimental UEFI support for early adopters. We did not perform enough testing to make this feature work flawlessly. The recommended way to install Tanglu is to use the BIOS compatibility mode of your UEFI. If you want to test the experimental support, boot the live-cd and use d-i to install Tanglu ("Install Tanglu" option in the live-cd menu). The live-installer still does not support UEFI.

### Installer

#### Live Installer

The live installer only received some minor bugfixes and tweaks. The recommended method to install Tanglu is via d-i

#### D-I installer

The Debian Installer (d-i) is available as installation method for Tanglu. In order to install Tanglu with it, choose the "Install Tanglu" option from the live-cd boot menu.

Support lifespan
----------------

Tanglu 2.0 (Bartholomea annulata) will be supported for one month after the next version of Tanglu is released. Only packages on the live-cd are fully supported, but any packages might receive updates as time and interest permits.

Updated packages
----------------

### Linux 3.16

We ship with a Linux 3.16, the same kernel currently used in Debian Jessie.

### Systemd

Systemd is available in version 215 and used as PID 1 by default.

### GRUB2

We use GRUB2 as our default boot manager. If Tanglu is the only installed OS, GRUB will boot it straight away, without showing any splash screen. If Tanglu fails to start, a GRUB screen will be shown on next boot. You can also force-display it by holding the shift key on boot.

Other Features
--------------

AppStream
---------

Tanglu 2 ships with initial, very basic support for [AppStream](http://www.freedesktop.org/software/appstream/docs/) metadata, which enhances the software-selection experience in GNOMEs GNOME-Software and KDEs Apper. The data is not yet complete, and can be seen as working technology preview. AppStream support will be improved further with Tanglu 3.

KDE
---

KDE workspaces are available in version 4.11, KDE applications are available in version 4.14. You can find information about the new stuff in KDE SC in the [4.14 release announcement](http://www.kde.org/announcements/4.14/).

The KDE flavor now uses SDDM as default display manager, replacing KDM.

GNOME
-----

GNOME 3.14 is included. You can find information about GNOME 3.14 in [the project's 3.14 release announcement](https://help.gnome.org/misc/release-notes/3.14).

### GNOME-Software

GNOME now ships with GNOME-Software to perform updates and install software. To make GNOME-Software work with Tanglu, we ship with with some very basic AppStream metadata.

#### Offline Updates

GNOME-Software performs offline-updates by default (install updates on reboot instead of instantly). This features did not receive excessive testing yet, and offline updates might not always work. You still have the opportunity to install updates "online" using the regular update tool.

Known Issues
------------

-   The live-installer does not preseed debconf answers for the location GRUB is installed to, so you might get a debconf question about the location when upgrading GRUB. To fix this, execute
    ``` bash
     sudo dpkg-reconfigure grub-pc
    ```

    after installing Tanglu and answer the questions. We recommend installing using the d-i installer instead ("Install Tanglu" option on the live-cd boot menu), which does not have this issue.

<!-- -->

-   GNOME: If you run offlne-updates, you will not see information about the update progress on the bootscreen (a fix for that is scheduled).

<!-- -->

-   Due to a bug in the KDE live-session setup scripts, a user account named 'user' won't be able to log in after installation. If you're affected by this, please reboot into recovery mode from grub and delete the /home/user/.kde folder.

Reporting Bugs
--------------

See <http://bugs.tanglu.org> Please report the bug against the package you experience problems with, and be as verbose as possible in your bug report. Please try to avoid duplicating bug reports from Debian or upstream projects in Tanglu.

Participate in Tanglu
---------------------

We need developers! So if you are a Debian Developer or Maintainer or an Ubuntu Developer, please ask to join the team and you can get access quickly. If you know how to create Debian packages and we have a trust chain to your GPG-key, it is also very easy to get involved. For everything else, we will help as good as we can. There are also plenty of non-packaging tasks to do, for example on the installer (port Ubiquity? get d-i to work?), or you can help with the artwork. Take a look at <http://tanglu.org/contribute/> for some suggestions on how to get involved.

Community
---------

You can join the Tanglu community at <http://tangluusers.org>

Download
--------

You can download Tanglu Live images on our download page: <http://tanglu.org/download/> For experienced users, we ship a Docker image to play with the base system.