---
title: Building Packages
permalink: /Building_Packages/
---

Preface
=======

In this Howto I will try

-   To show how to build correct packages for Tanglu using the pbuilder environment from [here](/Setup_pBuilder_Develop_Environment "wikilink").
-   Where are the pitfalls.
-   What some errors mean.
-   How to get an unbuilable package working.

Required Tools
==============

There are several tools needed on the host for proper work. The easiest way is to install **packaging-dev**:

    $ sudo apt install packaging-dev

This metapackage depends on common packages useful for the development of Debian-format packages, including patch management systems, build systems, packaging macros, helpful scripts for developers, and tools for building and testing packages. See the [list](http://packages.tanglu.org/aequorea/packaging-dev) to know which packages.

Types of Packages
=================

There're two types of packages: binary and source packages. Binary packages are architecture depended and source package not.

When will we create which type?

Binaries
--------

They're needed only in **one** case: if no source package of a program/library has ever been uploaded to the archive. Then one binary of a supported architecture can be uploaded.

Sources
-------

They're needed for newer versions and build fixes.

Rebuild Packages
================

This is the easiest step of all - building a package from the official Tanglu sources.

We will build grep, a utility to search for text in files from source.

On Tanglu
---------

You can download source packages directly from Tanglu. To check if you can download, you will need an uncommented deb-src line in /etc/apt/sources.list. It should look like:

    deb-src http://archive.tanglu.org/tanglu [distribution] main contrib non-free

Replace \[distribution\] with the version you are using (i.e. aequorea or bartholomea). If the line above is there, but is commented out, you must uncomment it. Make sure your package index files are synchronized so that apt-get knows where to find the sources.

After you ensured the deb-src line is correct and uncommented, run:

    $ sudo apt-get update

Let's download the **grep** source package from the Tanglu repository:

    $ apt-get source grep

It downloads the .dsc file, .debian.tar.gz/xz, .orig.tar.gz/xz and unpack both .tar.gz/xz files into a grep source directory.

On Debian/Ubuntu
----------------

Here you have to do a little bit more:

First use **dget** to download the sources. Therefore you have to go to [Tanglu's Package Search](http://packages.tanglu.org/). Search for the grep package, go there and copy the link address of the .dsc file on the right side under "Download Source Package grep:".

Now paste the link behind dget:

    $ dget http://archive.tanglu.org/tanglu/pool/main/g/grep/grep_2.16-1.dsc

dget downloads then the .dsc file, .debian.tar.gz and .orig.tar.gz.

Second unpack both .tar.gz files into a bc source directory with **dpkg-source**:

    $ dpkg-source -x grep_2.16-1.dsc

Building the Package
--------------------

Now you can build the package using your chroot cleanroom you created with the command:

**For i386**:

    $ linux32 sudo DIST=aequorea ARCH=i386 pbuilder build grep_2.16-1.dsc

**For amd64**:

    $ sudo DIST=aequorea ARCH=amd64 pbuilder build grep_2.16-1.dsc

Once the packages are successfully built, the binary and source packages will be stored in /var/cache/pbuilder/\[distribution\]-\[architecture\]/result/

|                                        |                                                                                                                                                                                                                        |
|----------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [left|Note](/File:Note.png "wikilink") | <span style="color:blue">If you on e.g. an amd64 architecture you can omit ARCH=amd64. The same with ARCH=i386 on a i386 architecure. If your running distribution is Aequorea you can omit DIST=aequorea, too.</span> |

Debuild Packages
================

What does this mean - debuild? From the manpage of debuild:

> **debuild** creates all the files necessary for uploading a Debian package. It first runs **dpkg-buildpackage**, then runs **lintian** on the .changes file created (assuming that lintian is installed), and finally signs the .changes and/or .dsc files as appropriate (using debsign(1) to do this instead of dpkg-buildpackage(1) itself; all relevant key-signing options are passed on).

Or in other words it will be used to create fresh source and binary packages (with all needed files) if you have changed something compared to the original (e.g. add missing dependencies, add/remove a patch, etc).

With pbuilder we use **pdebuild** instead of debuild - it is the pbuilder way of doing debuild. It comes along with the pbuilder package. pdebuild runs essentially in the same way as debuild.

As an example we will build a package of grep from Debian unstable for aequorea. Getting the sources is for Tanglu and Debian/Ubuntu the same - go to [Debian's Package Search](https://www.debian.org/distrib/packages). Search for the grep package in unstable, go there and copy the link address of the .dsc file on the right side under "Download Source Package grep:".

Paste the link behind dget:

    $ dget http://ftp.de.debian.org/debian/pool/main/g/grep/grep_2.18-2.dsc

Unpack both .tar.gz files into a bc source directory with **dpkg-source**:

    $ dpkg-source -x grep_2.18-2.dsc

### Using dch

The next steps differ from [Rebuild Packages](/#Rebuild_Packages "wikilink"). As this grep is a foreign package we have to add a new comment in the Changelog. Therefore we change into the grep directory

    $ cd grep_2.18/

and run **dch** (debchange or its alias dch) which will add a new comment line to the Debian changelog.

    $ dch --vendor=Tanglu --distribution staging -R "No-change rebuild for Tanglu."

|                                        |                                                                                                                                                                                                                                                                                                   |
|----------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [left|Note](/File:Note.png "wikilink") | <span style="color:blue">Following the <ins>[Tanglu Packaging Policy](/Policy/packaging "wikilink")</ins> section 3 the package gets a **b1** after its version and **"No-change rebuild for Tanglu."** as a comment. Also, as it is a new package we use **staging** instead of aequorea.</span> |

As you open debian/changelog you see such like this on the first place:

    grep (2.18-2b1) staging; urgency=medium

      * No-change rebuild for Tanglu.

     -- Your Name <you@example.com>  Sun, 08 Jun 2014 00:43:43 +0200

For more information about dch see it's [manpage](http://manpages.ubuntu.com/manpages/trusty/en/man1/dch.1.html)

Another part which have to be changed is the **maintainer** in debian/control. Change it to the following:

    Maintainer: Tanglu Developers <tanglu-devel-discuss@lists.tanglu.org>

Ok, let's start debuilding (we're still in the grep directory!):

**For i386**:

    $ linux32 sudo DIST=aequorea ARCH=i386 pdebuild

**For amd64**:

    $ sudo DIST=aequorea ARCH=amd64 pdebuild

### Missing Packages

**Grrrr...** both ends up in an unsatisfy build-dependency:

     -> Considering build-dep libpcre3-dev (>= 1:8.31-3)
          Tried versions: 1:8.31-2build1
       -> Does not satisfy version, not trying
    E: Could not satisfy build-dependency.
    E: pbuilder-satisfydepends failed.

That means we need a newer libpcre3-dev than aequorea provides. So, let's downloading libpcre3-dev &gt;= 1:8.31-3 from Debian and debuild this library, too:

    $ dget http://ftp.de.debian.org/debian/pool/main/p/pcre3/pcre3_8.31-5.dsc
    $ dpkg-source -x pcre3_8.31-5.dsc
    $ cd pcre3-8.31/
    $ dch --vendor=Tanglu --distribution staging -R "No-change rebuild for Tanglu."
    $ linux32 sudo DIST=aequorea ARCH=i386 pdebuild

or

    $ sudo DIST=aequorea ARCH=amd64 pdebuild

**Great!** debuild was successfull and we can carry on with grep :-)

    $ cd grep-2.18/
    $ linux32 sudo DIST=aequorea ARCH=i386 pdebuild

or

    $ sudo DIST=aequorea ARCH=amd64 pdebuild

If all works well you have created your first package and as bonus a library, too!

Sign Packages
=============

Whether a package is debuilt successfully we can sign it. For that we use **[debsign](http://manpages.ubuntu.com/manpages/trusty/en/man1/debsign.1.html)** and your public key is needed which you get it with

    $ gpg --list-keys

We need the red part:

/home/user/.gnupg/pubring.gpg
--------------------------------
pub 4096R/<span style="color:red; font-weight:bold">ABCDFE01</span> 2008-04-13
uid firstname lastname (description)
sub 4096R/DEFABC01 2008-04-13

    $ debsign -kABCDFE01 /var/cache/pbuilder/[distribution]-[architecture]/result/*.dsc

    You need a passphrase to unlock the secret key for
    user: "Your Name <you@example.com>"
    4096-Bit DSA key, ID ABCDFE01, created 2008-04-13

    Successfully signed dsc files

    $ debsign -kABCDFE01 /var/cache/pbuilder/[distribution]-[architecture]/result/*.changes

    You need a passphrase to unlock the secret key for
    user: "Your Name <you@example.com>"
    4096-Bit DSA key, ID ABCDFE01, created 2008-04-13

    Successfully signed changes files

Now all packages are signed and ready for "shipping" ;-)

If you want to save yourself the hassle of specifying the command arguments every time you run debsign, modify the configuration file(s) based on the instructions listed at the bottom of the debsign manpage (see below). Set DEBSIGN_PROGRAM=gpg, DEBSIGN_SIGNLIKE=gpg and DEBSIGN_KEYID=ABCDFE01 in one of the configurations files. This will provide the necessary parameters for debsign to execute properly.

    CONFIGURATION VARIABLES
           The  two configuration files /etc/devscripts.conf and ~/.devscripts are
           sourced in that order to set  configuration  variables.   Command  line
           options  can be used to override configuration file settings.  Environ‐
           ment variable settings are ignored for  this  purpose.   The  currently
           recognised variables are:

           DEBSIGN_PROGRAM
                  Setting this is equivalent to giving a -p option.

           DEBSIGN_SIGNLIKE
                  This  must be gpg or pgp and is equivalent to using either -sgpg
                  or -spgp respectively.

           DEBSIGN_MAINT
                  This is the -m option.

           DEBSIGN_KEYID
                  And this is the -k option.

Debuild for staging
===================

Now I will show on the basis of **mplayer2** how to fix an unbuildable package for Tanglu's staging. For the sake of convenience I will show it on amd64 only.

|                                        |                                                                                                                                                                        |
|----------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [left|Note](/File:Note.png "wikilink") | <span style="color:blue">All new stuff goes in staging first and after a while, if all dependencies work, into the development version (currently bartholomea).</span> |

The very first we do is creating a staging chroot. Therefore follow the steps under [Setup pBuilder Develop Environment: Add staging suite](/#Add_staging_suite "wikilink").

Get information
---------------

If you want to help Tanglu the first thing is to look on [Tanglu's build-system](http://buildd.tanglu.org/) for broken packages. You can look in the list of [unbuilt](http://buildd.tanglu.org/jobs/unbuilt) jobs or search directly By source name. We do the second.

Click on the link under 'Source' to see which job failed. Interessting are the build jobs. Depending on what architecture you want to build follow the job link to the results.

To see what problem occurs while building click on the latest job log link and jump to the end. In our case there's a problem with libass:

    Error: Unable to find development files for libass. Aborting. If you really mean to compile without libass support use --disable-libass.

    Checking for SSA/ASS support ...
    Check "config.log" if you do not understand why it failed.
    make[1]: *** [override_dh_auto_configure] Error 1
    debian/rules:49: recipe for target 'override_dh_auto_configure' failed
    make[1]: Leaving directory '/«PKGBUILDDIR»'
    make: *** [build] Error 2
    debian/rules:46: recipe for target 'build' failed
    dpkg-buildpackage: error: debian/rules build gave error exit status 2

All right, let's go back to the mplayer overview page. On top you'll find the link to it's .dsc file. Copy the link and paste it behind dget to get the sources, unpack it and do a normal rebuild:

    $ dget http://archive.tanglu.org/tanglu/pool/main/m/mplayer2/mplayer2_2.0-728-g2c378c7-2.dsc
    $ dpkg-source -x mplayer2_2.0-728-g2c378c7-2.dsc
    $ sudo DIST=staging ARCH=amd64 pbuilder build mplayer2_2.0-728-g2c378c7-2.dsc

|                                        |                                                                                                                                         |
|----------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------|
| [left|Note](/File:Note.png "wikilink") | <span style="color:blue">In Tanglu **dget** do the job of unpacking, too. So you won't need to execute the **dpkg-source** step.</span> |

The same error is occured. Luckily we have added the hook script **C10shell** which brings us into the chroot. Now we can check config.log to understand why it failed.

At the end of config.log:

    ============ Checking for SSA/ASS support ============

    pkg-config --cflags libass
    Package harfbuzz was not found in the pkg-config search path.
    Perhaps you should add the directory containing `harfbuzz.pc'
    to the PKG_CONFIG_PATH environment variable
    Package 'harfbuzz', required by 'libass', not found

Ah, Package 'harfbuzz' is missing. After doing a search in [Tanglu's Package Search](http://packages.tanglu.org/) we see that harfbuzz is a library and it exists in Tanglu's develop version. If not we had checked Debian's Package search for libharfbuzz and debuilt it for Tanglu, too.

Fix Issue
---------

Leave the chroot with CTRL-AD (or 'exit') switch into mplayers source directory and open **debian/control**. Search for 'libharfbuzz' and if not found (hopefully) add 'libharfbuzz-dev' in the **Build-Depends**:

<strong>Build-Depends:</strong>
 autoconf,
 :
 libgl1-mesa-dev,
<span style="color:red; font-weight:bold"> libharfbuzz-dev,</span>
 libjack-dev,

As the package has changed it must observed in the changelog:

    dch --local tanglu --vendor=Tanglu --distribution staging

|                                        |                                                                                                                                                                                                                                                                                                                                           |
|----------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [left|Note](/File:Note.png "wikilink") | <span style="color:blue">Following the <ins>[Tanglu Packaging Policy](/Policy/packaging "wikilink")</ins> section 3 the package gets a **tanglu1** after its version and **"Add missing libharfbuzz-dev package needed by libass."** as a comment. Also, as it is a package for staging we use **staging** instead of bartholomea.</span> |

debian/changelog entry:

    mplayer2 (2.0-728-g2c378c7-2tanglu1) staging; urgency=medium

      * Add missing libharfbuzz-dev package needed by libass.

     -- Your Name <you@example.com>  Sun, 08 Jun 2014 17:11:31 +0200

Don't forget to change the **maintainer** in debian/control:

    Maintainer: Tanglu Developers <tanglu-devel-discuss@lists.tanglu.org>

Now we can start a debuild:

    sudo DIST=staging ARCH=amd64 pdebuild

Worked ^^. Last step before "shipping" is the creation of a source only package.

Source only Packages
--------------------

With the current pbuilder configuration source AND binary packages are built. But Tanglu needs source packages only. To get them we will use after a successful pdebuild the real **[debuild](http://manpages.ubuntu.com/manpages/trusty/en/man1/debuild.1.html)** (we're in the source directory allready!):

    $ debuild -S -sa -kABCDFE01

It will create a .sources.change, .dsc, .orig.tar.gz/xz, .debian.tar.gz/xz and sign the package (.changes and .dsc) in the upper level directory.

|                                        |                                                                    |
|----------------------------------------|--------------------------------------------------------------------|
| [left|Note](/File:Note.png "wikilink") | <span style="color:blue">If you have problems with permissions try

                                              $ sudo chown $USER:$USER ../*

                                          and start debuild again.</span>                                     |

Build inside Chroot
===================

Another possibility is to build a packages inside the chroot. For that we have to mount i.e. /tmp from the host:

    $ sudo DIST=staging pbuilder --login --bindmounts /tmp

Inside the chroot we're switching into /tmp

    root@pbuild1234:/ # cd /tmp

Now we can get the sources and install build dependencies

**For packages from deb-src:**

    root@pbuild1234:/tmp/ # apt-get source [package]

Installing build dependencies:

    root@pbuild1234:/tmp/ # apt-get build-dep [package.dsc]

Changing into the package source folder:

    root@pbuild1234:/tmp/ # cd [package-version]/

**For other packages:**

    root@pbuild1234:/tmp/ # dget [path_to_package.dsc]
    root@pbuild1234:/tmp/ # dpkg-source -x [package.dsc]

Changing into the package source folder:

    root@pbuild1234:/tmp/ # cd [package-version]/

Check build dependencies:

    root@pbuild1234:/tmp/[package-version]/ # dpkg-checkbuilddeps

To use the output with apt-get we must remove all brackets with their contents and decisions like **package1.2 | package1.1**.

Check which package is available:

    root@pbuild1234:/tmp/[package-version]/ # apt-cache pkgnames package1

Install the packages

    root@pbuild1234:/tmp/[package-version]/ # apt-get install [package_list]

Check after installation which packages should be newer to build the package:

    root@pbuild1234:/tmp/[package-version]/ # dpkg-checkbuilddeps

The listed packages have to be built, first. For that repeat the steps before.

Put every built deb package into the local repo, do a package scan and update apt-gets database:

    root@pbuild1234:/tmp/[package-version]/ # cp ../[built_packages].deb /var/cache/pbuilder/[pbuilder_environment]-[architecture]/results/
    root@pbuild1234:/tmp/[package-version]/ # cd /var/cache/pbuilder/[pbuilder_environment]-[architecture]/results/
    root@pbuild1234:/var/cache/pbuilder/[pbuilder_environment]-[architecture]/results/ # dpkg-scanpackages ./ > Packages && gzip -f Packages
    root@pbuild1234:/var/cache/pbuilder/[pbuilder_environment]-[architecture]/results/ # apt-get update

After finishing switch into the source directory of the package you wanted to built at the beginning:

    root@pbuild1234:/var/cache/pbuilder/[pbuilder_environment]-[architecture]/results/ # cd [package-version]/

Create a changelog entry:

    root@pbuild1234:/tmp/[package-version]/ # dch --vendor=Tanglu --distribution staging -R "No-change rebuild for Tanglu."

Start a debuild:

    root@pbuild1234:/tmp/[package-version]/ # debuild

Leave the chroot:

    root@pbuild1234:/tmp/[package-version]/ # logout

You will find the package under /tmp.

Upload Packages
===============

Until [Debexpo](https://wiki.debian.org/Debexpo) isn't running requests goes over IRC.

Ask about upload possibilities on \#tanglu-devel at freenode.

Reviewing
---------

To make life easier for package reviewer we will use **[debdiff](http://manpages.ubuntu.com/manpages/trusty/en/man1/debdiff.1.html)** to show the changes. With debdiff you can diff complete packages:

    $ debdiff [original.dsc] [changed.dsc]

With the mplayer example we get this output which can be sent then for reviewing:

    $ debdiff mplayer2_2.0-728-g2c378c7-2.dsc mplayer2_2.0-728-g2c378c7-2tanglu1.dsc

    diff -Nru mplayer2-2.0-728-g2c378c7/debian/changelog mplayer2-2.0-728-g2c378c7/debian/changelog
    --- mplayer2-2.0-728-g2c378c7/debian/changelog  2014-05-12 00:52:45.000000000 +0200
    +++ mplayer2-2.0-728-g2c378c7/debian/changelog  2014-06-08 17:16:12.000000000 +0200
    @@ -1,3 +1,9 @@
    +mplayer2 (2.0-728-g2c378c7-2tanglu1) staging; urgency=medium
    +
    +  * Add missing libharfbuzz-dev package needed by libass.
    +
    + -- Your Name <you@example.com>  Sun, 08 Jun 2014 17:11:31 +0200
    +
     mplayer2 (2.0-728-g2c378c7-2) unstable; urgency=low

       * Upload to unstable, closes: #739337
    diff -Nru mplayer2-2.0-728-g2c378c7/debian/control mplayer2-2.0-728-g2c378c7/debian/control
    --- mplayer2-2.0-728-g2c378c7/debian/control    2014-05-12 00:51:33.000000000 +0200
    +++ mplayer2-2.0-728-g2c378c7/debian/control    2014-06-08 20:11:42.000000000 +0200
    @@ -1,7 +1,7 @@
     Source: mplayer2
     Section: video
     Priority: extra
    -Maintainer: Debian Multimedia Maintainers <pkg-multimedia-maintainers@lists.alioth.debian.org>
    +Maintainer: Tanglu Developers <tanglu-devel-discuss@lists.tanglu.org>
     Uploaders:
      Alessio Treglia <alessio@debian.org>,
      Jonas Smedegaard <dr@jones.dk>,
    @@ -40,6 +40,7 @@
      libfribidi-dev,
      libgif-dev,
      libgl1-mesa-dev,
    + libharfbuzz-dev,
      libjack-dev,
      libjpeg-dev,
      liblcms2-dev,

Errors
======

Over time I have collected some sites with errors while building. Below they're listed. Nothing fancy but as indication.

Missing gcc-multilib
--------------------

If testing a package build in an i386 environment on an amd64 host it is possible that unexpected and hard-to-diagnose build errors will occur. I had issues with gnu/stubs-64.h missing trying to build the kvm package. The reason was qemu/configure does a big/little-endian test and the source-code it compiles causes the stubs-64.h file to be \#included (since the host architecture is 64-bit).

Another issue is missing gcc-multilib which, if the redirects of STDERR to /dev/null are removed, will reveal:

    /usr/bin/ld: skipping incompatible /usr/lib/gcc/i486-linux-gnu/4.2.3/libgcc.a when searching for -lgcc
    /usr/bin/ld: skipping incompatible /usr/lib/gcc/i486-linux-gnu/4.2.3/libgcc.a when searching for -lgcc

    /usr/bin/ld: cannot find -lgcc
    collect2: ld returned 1 exit status
    big/little test failed

These problems are solved by causing pbuilder / pdebuild to run with a 32-bit kernel personality. Use *linux32* infront of the command on the 64-bit host.

Found [here](http://tjworld.net/wiki/Linux/Ubuntu/Packages/CreatingPbuilderVariations).

Several orig.tar Files Found
----------------------------

Is this error familiar to you?

    $ dpkg-buildpackage -us -uc -d
    ...
    ...
    make[1]: Leaving directory `/tmp/source/lammps-0~20120615.gite442279'
       dh_clean -O--parallel
     dpkg-source -b lammps-0~20120615.gite442279
    dpkg-source: info: using source format `3.0 (quilt)'
    dpkg-source: error: several orig.tar files found (./lammps_0~20120615.gite442279.orig.tar.gz and ./lammps_0~20120615.gite442279.orig.tar.xz) but only one is allowed
    dpkg-buildpackage: error: dpkg-source -b lammps-0~20120615.gite442279 gave error exit status 255

If you ever get the error that says several orig.tar files found, simply move or delete the original source file you have downloaded:

    $ ls ..
    lammps-0~20120615.gite442279
    lammps_0~20120615.gite442279-1_amd64.build
    lammps_0~20120615.gite442279-1_amd64.changes
    lammps_0~20120615.gite442279-1_amd64.deb
    lammps_0~20120615.gite442279-1.debian.tar.gz
    lammps_0~20120615.gite442279-1.dsc
    lammps_0~20120615.gite442279-1_source.changes
    lammps_0~20120615.gite442279.orig.tar.gz
    lammps_0~20120615.gite442279.orig.tar.xz
    lammps-doc_0~20120615.gite442279-1_all.deb

    $ rm ../lammps_0~20120615.gite442279.orig.tar.xz

Package Signing Errors
----------------------

All official Debian packages need to be signed. If you get an error like this:

    Would you like to use the current signature? [Yn]Y
     signfile lammps_0~20120615.gite442279-1_amd64.changes Lee Hanxue
    gpg: skipped "Lee Hanxue ": secret key not available
    gpg: /tmp/debsign.7YPJJ27W/lammps_0~20120615.gite442279-1_amd64.changes: clearsign failed: secret key not available
    debsign: gpg error occurred!  Aborting....
    debuild: fatal error at line 1271:
    running debsign failed

It means debsign cannot find the correct key to be used. Simply use the **-k** flag to specify the build key:

$ debuild -uc -us --source-option=--include-binaries --source-option=-isession -idoc/ <span style="color:red; font-weight:bold">-k4AC12E8F</span>

Found [here](http://flummox-engineering.blogspot.de/2012/12/common-debian-pbuilder-errors.html).

Other Listings
--------------

On [qa.debian](https://wiki.debian.org/qa.debian.org/FTBFS) there's a detailed list about build failures that affect several packages (and the "good" way to solve them).

If anybody find more please [mail me](mailto:t.funk@web.de) to add them here ^^

References
==========

Commands
--------

A collection of most important or used commands.

**changelog entry**

Source package is new:

    $ dch --vendor=Tanglu --distribution staging -R "No-change rebuild for Tanglu."

or

    $ dch --vendor=Tanglu --distribution staging -R "No-change rebuild against <whatever>."

or

    $ dch --local b --vendor=Tanglu --distribution staging

A modified source package for Tanglu staging

    $ dch --local tanglu --vendor=Tanglu --distribution staging

**Create a Package**

Tying a new package (executed within the source folder)

    $ dpkg-buildpackage -S -sa -rfakeroot -kABCDFE01

Tying a source only Package (executed within the source folder)

    $ debuild -S -sa -kABCDFE01

**Prepare a Package**

Download sources

    $ dget <Path-to-dsc-File>

Unpack sources

    $ dpkg-source -x <package>.dsc

**Building a Package**

From unchanged sources

    $ sudo DIST=aequorea pbuilder build <package>.dsc

From changed sources (executed within the source folder)

    $ sudo DIST=staging ARCH=i386 pdebuild

Signing a new built Package manually

    $ debsign -kA264FDFA /var/cache/pbuilder/result/*.dsc
    $ debsign -kA264FDFA /var/cache/pbuilder/result/*.changes

**Work with the chroot**

Login and save changes

    $ linux32 sudo DIST=staging ARCH=i386 pbuilder --login --save-after-login

or

    $ sudo DIST=staging ARCH=amd64 pbuilder --login --save-after-login

Transfer changes if .pbuilderrc has changed

    $ sudo DIST=staging pbuilder update --override-config

or with the [pbuilder_utils](/Setup_pBuilder_Develop_Environment#Helper_Functions "wikilink") function

    $ pbuilder_override_cfg

Update chroot system

    $ sudo DIST=staging pbuilder update

or with the [pbuilder_utils](/Setup_pBuilder_Develop_Environment#Helper_Functions "wikilink") function

    $ pbuilder_update staging

cleaning aptchache

    $ linux32 sudo ARCH=i386 pbuilder --clean

or with the [pbuilder_utils](/Setup_pBuilder_Develop_Environment#Helper_Functions "wikilink") function

    $ pbuilder_clean all

**Others**

List files in binary (executed within the source folder)

    $ debc /var/cache/pbuilder/staging-amd64/result/doc-debian_6.1tanglu1_amd64.changes

Lintian
-------

For error messages while running lintian see [Lintian Reports](http://lintian.debian.org/index.html).

Multiarch
---------

If there're problems pointing to multiarch have a look into Debians [Multiarch](https://wiki.debian.org/Multiarch/Implementation) document.

Patching
--------

Sometimes patches have to be removed or added. For more information look in Debians [UsingQuilt](https://wiki.debian.org/UsingQuilt), Raphaël Hertzog's very good [How to use quilt to manage patches in Debian packages](http://raphaelhertzog.com/2012/08/08/how-to-use-quilt-to-manage-patches-in-debian-packages/) or [Working with patches](http://wiki.openwrt.org/doc/devel/patches) from the openwrt wiki.

Bugs and Improvements
=====================

If you find bugs or have improvements send me an [email](mailto:t.funk@web.de). I would be happy to hear from you ^^

-- Thomas --

Thanks
======

Many thanks to all people who published the information and tips on the web that I used in this Howto!