---
title: BartholomeaChromodorisUpgrade
permalink: /BartholomeaChromodorisUpgrade/
---

__NOTOC__

Upgrade Tanglu 2 to Tanglu 3
============================

This page describes the steps necessary to upgrade from Tanglu 2 (Bartholomea annulata) to Tanglu 3 (Chromodoris willani)

To upgrade from Bartholomea to Chromodoris perform the following steps:

#### Step 1

Make a backup first. The probability of data loss during a system upgrade is small, but non-zero, and configuration files very well may get overwritten or rewritten as part of the upgrade. Especially the transition to Plasma 5 on the KDE flavor is a delicate thing.

#### Step 2

Write down on which drive your current grub is installed (might be needed during the upgrade, if you did not install using d-i). You can get this information using

    $ sudo fdisk -l

The '\*' shows you the drive (without the number).

#### Step 3

Make a copy of your current /etc/apt/sources.list:

    $ sudo cp /etc/apt/sources.list{,.bak}

#### Step 4

Exchange all occurrences of <strong>bartholomea</strong> with <strong>chromodoris</strong> in your sources.list.

#### Step 4.1

For users of the Tanglu KDE flavor (but might apply to GNOME as well, depending on the installed packages): You **must** ensure that the *chromodoris-updates* package source is activated in sources.list, otherwise the upgrade will fail!

#### Step 5

Update your Apt cache for Chromodoris:

    $ sudo apt update

#### Step 6

Download all packages that will be needed for the upgrade:

    $ sudo apt-get --download-only dist-upgrade

#### Step 7

Make sure one of the Tanglu metapackages is installed, depending on your configuration. For non-GUI systems, that is "tanglu-standard", for KDE, the package is named "tanglu-kde", for GNOME "tanglu-gnome":

    $ sudo apt install tanglu-kde

or

    $ sudo apt install tanglu-gnome

#### Step 8

Close all open programs. Optionally switch to a different TTY (CTRL+ALT+F1).

#### Step 8.1

If you are using the KDE flavor: Run the following command, which should cause the removal of the old settings package and the old *baloo4* package.

    $ sudo apt install tanglu-kde tanglu-kde-settings baloo-kf5

#### Step 9

Run

    $ sudo apt-get dist-upgrade

to perform the actual upgrade.

#### Step 10

Reboot, keep your fingers crossed and enjoy Tanglu 3.0.

[category:Chromodoris](/category:Chromodoris "wikilink")