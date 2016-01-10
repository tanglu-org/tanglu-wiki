---
title: BuildLiveImage
permalink: /BuildLiveImage/
---

Environment Setup
-----------------

Live images have to be built from a Tanglu install, if you aren't running Tanglu yourself you will need a debootstrap chroot. If you are running debian/ubuntu or related you first need to install a current version of debootstrap with Tanglu support from the [archive](http://archive.tanglu.org/tanglu/pool/main/d/debootstrap/). Then you can set the environment up (the commands expect that you're root):

-   debootstrap $RELEASE live_chroot

where RELEASE is the release you want to build the image for

-   chroot live_chroot
-   mount -t devpts devpts /dev/pts
-   mount -t proc proc /proc
-   apt-get install git

NOTES: Make sure the partition on which you build the rootfs is not mounted with nodev.

Build the image
---------------

First we need to get the live-build configuration from Gitlab:

-   git clone <git://github.com/tanglu-org/live-build.git>
-   cd live-build
-   sh install-prerequisites.sh

You can edit the configuration in auto/config if you want to build a customized image. Otherwise continue:

-   lb clean
-   lb config
-   lb build

Now wait as the image is built (that might take quite a while), the resulting image will be called *live-image.hybrid.iso* in the current directory. If you change the configuration and want to update the image you always need to run all 3 commands.

Building different image flavors
--------------------------------

The image building configuration supports different flavors by setting the FLAVOR environment variable. To build a different flavour just run

-   export FLAVOR=gnome

before running 'lb config'. If nothing is set if defaults to 'kde'.