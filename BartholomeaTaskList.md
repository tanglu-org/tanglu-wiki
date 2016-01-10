---
title: BartholomeaTaskList
import:
  - templates/tasktable

core-tasks:
  - done: yes
    component: systemd
    description: Fix Debian bug#738965 in Tanglu
        (and ideally Debian as well) by providing systemd units
    contact: Nobody (@null)

  - inprogress: yes
    component: debian-installer
    description: Provide a working debian installer
    contact: Philip Muskovac (@yofel)


kde-tasks:
  - done: yes
    component: kde-base
    description: Package/sync a more recent KDE to Tanglu
    contact: Nobody (@null)


gnome-tasks:
  - done: yes
    component: gnome-core
    description: Package/sync a more recent GNOME to Tanglu
    contact: Nobody (@null)


infrastructure-tasks:
  - done: yes
    component: debile
    description: Make debile work for Tanglu.
    contact: Jon Severinsson (@jonno),
             Matthias Klumpp (@ximion)

  - todo: yes
    component: FAS
    description: Create Tanglu account manager
        (based on Fedora's FAS maybe)
    contact: Matthias Klumpp (@ximion)


artwork-tasks:
  - inprogress: yes
    component: plymouth
    description: Create a Tanglu Bartholomea wallpaper
    contact: Thomas Funk (@gonzomcgraw)
---

Tasks for the Tanglu 2 (Bartholomea Annulata) release
=====================================================

This is an (incomplete) list of stuff which needs to be done for Bartholomea, or for the Tanglu project in general.

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
[TaskLists](/TaskLists) | [Bartholomea](/Bartholomea)
