---
title: Releases AequoreaReleaseNotes
permalink: /Releases/AequoreaReleaseNotes/
---

Tanglu 1.0 Release Notes
========================

We are proud to announce the release of Tanglu 1.0 (Aequorea Victoria), the first stable release of Tanglu. For this release, we support the KDE Plasma flavour. A GNOME flavour is available as a preview.

__TOC__

Tanglu is a social project, building a distribution in the open, sharing technology with Debian and other distributions. It also re-thinks the way distributions are built today, aiming for a layered approach with applications shipped separately from the operating system core in future. Compared to Debian, it offers frequent time-based releases and often ships more recent software than Debian does, making use of recent Linux technology. The 1.0 release is not the finished product, it just marks a major milestone of the Tanglu Project.

Upgrading from Debian
---------------------

Upgrading from Debian is possible, but only for experienced users who upgrade from Debian 7 (Wheezy). The preferred way to get Tanglu is a fresh installation, and we don't really support an upgrade from Debian, so if you do it, make a backup first. An upgrade from Ubuntu is not possible.

Installing Tanglu
-----------------

### UEFI Support

We currently do not support UEFI in our live-installer, this feature did not complete in time. If you are an experienced user, you can manually install Tanglu on an UEFI-based machine, for all other users we recommend switching to legacy BIOS for this release.

### Live Installer

We use a heavily modified version of the LMDE installer to install Tanglu from the live-cd. The installer is not incredibly fast, but working (if you don't use UEFI, see the UEFI section). You might want to partition your harddrive manually to match your use-case.

Support lifespan
----------------

Tanglu 1.0 (Aequorea Victoria) will be supported for one month after the next version of Tanglu is released. Only packages on the live-cd are fully supported, but any packages might receive updates as time and interest permits.

Updated packages
----------------

### Linux 3.12

We ship with a Linux 3.12, the same Kernel currently used in Debian testing/unstable.

### Systemd

Systemd is available in version 204 and used by default as PID 1.

### GRUB2

We use GRUB2 as our default boot manager. If Tanglu is the only installed OS, GRUB will boot it straight away, without showing any splash screen. If Tanglu fails to start, a GRUB screen will be shown on next boot. You can also force-display it by holding the shift key on boot.

Other Features
--------------

### Persistent systemd journal by default

Tanglu ships with persistent journal logging by default, so you can use journalctl to introspect logs from previous boots as well as the current one. This can be disabled in /etc/systemd/journald.conf.

### No default syslog daemon

Tanglu ships without a syslog deamon by default, as the systemd journal is sufficient for most use cases. To restore the traditional syslog support install the 'rsyslog' package.

### No default MTA

Tanglu ships without MTA by default. If you need an MTA, install the one of your choice and configure it properly.

### Non-Free/Contrib enabled by default

This is more like an anti-feature and will be changed with the next Tanglu release. In order to allow an easy boot of Tanglu, we include some proprietary firmware on the CD by default, which requires that the non-free component is enabled. With the next release of Tanglu, the non-free firmware will get its own component, or we find a better solution to fix the issue. If you don't want proprietary sources enabled, edit your /etc/apt/sources.list and remove non-free/contrib.

KDE
---

KDE SC is available in version 4.11. You can find information about the new stuff in KDE SC in the [4.11 release announcement](http://www.kde.org/announcements/4.11/).

GNOME
-----

GNOME 3.10 is included as a preview release. Currently, the GNOME team does not have enough manpower to maintain GNOME, so if you are a Debian Developer/Maintainer or Ubuntu Maintainer, please join the team and help GNOME. However, we ship with a full GNOME 3.10. A few non-core pieces are still from the 3.8 era. You can find information about GNOME 3.10 in [the project's 3.10 release announcement](https://help.gnome.org/misc/release-notes/3.10/).

Known Issues
------------

-   Tanglu uses the sqlite akonadi backend by default. If you are reusing an old kde profile configured with a different akonadi backend you will need to install it manually. Debian 7 ("wheezy") uses the mysql backend by default, which is included in the 'akonadi-backend-mysql' package.

<!-- -->

-   In some virtual machine configurations the network does not work. We are investigating the issue and will release an update if and when we have figured out how to fix it. No problems have been reported with networking on actual hardware.

<!-- -->

-   Please note that debian [bug\#726027](https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=726027) is not yet resolved, as it would require adding systemd service files to many more packages. Please keep that in mind when installing packages containing rcS-type services that depend on $network or $remote_fs.

<!-- -->

-   The installer does not preseed debconf answers for the location GRUB is installed to, so you might get a debconf question about the location when upgrading GRUB. To fix this, execute
    ``` bash
     sudo dpkg-reconfigure grub-pc
    ```

    after installing Tanglu and answer the questions.

<!-- -->

-   GParted fails to launch due to some PolicyKit issues on the live-cd which have not yet been resolved. To work around this, execute it manually using *gksudo* or *kdesudo*.

Tanglu Infrastructure
---------------------

We use components of the Debian infrastructure for maintaining Tanglu, including dak (Debian Archive Kit), Britney2 (Testing migration tool), Ben (Transition Tracker) and various tools from the DOSE toolset for QA purposes. For the next release of Tanglu (Bartholomea), it is planned to migrate away from Jenkins as package-build-coordinator and use Debile instead.

Reporting Bugs
--------------

See <http://bugs.tanglu.org> Please report the bug against the package you experience problems with, and be as verbose as possible in your bug report. Please try to avoid duplicating bug reports from Debian or upstream projects in Tanglu.

Participate in Tanglu
---------------------

We need developers! So if you are a Debian Developer or Maintainer or an Ubuntu Developer, please ask to join the team and you can get access quickly. If you know how to create Debian packages and we have a trust chain to your GPG-key, it is also very easy to get involved. For everything else, we will help as good as we can. There are also plenty of non-packaging tasks to do, for example on the installer (port Ubiquity? get d-i to work?), or you can help with the artwork. Take a look at <http://tanglu.org/contribute/> for some suggestions on how to get involved.

Download
--------

You can download Tanglu Live images on our download page: <http://tanglu.org/download/> For experienced users, we ship a Docker image to play with the base system.