---
title: AequoreaTaskList
import:
  - templates/tasktable

core-tasks:
  - done: yes
    component: systemd
    description: Systemd integration into the Tanglu base-system
    contact: Matthias Klumpp (@ximion)

  - done: yes
    component: systemd
    description: Transition to libudev1
    contact: Matthias Klumpp (@ximion)

  - done: yes
    component: live-build
    description: Create a Live-CD for Tanglu
    contact: Rohan Garg (@shadeslayer)

  - inprogress: yes
    component: debian-installer
    description: Make Debian-Installer work for Tanglu
    contact: Philip Muskovac (@yofel)


kde-tasks:
  - todo: yes
    component: kde-base
    description: Package/sync a more recent KDE to Tanglu
    contact: Nobody (@null)


gnome-tasks:
  - todo: yes
    component: gnome-core
    description: Package/sync a more recent GNOME for Tanglu.
        If that does not work in time, stay with old GNOME.
    contact: Nobody (@null)


infrastructure-tasks:
  - done: yes
    component: dak
    description: Create Tanglu archive, automatize most functions,
        bootstrap Tanglu from Debian Testing.
    contact: Matthias Klumpp (@ximion)

  - done: yes
    component: jenkins
    description: Set up usable package autobuilding service.
    contact: Matthias Klumpp (@ximion)

  - todo: yes
    component: gosa²
    description: Set up developer management and a public Tanglu
        developer directory
    contact: Matthias Klumpp (@ximion)

  - done: yes
    component: wiki
    description: Restore wiki
    contact: Matthias Klumpp (@ximion)

  - done: yes
    component: ftp-master
    description: Display public information about the Tanglu archive
        status
    contact: Matthias Klumpp (@ximion)

  - inprogress: yes
    component: britney
    description: Implement Tanglu package staging area
    contact: Matthias Klumpp (@ximion)
  

artwork-tasks:
  - done: yes
    component: plymouth
    description: Design a Tanglu plymouth theme
    contact: Nobody (@null)

  - done: yes
    component: wallpaper
    description: Create a Tanglu wallpaper
    contact: Ink Apnea (@inkapnea)

---

Tasks for the Aequorea release
==============================

This is an (incomplete) list of stuff which needs to be done for Tanglu Aequorea, or for the Tanglu project in general.

Core
----

{{> templates/tasktable tasks=core-tasks}}

Desktop/KDE
-----------

{{> templates/tasktable tasks=kde-tasks}}

Desktop/GNOME
-------------

{{> templates/tasktable tasks=gnome-tasks}}

Infrastructure
--------------

{{> templates/tasktable tasks=infrastructure-tasks}}

Artwork
-------

{{> templates/tasktable tasks=artwork-tasks}}


---
[category:TaskList](/TaskLists) | [category:Aequorea](/Aequorea)