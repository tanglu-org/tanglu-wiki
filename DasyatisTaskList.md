---
title: DasyatisTaskList
import:
  - templates/tasktable

core-tasks:
  - inprogress: yes
    component: live-build / installer
    description: Implement UEFI support
        (see [TT: #159](https://tracker.tanglu.org/T159))
    contact: Matthias Klumpp (@ximion)


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
    description: Support debug symbols (dbgsym) packages at least in
        the `staging` suite.
    contact: Matthias Klumpp (@ximion)

  - done: yes
    component: wiki
    description: Integrate wiki with the Tanglu Tracker (shared login)
        and maybe migrate it away from MediaWiki (or alternatively
        switch to MariaDB, so the antispam plugins work)
    contact: Matthias Klumpp (@ximion)


artwork-tasks:
  - todo: yes
    component: wallpaper
    description: Create a Tanglu wallpaper
    contact: Nobody (@null)

---

Tasks for the Dasyatis (T4) release
===================================

This is an (incomplete) list of stuff which needs to be done in the Tanglu 4 development cycle.

## Core

{{> templates/tasktable tasks=core-tasks}}


## Desktops

### KDE

{{> templates/tasktable tasks=kde-tasks}}

### GNOME

{{> templates/tasktable tasks=gnome-tasks}}


## Infrastructure

{{> templates/tasktable tasks=infrastructure-tasks}}


## Artwork

{{> templates/tasktable tasks=artwork-tasks}}


---
[category:TaskList](/TaskLists) | [category:Dasyatis](/Dasyatis)