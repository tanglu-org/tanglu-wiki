---
title: Tanglu 4 Release Notes
---

Tanglu 4.0 Release Notes
========================

We are proud to announce the release of Tanglu 4.0 (Dasyatis kuhlii). 

Upgrading
---------

### From Tanglu 3.0

TODO

Installing Tanglu
-----------------

TODO

### UEFI Support

TODO

### Installer

#### Live Installer

We now use [Calamares](https://calamares.io/) as live-installer for Tanglu. Installations with the live-installer should work well, but if you experience problems, you can switch to the alternate installer.

#### Alternate Installer (d-i)

The Debian Installer (d-i) is available as installation method for Tanglu. In order to install Tanglu with it, choose the "Install Tanglu" option from the live-cd boot menu.

Support lifespan
----------------

TODO

Updated packages
----------------

### Linux 4.4

We ship with Linux 4.4

TODO

### Systemd

TODO

### GRUB2

We use GRUB2 as our default boot manager. If Tanglu is the only installed OS, GRUB will boot it straight away, without showing any splash screen. If Tanglu fails to start, a GRUB screen will be shown on next boot. You can also force-display it by holding the shift key on boot.

Other Features
--------------

AppStream
---------

TODO

KDE Plasma 5
------------

TODO

GNOME
-----

TODO

Known Issues
------------

* TODO
* TODO

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

---
[category:Chromodoris](/Dasyatis)
[category:ReleaseNotes](/ReleaseNotes)
