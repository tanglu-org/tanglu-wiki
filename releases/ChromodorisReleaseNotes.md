---
title: Releases ChromodorisReleaseNotes
permalink: /Releases/ChromodorisReleaseNotes/
---

Tanglu 3.0 Release Notes
========================

We are proud to announce the release of Tanglu 3.0 (Chromodoris willani). For this release, we support the KDE Plasma flavour. A GNOME flavour is available as a preview as well.

__TOC__

Upgrading
---------

### From Tanglu 2.0

To upgrade from Bartholomea to Chromodoris follow the [upgrade guide](/BartholomeaChromodorisUpgrade "wikilink"). A fresh installation is recommended.

### From Debian

An upgrade from Debian is not supported, it theory an upgrade from Debian Wheezy or Jessie should work, but we strongly advise for a fresh installation of Tanglu. An upgrade from Ubuntu is not possible.

Installing Tanglu
-----------------

You can install Tanglu using d-i (via the "Install Tanglu" option in the live-cd boot menu), or by using the Calamares installer on the Live-CD (select "Install Tanglu" in K-Menu/GNOME-Shell/etc. when booted into the live-mode). If you install with Calamares, be aware that bug [\#133](http://tracker.tanglu.org/T133) exists, and might hit you if you have a broken BIOS.

### UEFI Support

Tanglu 2 contains highly experimental UEFI support for early adopters. We did not perform enough testing to make this feature work flawlessly. The recommended way to install Tanglu is to use the BIOS compatibility mode of your UEFI. Tanglu 3 will install properly in EFI partitions, but the live-cd does not support EFI properly yet.

### Installer

#### Live Installer

We now use [Calamares](https://calamares.io/) as live-installer for Tanglu. Installations with the live-installer should work well, but if you experience problems, you can switch to the alternate installer.

#### Alternate Installer (d-i)

The Debian Installer (d-i) is available as installation method for Tanglu. In order to install Tanglu with it, choose the "Install Tanglu" option from the live-cd boot menu.

Support lifespan
----------------

Tanglu 3.0 (Chromodoris willani) will be supported for one month after the next version of Tanglu is released. Only packages on the live-cd are fully supported, but any packages might receive updates as time and interest permits.

Updated packages
----------------

### Linux 4.0

We ship with Linux 4.0, an upgrade to a higher version will maybe be offered later.

### Systemd

Systemd is available in form of its latest 224 release and used as PID 1 by default.

### GRUB2

We use GRUB2 as our default boot manager. If Tanglu is the only installed OS, GRUB will boot it straight away, without showing any splash screen. If Tanglu fails to start, a GRUB screen will be shown on next boot. You can also force-display it by holding the shift key on boot.

Other Features
--------------

AppStream
---------

Tanglu 3 ships with up-to-date AppStream metadata, which is used by GNOME Software and Muon.

KDE Plasma 5
------------

We ship with KDEs Plasma Workspaces 5.3. See [dot.kde.org/2015/04/28/plasma-5.3](https://dot.kde.org/2015/04/28/plasma-5.3) for details about this new Plasma release.

GNOME
-----

GNOME 3.16 is included. You can find information about GNOME 3.16 in [the project's 3.16 release announcement](https://help.gnome.org/misc/release-notes/3.16).

Known Issues
------------

-   GNOME: If you run offlne-updates, you will not see information about the update progress on the bootscreen (a fix for that is scheduled).

<!-- -->

-   Due to a bug in the KDE live-session setup scripts, a user account named 'user' won't be able to log in after installation. If you're affected by this, please reboot into recovery mode from grub and delete the /home/user/.kde folder.

<!-- -->

-   When installing with Calamares, the *boot* flag is not set on the boot partition. This causes problems with some BIOSes which expect it to be present. For details, see [bug \#133](http://tracker.tanglu.org/T133).

Reporting Bugs
--------------

See <http://tracker.tanglu.org> Please report the bug against the package you experience problems with, and be as verbose as possible in your bug report. Please try to avoid duplicating bug reports from Debian or upstream projects in Tanglu.

Participate in Tanglu
---------------------

We need developers! So if you are a Debian Developer or Maintainer or an Ubuntu Developer, please ask to join the team and you can get access quickly. If you know how to create Debian packages and we have a trust chain to your GPG-key, it is also very easy to get involved. For everything else, we will help as good as we can. There are also plenty of non-packaging tasks to do, for example on the installer (port Ubiquity? get d-i to work?), or you can help with the artwork. Take a look at <http://tanglu.org/contribute/> for some suggestions on how to get involved.

Community
---------

You can join the Tanglu community at <http://tangluusers.org>

Download
--------

You can download Tanglu Live images on our download page: <http://tanglu.org/download/> For experienced users, we ship a Docker image to play with the base system.

[category:Chromodoris](/category:Chromodoris "wikilink") [category:ReleaseNotes](/category:ReleaseNotes "wikilink")