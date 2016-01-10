---
title: HowtoInstallTanglu
permalink: /HowtoInstallTanglu/
---

Translation: [Français](/HowtoInstallTanglu/fr "wikilink")

How to install Tanglu
=====================

Currently, there is no live CD or debian-installer images available for Tanglu. Therefore, it is only possible to install Tanglu by specifically upgrading your Debian distribution to Tanglu.

Here are the steps:

1. Install Debian Wheezy (a.k.a. stable) onto a real or virtual machine. Debian Sid (a.k.a. unstable) is NOT recommended due to the fact that Tanglu is based mainly on Debian's testing distribution (with parts from unstable and experimental).

2. With root permissions, modify your

``` bash
/etc/apt/sources.list
```

to:

``` bash
deb http://archive.tanglu.org/tanglu aequorea main
deb-src http://archive.tanglu.org/tanglu aequorea main
```

If you want non-free software, include the *contrib* and *non-free* components too. (Note that the non-free section is needed to get all firmware.)

3. Assuming that you have sudo powers, run in a terminal:

``` bash
sudo apt-get update
sudo apt-get dist-upgrade
sudo apt-get install tanglu-minimal
```

This will give you a Tanglu Aequorea Victoria (1.1) system.