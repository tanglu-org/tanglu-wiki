---
title: DasyatisTaskList
import:
  - templates/tasktable

core-tasks:
  - inprogress: yes
    component: live-build / installer
    description: Implement UEFI support
        (see [TT:#159](https://tracker.tanglu.org/T159))
    contact: Matthias Klumpp (@ximion)

  - done: yes
    component: gcc5
    description: Complete the GCC5/libstdc++6 ABI transition
        ([TT:#146](https://tracker.tanglu.org/T146))
    contact: Matthias Klumpp (@ximion)

  - done: yes
    component: live-build
    description: Use gfxboot for the live-cd menu.
    contact: Matthias Klumpp (@ximion)

  - done: yes
    component: casper
    description: Make casper manage the live-cd, replacing
        live-boot.
    contact: Matthias Klumpp (@ximion)

  - inprogress: yes
    component: linux
    description: Ship with Linux 4.4 or 4.5 (needs to be decided)
    contact: Matthias Klumpp (@ximion)


kde-tasks:
  - todo: yes
    component: kde-base
    description: Package/sync a more recent KDE to Tanglu
    contact: Nobody (@null)

  - done: yes
    component: kde
    description: Merge in Debian changes to KDE packages, drop
        the Tanglu delta acquired when updating Chromodoris to KF5.
    contact: Matthias Klumpp (@ximion)


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

  - done: yes
    component: synchrotron
    description: Make synchrotron detect cruft packages in the
        development suite, make it more efficient, run syncs daily.
    contact: Matthias Klumpp (@ximion)

  - inprogress: yes
    component: synchrotron
    description: Make collision reports more useful, allow ignoring
        cruft / make cruft report more useful.
    contact: Matthias Klumpp (@ximion)

  - todo: yes
    component: synchrotron
    description: When syncing with `testing`, and dak already knows the
        version which is to-be-synced, import the existing packages
        instead. If that fails, create collision report.
    contact: Matthias Klumpp (@ximion)

  - todo: yes
    component: synchrotron
    description: Make manual syncs by Tanglu Developers work again.
    contact: Matthias Klumpp (@ximion)

  - todo: yes
    component: janitor
    description: Use synchrotron data for generating automatic
        removal hints.
    contact: Matthias Klumpp (@ximion)

  - todo: yes
    component: dak
    description: Create separate section for non-free firmware.
        (=> We are waiting on Debian for a decision.)
    contact: Matthias Klumpp (@ximion)

  - todo: yes
    component: dak
    description: Ship DEP-11 data in the archive.
    contact: Matthias Klumpp (@ximion)

  - todo: yes
    component: phabricator
    description: Export bug data for the Debian PTS to use.
    contact: Matthias Klumpp (@ximion)

  - inprogress: yes
    component: web
    description: Use Let's Encrypt to deliver Tanglu websites via
        HTTPS.
    contact: Matthias Klumpp (@ximion)


artwork-tasks:
  - todo: yes
    component: wallpaper
    description: Create a Tanglu wallpaper
    contact: Nobody (@null)

  - todo: yes
    component: calamares-branding
    description: Create new slideshow for the live-cd installers on
        KDE and GNOME.
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