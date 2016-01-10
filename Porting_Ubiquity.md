---
title: Porting Ubiquity
permalink: /Porting_Ubiquity/
---

Ubiquity
========

Version: 2.18.10

Download Page: [Ubiquity 2.18.10 on Launchpad](https://launchpad.net/ubuntu/utopic/+source/ubiquity)

Installer [slideshow](http://www.dylanmccall.com/ubiquity-slideshow-preview/) - nice site with all slideshows of the different flavors like Ubuntu, Kubuntu, Edubuntu, etc.

build-dependencies
------------------

Needed packages on the **host** before package can be built:

-   pyflakes
-   pep8
-   dh-di
-   python-scour

Adaption steps
--------------

1.  Get ubiquity to built inside Tanglu's chroot - **Done**
2.  Exchange Ubuntu's d-i components with them in Tanglu - ***in progress***
3.  Create live-CD to see what's working and implement missing parts, etc

Remember list
-------------

What have to be changed in the debian/ directory

-   exchange upstart parts with systemd calls
    -   ubiquity.ubiquity.upstart
    -   oem-config-debconf.oem-config-debconf.upstart
    -   oem-config.oem-config.upstart

<!-- -->

-   check if d-i/patches/localechooser-post-base-installer.patch is needed for special behaviour with combination of some languages and locations. For more info see the infotext in the patch. For now patch is removed in tanglu2

d-i Componnents
===============

What are the differences between Ubuntu and Tanglu/Debian versions. Changelog comparing.

Uninterresting or removed parts are ~~striked~~.

apt-setup
---------

Configure apt

***Current:*** 1:0.80ubuntu6

***Tanglu:*** 1:0.89tanglu3

### Differences

***Versions:***

-   Languages

***Ubuntu:***

-   ~~Install the Ubuntu mirror generator instead of Debian's~~
-   mirror verification timeout: 30 seconds
-   For CD installs, leave the sources.list created by apt-setup in /etc/apt/sources.list.apt-setup, and restore the sources.list created during base installation for the rest of the installation.
-   Honour OVERRIDE_BASE_INSTALLABLE when checking /cdrom/.disk/base_installable.
-   Always disable the CD at the end of installation if any mirrors are present, even if it's a complete CD.
-   Pre-populate apt's lists directory with signed Release files for archive.ubuntu.com (and mirrors) and security.ubuntu.com, to protect against downgrade attacks right from initial installation.
-   Run 'apt-get update' for all sources.list lines produced by a single generator in one go, and don't comment out sources.list lines if it fails.
-   Make the path to security updates configurable, as well as the host.
-   apt-cdrom doesn't unmount the CD if cd_type ends with /single
-   Enable all network sources, including security updates, even if the network is unconfigured.
-   Run 'apt-get update', without downloading package lists or cleaning up old files, after moving the sources.list generated during base system installation back into place
-   If OVERRIDE_LEAVE_CD_MOUNTED is set, don't unmount /cdrom; this is a bad idea in a live CD environment!

**Conclusion:**

base-installer
--------------

base system installation framework

***Current:*** 1.144ubuntu1

***Tanglu:*** 1.145tanglu1

### Differences

***Versions:***

-   Language

***Ubuntu:***

-   Install busybox-initramfs rather than busybox
-   Move some kernel installation support code from bootstrap-base to base-installer, and allow live-installer to override the title template used by that
-   Add overlay archive support

**Conclusion:** create a patch - ***done***.

bterm-unifont
-------------

Include complete Unicode font for bogl-bterm

***Current:*** 1.3

***Tanglu:*** 1.3

### Differences

***Versions:***

-   nothing

***Ubuntu:***

-   nothing

**Conclusion:** Nothing to do

choose-mirror
-------------

Choose mirror to install from (menu item)

***Current:*** 2.57ubuntu1

***Tanglu:*** 2.57tanglu3

### Differences

***Versions:***

-   nothing

***Ubuntu:***

-   Set default country to GB
-   Drop the priorities of the country/mirror questions to medium if we're installing from a CD that includes the base system
-   upport setting OVERRIDE_BASE_INSTALLABLE in the environment to force choose-mirror to assume that the base system is installable
-   Skip mirror validation if the base system is installable and we're installing from a mirror in the masterlist
-   Force xgettext to use UTF-8 encoding when generating templates files, to cope with CÃ´te d'Ivoire

**Conclusion:** create a patch - ***done***.

clock-setup
-----------

set up clock

***Current:*** 0.117ubuntu1

***Tanglu:*** 0.120b1

### Differences

***Versions:***

-   Languages

***Ubuntu:***

-   Ask the UTC question, defaulting to true, even if we appear to be the only OS
-   Pass --noadjfile to hwclock

**Conclusion:** create a patch - ***done***.

console-setup
-------------

console font and keymap setup program

***Current:*** 1.70ubuntu9

***Tanglu:*** 1.104tanglu1

### Differences

***Versions:***

-   Too much :-/ until 1.93
-   1.94 - now mostly translation updates

***Ubuntu:***

-   Move boot tasks to a combination of two udev rules and a single Upstart job, ensuring that they're run at points when we are able to satisfy the constraints on the relevant ioctls
-   Change the default font from Fixed to VGA for Lat15; while it's not entirely complete, it looks better and is largely good enough.
-   Set keymap and font in the initramfs if possible and sensible.
-   Don't try to call update-rc.d if it doesn't exist, such as in d-i
-   Explicitly build-depend on liblocale-gettext-perl for kbdnames-maker, and likewise have keyboard-configuration depend on liblocale-gettext-perl.
-   Depend on debconf instead of pre-depending, because pre-depends have no effect on config scripts
-   Make keyboard-configuration replace old console-setup/console-setup-mini versions as well as conflicting with them.
-   Depend on kbd (&gt;= 1.15-1ubuntu3) for a valuable loadkeys improvement.
-   If the detect-keyboard debconf plugin is available (cdebconf-newt-detect-keys in the installer), then offer to use it to detect the keyboard layout
-   Replace usplash detection code with Plymouth detection code.
-   Load the new keyboard configuration immediately when running 'dpkg-reconfigure keyboard-configuration' in an installed system
-   Tolerate absence of setupcon in keyboard-configuration.postinst
-   Move keyboard detection templates from console-setup.templates to keyboard-configuration.templates.
-   Stop running debconf-updatepo on clean.
-   Include pc105.tree for ubiquity.
-   Run kbd_mode on each tty in ACTIVE_CONSOLES rather than on the current tty, since the current tty might belong to X and changing X's tty out of raw mode is a very bad idea.
-   Weaken test for whether /usr is mounted; testing for /usr/share is sufficient, and fixes operation in d-i.
-   Make setupcon explicitly exit 0, so that postinsts don't fail in the event that loadkeys can't find a console
-   debian/console-setup.console-font.upstart: Add Upstart job that sets up console font when plymouth-splash is starting, to work around a possible udev/plymouth race that would otherwise prevent the font being set (LP: \#632382).

**Conclusion:**

debian-installer-utils (di-utils)
---------------------------------

Miscellaneous utilities for the debian installer

***Current:*** 1.105ubuntu1

***Tanglu:*** 1.107

### Differences

***Versions:***

-   Fixes

***Ubuntu:***

-   user-params: Don't propagate vga=\*, break=\*, \*-ubiquity, or noninteractive to installed system
-   Don't include the battery subsystem on calls to udevadm trigger

**Conclusion:**

<strike>flash-kernel (source-package)
-------------------------------------

utility to make system and certain embedded devices bootable

***Current:*** 3.0~rc.4ubuntu50

***Tanglu:*** -/-

### Differences

***Versions:***

-   -/-

***Ubuntu:***

-   see [changelog](http://changelogs.ubuntu.com/changelogs/pool/main/f/flash-kernel/flash-kernel_3.0~rc.4ubuntu50/changelog)</strike>

**Conclusion:** Not needed: embedded devices (Arm) - removed from current package. Perhaps insert later if armhf is available.

grub-installer
--------------

Install GRUB on a hard disk

***Current:*** 1.78ubuntu20

***Tanglu:*** 1.94

### Differences

***Versions:***

-   Languages
-   Fixes

***Ubuntu:***

-   Show the grub menu and raise the menu timeout if other operating systems are installed (only for GRUB Legacy right now).
-   Remove splash boot parameter unless debian-installer/framebuffer=true and debian-installer/splash=true.
-   If / or /boot are on a removable device, install GRUB there by default.
-   Support setting OVERRIDE_UNSUPPORTED_OS in the environment to force grub-installer to use its default MBR selection method despite there being unsupported operating systems on the disk.
-   Handle cases where /boot is bind-mounted.
-   Add support for writing an GRUB Legacy MBR on each disk in an mdadm-managed RAID providing /boot. (GRUB 2 can handle this already.)
-   Properly make use of output from os-prober to configure the booting of other operating systems on dmraid arrays. Attempt to guess where in the device map the array belongs, by substituting the first drive in the dmraid array for the dmraid array device node itself, and removing any reference to other member disks of the array.
-   Go back to using update-grub -y for GRUB Legacy for now; our grub package is a bit old and still requires this.
-   Allow grub/grub2 choice for ext4, though still default to grub2.
-   If /boot is on an MD device and we're using GRUB 2, install GRUB there rather than (hd0); GRUB 2 will interpret that as meaning that it needs to install to each of the RAID members.
-   If using GRUB 2 and installing to a RAID device any of whose components are partitions, then default to installing to the MBRs of each of the containing disks, since GRUB 2 will refuse to install to the partition devices.
-   Add a preseedable grub-installer/timeout template to adjust the initial GRUB timeout.
-   Install GRUB to the SATA RAID or multipath device when /boot is on such a device, rather than installing to the first hard disk.
-   Remove grub-gfxpayload-lists in situations where we need to remove grub-pc.
-   Remove 'quiet' from target system command line if debian-installer/quiet is set to false.
-   When /boot is on a loopback device (i.e. Wubi), install GRUB there.
-   Simplify /proc and /sys mounting; make sure they're consistently mounted for the entire life of grub-installer, and consistently unmounted on exit.
-   Only process the output of os-prober into boot menu entries if we're actually going to use them; if we're using GRUB 2 and os-prober is installed, then it will deal with this itself and do a better job of it.
-   Drop grub-installer/bootdev_directory preseeding. Wubi no longer uses this, and we no longer care about grub4dos.
-   If the SecureBoot EFI variable is set, then install grub-efi-amd64-signed rather than grub-efi, along with shim-signed if available.
-   Always use grub-efi-amd64-signed on 64bit UEFI systems instead of relying on SecureBoot detection. (LP: \#1184297)
-   Force grub-installer/bootdev to take precedence over only_debian and with_other_os. (LP: \#1012629)
-   If grub-installer/bootdev was set to (hd0), override it to the OS device name for the first disk if possible (LP: \#1292628).
-   Fall back to grub-pc if there is no /target/boot/efi directory (thanks, Phillip Susi; LP: \#1302418).

**Conclusion:**

hw-detect
---------

Detect hardware and load kernel drivers for it

***Current:*** 1.95ubuntu3

***Tanglu:*** 1.99

### Differences

***Versions:***

-   Languages

***Ubuntu:***

-   Exit zero if you continue all the way through ethdetect's errors about having no network interfaces.
-   Drop priorities of a couple of ethdetect questions to medium.
-   disk-detect.sh: Do not check the kernel command line for any option to enable dmraid support. If functional dmraid arrays are found, the user will be asked if they wish to activate them.
-   Stop installing acpi, acpid, and acpi-support-base if acpi is available.
-   Add an 'archdetect-deb' package, containing /usr/bin/archdetect. Add an archdetect(1) manual page.
-   Move firmware/injected drivers' .deb packages installation from post-base-installer.d stage to pre-pkgsel.d stage. (LP: \#1209287, LP: \#1216043)
-   Bump question to load driver injection disk from medium to high, and raise driver-injection-disk package priority to standard. Thus the udeb will be loaded by default, but before installing any debs from the OEMDRV a confirmation question will be asked (default true, can be pre-seeded)

**Conclusion:**

localechooser
-------------

choose language/country/locale

***Current:*** 2.49ubuntu5

***Tanglu:*** 2.63

### Differences

***Versions:***

-   Languages

***Ubuntu:***

-   Add a localechooser-data package containing lists useful for packages that create automatic installation scripts.
-   Simplify use of locale-gen using the new command-line argument facility in Ubuntu's locales package.
-   Add encoding field to languagelist file and use the full SUPPORTED file rather than SUPPORTED-short.
-   If the language question was already marked as seen and the answer didn't change, then don't recalculate the locale.
-   If OVERRIDE_SHOW_ALL_LANGUAGES is set in the environment, display all languages regardless of frontend.
-   Cancel any locale preseeding if the user changes the language.
-   For cases where selecting a different location may imply a different dialect of the language, i.e. Portuguese and Chinese, take care to set LANG and LANGUAGE to something reflecting the language and LC_NUMERIC, LC_TIME, LC_MONETARY, LC_PAPER, LC_NAME, LC_ADDRESS, LC_TELEPHONE, LC_MEASUREMENT, and LC_IDENTIFICATION to something reflecting the location.

~~\* Create skeleton locale-langpack subdirectories for each language being installed, so that accountsservice knows to offer the incomplete-language-support prompt (LP: \#1307983).~~

**Conclusion:** create a patch - ***done***.

netcfg
------

Configure the network

***Current:*** 1.116ubuntu1

***Tanglu:*** 1.117tanglu3

### Differences

***Versions:***

-   Lintian

***Ubuntu:***

-   Set priority for get_domain to high for static configurations.
-   Set priority for get_domain to medium for non-static configurations.
-   Use 'auto <interface>' for all interfaces, dropping allow-hotplug which doesn't work with current udev.
-   Set DHCP and DHCPv6 timeout to 30s.
-   Use isc-dhcp-client-udeb on all architectures.
-   Flush all addresses and routes before configuring interfaces (LP: \#848072)
-   Apply patch from Alec Warner making netcfg respect netcfg/dhcpv6_timeout and running dhclient in one-shot mode (-1). (LP: \#917905)
-   Fix FTBFS by checking the return value of fgets and fscanf.
-   Fix nm-conf to generate a valid NetworkManager static configuration file.

**Conclusion:**

partconf
--------

debian-installer utility for finding partitions and creating fstab file

***Current:*** 1.45

***Tanglu:*** 1.45

### Differences

***Versions:***

-   nothing

***Ubuntu:***

-   nothing

**Conclusion:** Nothing to do

partman-auto
------------

Automatically partition storage devices (partman)

***Current:*** 118ubuntu3

***Tanglu:*** 118

### Differences

***Versions:***

-   nothing

***Ubuntu:***

-   Recipe changes:
    -   Adjust default partition and EFI sizes for Ubuntu.
    -   Change the x86 atomic recipes to make the minimum swap size be 100% of RAM, so that hibernate always works.
    -   In addition to EFI partition, reuse existing swap partitions and BIOS Boot Partitions, if any, rather than creating new ones.

<!-- -->

-   Automatic Partitioning:
    -   Offer resize_use_free autopartitioning method, except on armel/armhf.
    -   Add an option to reuse an existing installation.
    -   Split out the replace option from resize_use_free.

<!-- -->

-   Code changes:
    -   Accept autopartitioning automatically rather than showing choose_partition.
    -   Drop priority of partman-auto/choose_recipe question to medium.
    -   Add support for partman-auto/method=loop via partman-auto-loop.
    -   Make biggest_free respect the selection made in partman-auto/disk.
    -   Exclude devices containing the installation medium from automatic partitioning.
    -   Use new get_real_resize_range function from partman-partitioning 72ubuntu3, which caches calls to tune2fs and ntfsresize.
    -   Support resizing the largest partition on multiple disks.
    -   Support formatting the entire partition for any partition that can be resized.
    -   Signal to clear_partitions in partman-target that it should not notify the user about the unformatted partitions we have set up.
    -   Add a hack to stop EFI System Partitions showing up as to-be-formatted in the confirm-changes screen when there's an existing filesystem that would cause partman-efi to skip them.
    -   Increase the EFI System Partition size slightly to ensure that it's at least 512MiB, not just 512MB. See LP \#1306164.

**Conclusion:**

partman-auto-crypto
-------------------

Automatically partition storage devices using crypto and LVM

***Current:*** 20ubuntu1

***Tanglu:*** 22

### Differences

***Versions:***

-   Language

***Ubuntu:***

-   Accept autopartitioning automatically rather than showing choose_partition.

**Conclusion:**

partman-auto-loop
-----------------

Automatically partition using a loop device

***Current:*** 0ubuntu21

***Tanglu:*** 0.21tanglu1 (uploaded 2012/07/12)

### Differences

***Versions:***

-   -/-

***Ubuntu:***

-   See [changelog](http://changelogs.ubuntu.com/changelogs/pool/main/p/partman-auto-loop/partman-auto-loop_0ubuntu21/changelog)

**Conclusion:** Nothing to do

partman-auto-lvm
----------------

Automatically partition storage devices using LVM

***Current:*** 53ubuntu1

***Tanglu:*** 53

### Differences

***Versions:***

-   -/-

***Ubuntu:***

-   Accept autopartitioning automatically.
-   Change fallback VG name to Ubuntu.
-   Ask how much of the VG should be used for logical volumes, rather than unconditionally using it all.
-   Set locale to "C" when calling vgs for available free space.

**Conclusion:**

partman-base
------------

Partition the storage devices (partman)

***Current:*** 172ubuntu1

***Tanglu:*** 173

### Differences

***Versions:***

-   Language

***Ubuntu:***

-   Ubiquity integration: If PARTMAN_NO_COMMIT is set, then exit rather than running commit.d and finish.d scripts; add a partman-commit script; dump extra information to /var/lib/partman/snoop if PARTMAN_SNOOP is set; check for per-menu 'no_show_choices' file in ask_user and don't reshow the menu if it exists.
-   Don't skip over dmraid devices if the user chooses not to activate them.
-   If the only thing mounted on a disk is the installation medium and it uses more or less the whole disk, then silently exclude that disk; if the installation medium is mounted but doesn't use the whole disk, issue a warning that partitioning may be difficult; if anything else is mounted, offer to unmount it. partman/filter_mounted=false disables this.
-   Use the device's logical sector size throughout rather than PED_SECTOR_SIZE_DEFAULT.

**Conclusion:** create a patch - ***done***.

partman-basicfilesystems
------------------------

Add to partman support for ext2, linux-swap, fat16 and fat32

***Current:*** 97ubuntu1

***Tanglu:*** 95 (Debian: [97](https://packages.debian.org/en/sid/partman-basicfilesystems) - uploaded 2014/07/12)

### Differences

***Versions:***

-   -/-

***Ubuntu:***

-   If partman/automount is preseeded to true, automatically mount partitions with usefully-mountable filesystems on subdirectories of /media.
-   Add minimal support for NTFS partitions using ntfs-3g.
-   Mount FAT filesystems at boot (fstab pass 1).
-   Mount FAT (other than EFI System Partitions) and NTFS with umask=007,gid=46 (static group plugdev).
-   When formatting over the top of an existing swap partition, preserve its UUID to avoid leaving systems that use UUIDs in /etc/fstab without swap.
-   mount.d/basic: Close mount's fd 3 so that it doesn't inherit a debconf file descriptor, to prevent log-output hanging when ntfs-3g is in use.
-   Special case loopmounted filesystems as it's safer to format the underlying file, not the device.
-   Allow armel/omap to use FAT for /boot, since the problems with it can be worked around while it's difficult to use anything else given uboot limitations.
-   Disable existing swap partitions before formatting them.

**Conclusion:**

partman-basicmethods
--------------------

Basic partition usage methods for partman

***Current:*** 58

***Tanglu:*** 58

### Differences

***Versions:***

-   -/-

***Ubuntu:***

-   -/-

**Conclusion:** Nothing to do

partman-btrfs
-------------

Add to partman support for btrfs

***Current:*** 14ubuntu2

***Tanglu:*** 15

### Differences

***Versions:***

-   -/-

***Ubuntu:***

-   Add support for creating and mounting btrfs subvolumes corresponding to / and /home, in case of a btrfs rootfs.

**Conclusion:**

partman-crypto
--------------

***Current:*** 63ubuntu2

***Tanglu:*** 73

### Differences

***Versions:***

-   Languages
-   Fixes

***Ubuntu:***

-   Disable partition erasing by default, as it's very slow and only of moderate value.
-   Warn that the passphrase cannot be recovered if lost.
-   Allow preseeding the first passphrase prompt and partman-crypto/weak_passphrase.
-   Add an "Activate existing encrypted volumes" option to the partman-crypto main menu.
-   Allow discard on luks devices.

**Conclusion:**

partman-efi
-----------

Add to partman support for EFI boot partitions

***Current:*** 25ubuntu6

***Tanglu:*** 41

### Differences

***Versions:***

-   Languages
-   EFI development work, merging code from Ubuntu
-   Fixes

***Ubuntu:***

-   Automatically use existing EFI system partitions on Intel Macs as EFI boot partitions.
-   Create fat32 EFI partitions with the name "EFI System Partition" by default on Intel Macs.
-   choose_method/efi/do_option: Make sure no mountpoint is set.
-   Remove efi-modules dependency; it seems to be built into Ubuntu kernels now.
-   Require partman-base &gt;= 129 to support a null value for the name.
-   Automatically mount the first method=efi filesystem we see on /boot/efi.
-   On x86 architectures, create EFI system partitions using FAT32 rather than FAT16, and require newly-created ones to have a minimum size of 34091008 bytes.
-   Never format EFI system partitions that already contain a filesystem.
-   Detect existing EFI system partitions more reliably on GPT partition tables (LP: \#972122, \#900245).
-   check.d/efi: Fix parsing bug in code to find the EFI System Partition size, spotted by Steve McIntyre.
-   Use mkdosfs to create FAT filesystems, since libparted cannot handle doing that on non-512-sector disks (LP: \#1065281).
-   Depend on dosfstools-udeb for the changes in 25ubuntu3.
-   Pass "-s 1" to mkdosfs if the logical sector size is not equal to 512 bytes, since mkdosfs has trouble with cluster calculations otherwise (LP: \#1065281).
-   Use mkfs.fat rather than mkdosfs if it exists (LP: \#1257702).

**Conclusion:**

partman-ext3
------------

Add to partman support for ext3 and ext4

***Current:*** 80ubuntu1

***Tanglu:*** 82

### Differences

***Versions:***

-   Languages

***Ubuntu:***

-   Special case loopmounted filesystems as it's safer to format the underlying file, not the device.
-   Add preseedable partman-ext3/lazy_itable_init question, which if true runs mkfs.ext\* with '-E lazy_itable_init', greatly speeding up mkfs on large drives. This defaults to false since it is currently unsafe for use on areas of disk that previously contained a filesystem.

**Conclusion:**

partman-jfs
-----------

Add support for jfs to partman

***Current:*** 43

***Tanglu:*** 43

### Differences

***Versions:***

-   -/-

***Ubuntu:***

-   -/-

**Conclusion:** nothing to do

partman-lvm
-----------

Adds support for LVM to partman

***Current:*** 90

***Tanglu:*** 90

### Differences

***Versions:***

-   -/-

***Ubuntu:***

-   -/-

**Conclusion:** nothing to do

<strike>partman-newworld
------------------------

partman support for new-world PowerMac boot partitions

***Current:*** 32

***Tanglu:*** -/- (Debian: [32](https://packages.debian.org/sid/partman-newworld)

### Differences

***Versions:***

-   -/-

***Ubuntu:***

-   -/- </strike>

**Conclusion:** not needed for i386 and x64 - removed.

partman-partitioning
--------------------

Partitioning operations for partman

***Current:*** 101ubuntu1

***Tanglu:*** 100 (Debian: [101](https://packages.debian.org/sid/partman-partitioning) - uploaded 2014/07/12)

### Differences

***Versions:***

-   Languages
-   Bugfix

***Ubuntu:***

-   Make sure to wipe disk label on Sun disks before creating a new one.
-   Depend on ntfs-3g-udeb as well as ntfsprogs-udeb.
-   Add PATH, RAWMINSIZE, RAWPREFSIZE, and RAWMAXSIZE substitutions to partman-partitioning/new_size in support of ubiquity's resize widget.
-   Cache calls to tune2fs and ntfsresize, to make navigating through the resize UI a little faster.
-   Check that minimum filesystem sizes reported by tune2fs and ntfsresize are between the minimum partition size and the current partition size; if not, refuse to resize the partition at all.
-   Use mac as the default disk label on ppc64.
-   Save the swap size for ubiquity.
-   On systems with only GPT disks, check that an EFI System Partition or a BIOS Boot Partition exists, as appropriate.
-   Detect "fsl" subarch for powerpc and set partition map to GPT.
-   Use GPT as the default disk label on ppc64el.

**Conclusion:**

partman-target
--------------

Provides partman with ability to prepare /target

***Current:*** 83ubuntu1

***Tanglu:*** 91

### Differences

***Versions:***

-   Languages
-   Bugfix (symlinks for /var/run and /var/lock)

***Ubuntu:***

-   Disable automatic mounting of USB removable devices.
-   Mount floppies with 'exec' and 'utf8'.
-   Use the path of the file associated with a loop device in /etc/fstab, rather than the filesystem's UUID or the loop device path.
-   Always set the loop option for loop devices.
-   Remove critical system files from the existing filesystem before installing.
-   Preserve the UID and GID of the initial user, if possible. Requires a patch to user-setup.
-   Check that all system partitions are formatted.
-   Stop adding removable devices to /etc/fstab, now that apt knows how to find CD-ROM devices using libudev.
-   Notify user-setup if there is an encrypted home partition present.

**Conclusion:**

<strike>partman-uboot
---------------------

partman for arm & mipsel architectures

***Current:*** 5

***Tanglu:*** -/-

### Differences

***Versions:***

-   -/-

***Ubuntu:***

-   See [changelog](http://changelogs.ubuntu.com/changelogs/pool/main/p/partman-uboot/partman-uboot_5/changelog)</strike>

**Conclusion:** Not needed: for arm & mipsel - removed. Will insert again if armhf is available in Tanglu.

partman-xfs
-----------

Add to partman support for xfs

***Current:*** 53

***Tanglu:*** 53

### Differences

***Versions:***

-   -/-

***Ubuntu:***

-   -/-

**Conclusion:** Nothing to do

preseed
-------

debconf preseeding via environment variables

***Current:*** 1.64ubuntu1

***Tanglu:*** 1.64

### Differences

***Versions:***

-   -/-

***Ubuntu:***

-   Change the "keymap" alias to keyboard-configuration/layoutcode.
-   Change default preseed root to "d-i/utopic/./preseed.cfg".

**Conclusion:**

tzsetup
-------

choose time zone

***Current:*** 1:0.26ubuntu12

***Tanglu:*** 1:0.57

### Differences

***Versions:***

-   Languages
-   Bugfixes

***Ubuntu:***

-   Make it possible to select from a worldwide list of timezones if the default list is insufficient.
-   Offer UTC in worldwide list of timezones.
-   Support getting the timezone from a geoip server (LP: \#229884)

**Conclusion:** create a patch - ***done*** (but I don't know whether it's needed :-/ )

user-setup
----------

Set up initial user and password

***Current:*** 1.48ubuntu2

***Tanglu:*** 1.56tanglu1

### Differences

***Versions:***

-   Languages
-   Fixes

***Ubuntu:***

-   Add the initial user to the adm, lpadmin, and sambashare groups too. Do not add them to the audio, video, floppy, netdev, powerdev, scanner, or bluetooth groups.
-   Default passwd/root-login to false.
-   Make is_system_user always return false if OVERRIDE_SYSTEM_USER is set.
-   Add preseedable passwd/auto-login question; if set to true, configure gdm, kdm, lxdm, and lightdm for automatic login. Add passwd/auto-login-backup question which backs up the previous contents of the files as well.
-   Ask whether the user wants to encrypt their home directory.
-   Allow forcing the encrypted home option.
-   If a user requests an encrypted-home, we must have their login passphrase, in order to wrap their mount passphrase; it's fundamentally incompatible to preseed encrypted-home AND a crypted password; if this happens, send the user back to password selection.
-   Zero out swap devices at the end of install when encryption is enabled.
-   Provide a progress message for wiping swap space.
-   If user-setup/allow-password-empty is preseeded to true, allow empty passwords.
-   Disable installation of pre-pkgsel.d/10kdesudo; it does nothing for Ubuntu, and causes a confusing message that worries some people.
-   Add weak password detection (purely length-based for now, matching partman-crypto).
-   Consider a password of '!' in shadow for root to be unset.
-   Don't restrict guest login from login screen if autologin was configured, just restrict autologin for guest specifically.
-   If OVERRIDE_ALREADY_ENCRYPTED_SWAP is set in the environment, assume that encrypted swap has already been set up rather than re-creating and re-zeroing swap.
-   Add the initial user to the libvirtd group (LP: \#1304008).
-   Add maas to reserved-usernames (LP: \#1069684).

**Conclusion:**

<strike>yaboot-installer
------------------------

tool for installing yaboot, on NewWorld PowerMac systems.

***Current:*** 1.1.29ubuntu1

***Tanglu:*** -/-

### Differences

***Versions:***

-   -/-

***Ubuntu:***

-   -/-</strike>

**Conclusion:** Not needed: PowerMac systems - removed.

Documentation
=============

Links and hints found in different documents to help understanding the working of ubiquity.

README in doc/
--------------

[README online](http://bazaar.launchpad.net/~ubuntu-installer/ubiquity/trunk/view/head:/doc/README)

[User interface design - old (2005)](http://wiki.ubuntu.com/UbuntuExpress)

[debconf-devel(7)](http://manpages.ubuntu.com/manpages/trusty/en/man7/debconf-devel.7.html)

[The presentation layer of partman - old (2006)](http://wiki.ubuntu.com/Ubiquity/AdvancedPartitionerRewrite)

[manpage](http://manpages.ubuntu.com/manpages/trusty/man8/ubiquity.8.html)

Debugging
---------

-   [gnewsense porting guide](http://www.gnewsense.org/DevelopmentTeam/Ubiquity) - is marked as outdated but perhaps helpful.
-   [Cusomize Ubiquity installer](http://www.mybinarylife.net/2011/07/ubuntu-1104-custom-ubiquity-installer.html) - This post is a mini how to guide running the Ubuntu 11.04 installer from a flash drive and debugging it.
-   [DebuggingUbiquity](https://wiki.ubuntu.com/DebuggingUbiquity) - Ubuntu Wiki.
-   [Hacking on Ubiquity, the setup](http://agateau.com/2013/hacking-on-ubiquity-the-setup/) - hacking inside of a VM. could be interesting ...

Upstart / Systemd
-----------------

Documentation for porting upstart parts to systemd.

### Upstart

-   [Getting started](http://upstart.ubuntu.com/getting-started.html)
-   [UpstartHowto](https://help.ubuntu.com/community/UpstartHowto)
-   [Cookbook](http://upstart.ubuntu.com/cookbook/)
-   [Ubuntu startup – init scripts, runlevels, upstart jobs explained](http://www.pathbreak.com/blog/ubuntu-startup-init-scripts-runlevels-upstart-jobs-explained)

### Systemd

-   [Getting Started with systemd](http://coreos.com/docs/launching-containers/launching/getting-started-with-systemd/)
-   [Opensuse Reference - The systemd daemon](http://activedoc.opensuse.org/book/opensuse-reference/chapter-8-the-systemd-daemon)
-   [Redhat System Administration Guide - MANAGING SERVICES WITH SYSTEMD](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/System_Administrators_Guide/chap-Managing_Services_with_systemd.html)
