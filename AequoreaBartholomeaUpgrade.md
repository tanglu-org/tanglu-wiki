---
title: AequoreaBartholomeaUpgrade
permalink: /AequoreaBartholomeaUpgrade/
---

__NOTOC__

Upgrade Tanglu 1 to Tanglu 2
============================

This page describes the steps necessary to upgrade from Tanglu 1 (Aequorea victoria) to Tanglu 2 (Bartholomea annulata)

To upgrade from Aequorea to Bartholomea perform the following steps:

#### Step 1

Make a backup first. The probability of data loss during a system upgrade is small, but non-zero, and configuration files very well may get overwritten or rewritten as part of the upgrade.

#### Step 2

Write down on which drive your current grub is installed (might be needed during the upgrade, if you did not install using d-i). You can get this information using

    $ sudo fdisk -l

The '\*' shows you the drive (without the number).

#### Step 3

Make a copy of your current /etc/apt/sources.list:

    $ sudo cp /etc/apt/sources.list{,.bak}

#### Step 4

Exchange all occurences of <strong>aequorea</strong> with <strong>bartholomea</strong> in your sources.list.

#### Step 5

Update your Apt cache for Bartholomea:

    $ sudo apt-get update

#### Step 6

Download all packages that will be needed for the upgrade:

    $ sudo apt-get --download-only dist-upgrade

#### Step 7

Make sure one of the Tanglu metapackages is installed, depending on your configuration. For non-GUI systems, that is "tanglu-standard", for KDE, the package is named "tanglu-kde", for GNOME "tanglu-gnome":

    $ sudo apt-get install tanglu-kde

or

    $ sudo apt-get install tanglu-gnome

#### Step 8

Close all open programs. Optionally switch to a different TTY or event stop your display-manager completely. Example for KDM:

    $ sudo systemctrl stop kdm

#### Step 9

Run

    $ sudo apt-get dist-upgrade

to perform the actual upgrade.

#### Step 10

Reboot, keep your fingers crossed and enjoy Tanglu 2.0.