---
title: SetUpPBuilderEnvironment
permalink: /SetUpPBuilderEnvironment/
---

Localizations: [Español](/PBuilderEnvironmentSetup-Quick/es)

Create Tanglu pBuilder Environment
==================================

This page explains how to set up a Tanglu pBuilder environment.

Prerequisites
-------------

On Tanglu, everything you need is already preconfigured, you just need to install pbuilder:

``` bash
sudo apt-get install pbuilder debootstrap devscripts
```

If you are not on a Tanglu distribution, but on Debian or a derivative (e.g. Ubuntu), do:

1.  Download & install the [debootstrap package](http://archive.tanglu.org/tanglu/pool/main/d/debootstrap/) from Tanglu
2.  Download & install the Tanglu [archive keyring package](http://archive.tanglu.org/tanglu/pool/main/t/tanglu-archive-keyring/).

\# Install pbuilder:

``` bash
sudo apt-get install pbuilder devscripts
```

Create the pBuilder environment
-------------------------------

If you are on Tanglu, just do

``` bash
sudo pbuilder create --distribution aequorea --debootstrapopts --variant=buildd
```

If you are not on Tanglu, try the following command:

``` bash
sudo pbuilder create --distribution aequorea --debootstrapopts --variant=buildd --mirror http://archive.tanglu.org/tanglu --debootstrapopts "--keyring=/usr/share/keyrings/tanglu-archive-keyring.gpg"
```

Always replace "aequorea" with the Tanglu release you want to build for.

And that's it, you should have a working Tanglu pbuilder environment now!

Development: Add staging suite
------------------------------

If you want to upload packages to the Tanglu development version, you need to add the staging suite.

To do so, follow these steps:

1. Login into pbuilder:



``` bash
DIST=aequorea pbuilder --login --save-after-login
```

2. Edit the /etc/apt/sources.list file, adding the following entries:



``` apt_sources
deb http://archive.tanglu.org/tanglu staging main contrib non-free
```

``` apt_sources
deb-src http://archive.tanglu.org/tanglu staging main contrib non-free
```

3. Exit the pbuilder environment

4. Update your environment:



``` bash
DIST=aequorea pbuilder --update
```

