---
title: Setup pBuilder Develop Environment
permalink: /Setup_pBuilder_Develop_Environment/
---

Preface
=======

To help Tanglu building working packages it is necessary to setup chroot environments. A clean chroot environment makes it possible to check what dependencies are really required or missing. With [pbuilder](http://www.netfort.gr.jp/~dancer/software/pbuilder-doc/pbuilder-doc.html) you can do such things. But there are some preliminary work to do. This howto tries to setup a pbuilder configuration which you can use to build packages for various architectures and distribution version on your Tanglu/Debian machine.

Creating a GPG key
==================

First of all you need a GPG key to sign the created/modified packages. To do this follow the instructions in [Debian's Keysigning](https://wiki.debian.org/Keysigning) tutorial.

|                                                   |                                                                                                                                                                                                                                                                                                |
|---------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [left|Important!](/File:Important.png "wikilink") | <span style="color:blue">As Tanglu is a Debian derivate the key should be at least 2048 bits long (better 4096 bits) and created with SHA2 hashing algorithm. So please follow in <strong>Step 1: Create a RSA keypair</strong> the creation under <strong>creating a keypair</strong>.</span> |

For more information about GPG see also [GnuPG Arch Wiki](https://wiki.archlinux.org/index.php/GnuPG) and [Ubuntu's GnuPrivacyGuardHowto](https://help.ubuntu.com/community/GnuPrivacyGuardHowto).

Setup Environment
=================

Tanglu supports two architecture flavours: **i386** and **amd64**. From that point of view **two** pbuilder environments should be created assuming you have an amd64 system. If not only an i386 environment can be created.

For each flavour 500 - 1000 MB of free space are needed.

Installing pbuilder
-------------------

On Tanglu, everything you need is already preconfigured. You just need to install pbuilder:

    $ sudo apt-get install pbuilder debootstrap devscripts util-linux

If you are not on a Tanglu distribution, but on Debian or a derivative (e.g. Ubuntu), do:

1.  Download & install the newest [debootstrap package](http://archive.tanglu.org/tanglu/pool/main/d/debootstrap/) from Tanglu
2.  Download & install the Tanglu [archive keyring package](http://archive.tanglu.org/tanglu/pool/main/t/tanglu-archive-keyring/).
3.  Install pbuilder:

<!-- -->

    $ sudo apt-get install pbuilder devscripts util-linux

|                                        |                                                                                                                                                                                                                                      |
|----------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [left|Note](/File:Note.png "wikilink") | <span style="color:blue"><strong>devscripts</strong> is not necessary to install along with pbuilder, however if you are serious about using pbuilder and creating and maintaining packages for Tanglu you should install it.</span> |

|                                        |                                                                                                                                                                                                                                                                                                                                                                                             |
|----------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [left|Note](/File:Note.png "wikilink") | <span style="color:blue">Some packages have broken build systems, which check for kernel architecture instead of the compiler's host architecture. For that we install <strong>util-linux</strong>. With the command <strong>linux32</strong> infront of the i386 build command will be used to fool the build system to think it is running on an i386 rather than an amd64 kernel.</span> |

Edit /etc/sudoers
-----------------

We need some environment variables used as root but set as user. Also **pdebuild** uses to rebuild source packages **sudo -E** to call internally **"pbuilder build"** as root.

So add with **visudo** the red highlighted lines:

\#
\# This file MUST be edited with the 'visudo' command as root.
\#
\# Please consider adding local content in /etc/sudoers.d/ instead of
\# directly modifying this file.
\#
\# See the man page for details on how to write a sudoers file.
\#
<span style="color:red; font-weight:bold">Defaults env_reset,env_keep="DIST ARCH CONCURRENCY_LEVEL"</span>
Defaults mail_badpass
Defaults secure_path="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"
\# Host alias specification
\# User alias specification
\# Cmnd alias specification
<span style="color:red; font-weight:bold">Cmnd_Alias PBUILDER = /usr/sbin/pbuilder, /usr/bin/pdebuild, /usr/bin/debuild-pbuilder</span>
\# User privilege specification
root ALL=(ALL:ALL) ALL
<span style="color:red; font-weight:bold">\[your_user\] ALL=(ALL) PBUILDER</span>
\# Allow members of group sudo to execute any command
%sudo ALL=(ALL:ALL) ALL
\# See sudoers(5) for more information on "\#include" directives:
\#includedir /etc/sudoers.d

|                                        |                                                                                                  |
|----------------------------------------|--------------------------------------------------------------------------------------------------|
| [left|Note](/File:Note.png "wikilink") | <span style="color:blue">To deactivate password retrieval change the line with \[your_user\] to

                                              [your_user] ALL=(ALL) SETENV: NOPASSWD: PBUILDER

                                          </span>                                                                                           |

|                                        |                                                                                                                                                                                                        |
|----------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [left|Note](/File:Note.png "wikilink") | <span style="color:blue">The "Defaults" line contains instructions for keeping the values of variables DIST, ARCH and CONCURRENCY_LEVEL. Keep those variables in mind, we will use them later.</span> |

Configure pbuilder
------------------

pbuilder has a default config /etc/pbuilderrc but we will use our own .pbuilderrc because of more flexibility.

You'll find the used .pbuilderrc for this Howto [here](/pbuilderrc "wikilink"). But anyway I am listing it below and try to explain it as good as possible ...

Ok, let's start ...

Open as root an editor of your choice, copy the content into the new document, save it as /root/.pbuilderrc and follow me through the file.

### DIST - Use of Codenames

    # Codenames for Tanglu suites according to their alias. Update these when
    # needed.
    UNSTABLE_CODENAME="staging"
    TESTING_CODENAME="staging"
    EXPERIMENTAL_CODENAME="staging"
    STABLE_CODENAME="bartholomea"

    # List of Tanglu suites.
    TANGLU_SUITES=($EXPERIMENTAL_CODENAME $UNSTABLE_CODENAME $TESTING_CODENAME $STABLE_CODENAME
        "experimental" "unstable" "testing" "stable")

**DIST** enables the possibility to use a pbuilder environment based on the respective Tanglu version suposed you have it created.

Examples (both are for the same distribution):

    $ sudo DIST=bartholomea pbuilder build example.dsc

    $ sudo DIST=stable pbuilder build example.dsc

### Components, Mirror and Changelog

    # specifying the components of the distribution, for instance to enable all
    # components on Tanglu use "main contrib non-free"
    COMPONENTS="main contrib non-free"

    # Mirrors to use. Update these to your preferred mirror.
    TANGLU_MIRROR="archive.tanglu.org"

    # Specify the mirror site which contains the main distribution.
    MIRRORSITE=http://$TANGLU_MIRROR/tanglu

    # Optionally use the changelog of a package to determine the suite to use if
    # none set.
    if [ -z "${DIST}" ] && [ -r "debian/changelog" ]; then
        DIST=$(dpkg-parsechangelog | awk '/^Distribution: / {print $2}')
        # Use the staging suite for Debian testing/unstable/experimental packages.
        if [ "${DIST}" == "testing" ]; then
            DIST="staging"
        elif [ "${DIST}" == "unstable" ]; then
            DIST="staging"
        elif [ "${DIST}" == "experimental" ]; then
            DIST="staging"
        elif [ "${DIST}" == "staging" ]; then
            DIST="staging"
        else
            DIST="bartholomea"
        fi
    fi

    # Set the default distribution if none is used. Note that you can set
    # your own default (i.e. ${DIST:="staging"}).
    : ${DIST:="aequorea"}
    #: ${DIST:="$(lsb_release --short --codename)"}

    # Optionally change Debian codenames in $DIST to their Tanglu aliases.
    case "$DIST" in
        experimental)
            DIST=$EXPERIMENTAL_CODENAME
            ;;
        unstable)
            DIST=$UNSTABLE_CODENAME
            ;;
        testing)
            DIST=$TESTING_CODENAME
            ;;
        staging)
            DIST=$TESTING_CODENAME
            ;;
        stable)
            DIST=$STABLE_CODENAME
            ;;
    esac

These parts should be self-explanatory - hopefully.

### ARCH - Use of different Architectures

    # Optionally set the architecture to the host architecture if none is set.
    # Note that you can set your own default (i.e. ${ARCH:="i386"}).
    : ${ARCH:="$(dpkg --print-architecture)"}

    NAME="$DIST"
    if [ -n "${ARCH}" ]; then
        NAME="$NAME-$ARCH"
        # specifying the architecture passes --arch= to debootstrap; the default is
        # to use the architecture of the host
        ARCHITECTURE=${ARCH}
    fi

With the parameter **ARCH** you can select which architectural pbuilder environment will be used.

Building a package for the i386 staging distribution:

    $ linux32 sudo DIST=staging ARCH=i386 pbuilder build example.dsc

Building an amd64 package for the distribution depending on the Changelog:

    $ sudo ARCH=amd64 pbuilder build example.dsc

### CCL - Use of Concurency Level

    # Optionally set the CONCURRENCY_LEVEL to specifies the number of allowed
    # concurrent jobs for make while building the package.
    # Default is cores-1.
    : ${CCL:="$(grep -cE "^processor" /proc/cpuinfo)"}
    if [ "${CCL}" -eq "1" ]; then
        CONCURRENCY_LEVEL=${CCL}
    else
        CONCURRENCY_LEVEL=$((${CCL}-1))
    fi

When compiling a package, one can adjust the so-called concurrency level, which basically is the number of threads make is going to compile. A general rule often read when it comes to compiling e.g. a custom kernel, is that the concurrency level should be the number of cores on the machine - 1. So with the **CCL** parameter you can play around this level whether you think the build slows down. For more information see [here](https://nrdblg.de/nrd/benchmarks/kernel_make_concurrency_levels/kernel_make_concurrency_levels_en.html) and [here](http://ubuntuforums.org/archive/index.php/t-1014190.html).

Example:

    $ sudo CCL=1 ARCH=amd64 pbuilder build example.dsc

    # export the three variables
    export DIST
    export ARCH
    export CONCURRENCY_LEVEL

    #echo architecture: $ARCH
    #echo distribution: $DIST
    #echo used cpus:    $CONCURRENCY_LEVEL

Now the variables will be exported. If you want to see the values in the build log uncomment the 3 echos.

### Used Locations

    # Specifies the default location for the archived chroot image to be created and used.
    BASETGZ="/var/cache/pbuilder/$NAME-base.tgz"

    # specifying the distribution forces the distribution on "pbuilder update"
    DISTRIBUTION="$DIST"

Sets the needed base.tgz depending on the wanted architecture and distribution.

    # Specify the default directory which the build result will be copied over to after
    # the building. Specify a full-path, not a relative path.
    BUILDRESULT="/var/cache/pbuilder/$NAME/result/"

In this directory the built packages and source files will be placed.

For later signing the $NAME directory have to be owner changed:

    $ sudo chown -R $USER /var/cache/pbuilder/[$DIST-$ARCH]

|                                                   |                                                                        |
|---------------------------------------------------|------------------------------------------------------------------------|
| [left|Important!](/File:Important.png "wikilink") | <span style="color:blue">Do this for each pbuilder environment.</span> |

    # additional build results to copy out of the package build area
    #ADDITIONAL_BUILDRESULTS=(xunit.xml .coverage)

If you need additional build results uncomment this line.

    # Specify the location that the packages downloaded by apt should be cached.
    # Setting this value to "" will cause  caching  to  be turned off.
    APTCACHE="/var/cache/pbuilder/$NAME/aptcache/"

    # Specify using hard links in apt cache handling. Changing this
    # to "no" will disable hard linking and will copy the files.
    APTCACHEHARDLINK="yes"

    # default AUTOCLEANAPTCACHE
    # If it is set to "yes" always run pbuilder with --autocleanaptcache option.
    AUTOCLEANAPTCACHE=""

    # The default place which the chroot is constructed.
    BUILDPLACE="/var/cache/pbuilder/build/"

### Local Repository

    # use previously built packages as local respository
    BINDMOUNTS="${BUILDRESULT} ${CCACHE_DIR}"
    OTHERMIRROR="deb [trusted=yes] file:$BUILDRESULT ./"

    # create local repository if it doesn't already exist,
    # such as during initial 'pbuilder create'
    if [ ! -d $BUILDRESULT ] ; then
        mkdir -p $BUILDRESULT
        # set permissions so I can delete files
        chgrp sudo $BUILDRESULT
        chmod g+rwx $BUILDRESULT
    fi
    if [ ! -e $BUILDRESULT/Packages ] ; then
        touch $BUILDRESULT/Packages
    fi
    if [ ! -e $BUILDRESULT/Release ] ; then
        cat << EOF > $BUILDRESULT/Release
    Archive: $DIST
    Component: main
    Origin: pbuilder
    Label: pbuilder
    Architecture: $ARCH
    EOF

    fi

This part will create for you a local repository with previous built packages. This is useful if you have to build and upload both a library, then a package depending on it. See for more information [Pbuilder Tricks - Debian Wiki](https://wiki.debian.org/PbuilderTricks).

### Hook directory and Files

    # Specifies the default location for the user hooks.
    # Default is "/usr/lib/pbuilder/hooks"
    #HOOKDIR="/path/to/hook/dir"
    HOOKDIR="/var/cache/pbuilder/hooks.d"

This is a very important part. With hooks you can control the build process in every stage.

The convention of hook files are:

    X<digit><digit><whatever-else-you-want-as-name>

The **X** defines the hook class followed by 2 digits which define the order of execution within a class.

| Classes | Description                                                                                                                                                                             |
|---------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| A       | Is for --build target. It is executed before build starts; after unpacking the build system, and unpacking the source, and satisfying the build-dependency.                             |
| B       | s executed after build system finishes building, successfully, before copying back the build result.                                                                                    |
| C       | Is executed after build failure, before cleanup.                                                                                                                                        |
| D       | Is executed before unpacking the source inside the chroot, after setting up the chroot environment.

           Create $TMP, and $TMPDIR if necessary.

           This is called before build-dependency is satisfied. Also useful for calling apt-get update                                                                                              |
| E       | Is executed after pbuilder --update and pbuilder --create finishes apt-get work with the chroot, before umounting kernel file systems (/proc) and creating the tarball from the chroot. |
| F       | Is executed just before user logs in, or program starts executing, after chroot is created in --login or --execute target.                                                              |
| G       | Is executed just after debootstrap finishes, and configuration is loaded, and pbuilder starts mounting /proc and invoking apt-get install in --create target.                           |

First of all we should create the hook directory and change the ownership (for editing the files without root permissions):

    $ sudo mkdir /var/cache/pbuilder/hooks.d
    $ sudo chown -R $USER /var/cache/pbuilder/hooks.d

Then we copy some hook files into that directory and change some parts:

    $ cp /usr/share/doc/pbuilder/examples/B90lintian /var/cache/pbuilder/hooks.d/

This script will run **lintian** to check for packaging errors after building.

To avoid lintian to fail the build change the following in the script:

    #su -c "lintian -I --show-overrides /tmp/buildd/*.changes" - pbuilder
    # use this version if you don't want lintian to fail the build
    su -c "lintian -I --show-overrides /tmp/buildd/*.changes; :" - pbuilder

    $ cp /usr/share/doc/pbuilder/examples/C10shell /var/cache/pbuilder/hooks.d/

This script starts a **bash shell** session after build failure inside chroot. This is very useful to check for e.g. a config.log for configure errors.

Now we create own hook files to get some nice extra feature:

The following hook file **D70results** is adapted from [pbuilder documentation - 9. Using special apt sources lists, and local packages](http://www.netfort.gr.jp/~dancer/software/pbuilder-doc/pbuilder-doc.html#usingspecialaptsources). It use your built packages from inside the chroot as an apt-get package pool.

    #!/bin/sh
    echo Calling $0

    : ${DIST:=$(lsb_release --short --codename)}
    : ${ARCH:=$(dpkg --print-architecture)}

    echo Used distribution: $DIST
    echo Used architecture: $ARCH
    NAME="$DIST-$ARCH"
    BUILDRESULT="/var/cache/pbuilder/$NAME/result"

    cd $BUILDRESULT
    /usr/bin/dpkg-scanpackages . /dev/null > ${BUILDRESULT}/Packages
    /usr/bin/apt-get update

The next **D50sun-java-licences** ensures that any package that Build-Depends on, or whose Build-Depends packages in turn Depend on, the sun-java?-\*{bin,jre,jdk} will install successfully in a non-interactive buildd or pbuilder.

    #!/bin/bash
    # from http://tjworld.net/wiki/Linux/Ubuntu/Packages/CreatingPbuilderVariations
    debconf-set-selections <<EOF
    sun-java5-bin shared/accepted-sun-dlj-v1-1 boolean true
    sun-java5-jre shared/accepted-sun-dlj-v1-1 boolean true
    sun-java5-jdk shared/accepted-sun-dlj-v1-1 boolean true
    sun-java6-bin shared/accepted-sun-dlj-v1-1 boolean true
    sun-java6-jre shared/accepted-sun-dlj-v1-1 boolean true
    sun-java6-jdk shared/accepted-sun-dlj-v1-1 boolean true
    EOF

If we are logged in in the chroot it would be fine to know this. For this create **F90know-chroot**

    #!/bin/sh
    # this shell is executed before logging in to chroot via login/execute.

    # change the prompt
    if [ `grep -c '@pbuild' /root/.bashrc` -eq 0 ];
    then
            echo 'export PS1="$USER@pbuild$$:\w# "' >> /root/.bashrc
    fi

    # add alias ll
    if [ `grep -c 'laFh' /root/.bashrc` -eq 0 ];
    then
            echo "alias ll='ls -laFh'" >> /root/.bashrc
    fi

    # Add a chroot reminder
    # suggested by Turbo Fredriksson
    if [ ! -e /CHROOT ];
    then
            echo "This is a chroot made by pbuilder" > /CHROOT
    fi

To restore in staging /etc/apt/sources.list after a 'pbuilder update --override-config' (it removes the deb/deb-src lines of the development version) create **E50restore-sources.list**:

    # script to revert override-config action in Tanglu
    # => override-config removes the development entries
    #!/bin/bash

    if [ -e /etc/apt/sources.list_full ]
    then
        if [ -n "`/usr/bin/diff -q /etc/apt/sources.list /etc/apt/sources.list_full`" ]
        then
            echo "I: restore /etc/apt/sources.list"
            cp /etc/apt/sources.list_full /etc/apt/sources.list
            apt-get update
        fi
    fi

Last but not least make the scripts executable

    $ chmod 755 /var/cache/pbuilder/hooks.d/*

|                                        |                                                                                                                           |
|----------------------------------------|---------------------------------------------------------------------------------------------------------------------------|
| [left|Note](/File:Note.png "wikilink") | <span style="color:blue">You will find some others hook scripts under /usr/share/doc/pbuilder/examples/ if needed.</span> |

### Extra Packages and Signing

    # Specifies extra packages which the system should install in the
    # chroot on pbuilder create.
    EXTRAPACKAGES="apt-utils ccache vim"

|                                        |                                                                                                     |
|----------------------------------------|-----------------------------------------------------------------------------------------------------|
| [left|Note](/File:Note.png "wikilink") | <span style="color:blue"> <strong>apt-utils</strong> is needed for e.g. generating package indexes.
                                          <strong>ccache</strong> is a compiler cache to save time while building.
                                          </span>                                                                                              |

    # Specify the packages to be removed on creation of base.tgz
    REMOVEPACKAGES=""

One of the packages which should removed is lilo but it isn't in Tanglu. So we leave it blank ... ;-)

    # When this value is set to yes, pdebuild will invoke debsign command
    # after building.
    #AUTO_DEBSIGN=${AUTO_DEBSIGN:-yes}

Here you can define whether the packages shall be signed after building. I have commented it out because mostly errors occur while building. So it is better to sign manually when building was successful.

### Mountpoints

    # Specify yes when it is desired to mount /proc interface.
    USEPROC=yes
    # Specify  yes  when  it is desired to mount /dev/pts interface.
    USEDEVPTS=yes
    # Specify yes when it is desired to mount /run/shm mount point.
    USERUNSHM=yes
    # Whether to use DEVFS or not.
    USEDEVFS=no

It is usually a good idea to set USEPROC and USEDEVPTS to yes, since there are many software which fail miserably when there is no /proc and /dev/pts being mounted. The same is with /run/shm (USERUNSHM) in order to work with software that expect shm to work.

### Ccache

    # NB: this var is private to pbuilder; ccache uses "CCACHE_DIR" instead
    # CCACHEDIR="/var/cache/pbuilder/ccache"
    CCACHEDIR=""
    export CCACHE_DIR="/var/cache/pbuilder/ccache"
    export PATH="/usr/lib/ccache:${PATH}"

**Ccache** is a great time saver when you’re building packages. It’s a compiler cache. It improves build time of recompiles by using previously unchanged items in its cache.

To activate it do the following steps:

    sudo mkdir -p /var/cache/pbuilder/ccache
    sudo chmod a+w /var/cache/pbuilder/ccache

Now pbuilder will automatically cache compiler output between multiple builds of the same software.

In case of permission problems, change the offending folder's owner to the pbuilder user, e.g

    chown 1234:1234 /var/cache/pbuilder/ccache/e

See also [PbuilderHowto Ubuntu Wiki](https://wiki.ubuntu.com/PbuilderHowto#Integration_with_ccache).

### Debconf

    # make debconf not interact with user
    export DEBIAN_FRONTEND="noninteractive"

In most cases **"noninteractive"** is ok but sometimes a package needs a "Yes I agree with this license" confirmation on installation -- which just failed in the chroot. For that change it temporarily to **"readline"**.

### Debemail

    # If this was specified, dpkg-buildpackage command will be passed with
    # the necessary sponsorship option -mMaintainer Name <Mail@Address> on building.
    DEBEMAIL=""

It is better to set this global in your ~/.bashrc because other tools outside pbuilder use this ENV, too. E.g. **dch** (create Changelog entries) or **debsign**.

    export DEBEMAIL="Your Name <you@example.com>"

### Debuild

    #for pbuilder debuild (sudo -E keeps the environment as-is)
    BUILDSOURCEROOTCMD="fakeroot"
    PBUILDERROOTCMD="sudo -E"
    # use cowbuilder for pdebuild
    #PDEBUILD_PBUILDER="cowbuilder"

    #Command-line option passed on to dpkg-buildpackage.
    #DEBBUILDOPTS="-IXXX -iXXX"
    # create sources only
    #DEBBUILDOPTS="-S -sa"
    # create binary AND sources
    DEBBUILDOPTS="-sa"

If sources only requested use "-S -sa".

### Satisfy Build-Dependencies

    # Command to satisfy build-dependencies; the default is an internal shell
    # implementation which is relatively slow;
    #PBUILDERSATISFYDEPENDSCMD="/usr/lib/pbuilder/pbuilder-satisfydepends"

    # There are two alternate implementations, the "experimental" implementation,
    # "pbuilder-satisfydepends-experimental", which might be useful to pull
    # packages from experimental or from repositories with a low APT Pin Priority,
    # and the "aptitude" implementation, which will resolve build-dependencies and
    # build-conflicts with aptitude which helps dealing with complex cases but does
    # not support unsigned APT repositories
    #PBUILDERSATISFYDEPENDSCMD="/usr/lib/pbuilder/pbuilder-satisfydepends-aptitude"

    # The classic resolver based on apt
    PBUILDERSATISFYDEPENDSCMD="/usr/lib/pbuilder/pbuilder-satisfydepends-classic"

|                                                 |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
|-------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [left|Important!](/File:Warning.png "wikilink") | <span style="color:blue">Don't use the build-dependency resolver based on gdebi because this resolver uses gdebi outside the chroot to exec dpkg inside the chroot. If you don't have the libs to exec the i386 dpkg (especially if you don't even have libc6:i386) you will get File Not Found when trying to exec it. See also <ins>[here](http://askubuntu.com/questions/323557/resolving-dependencies-inside-a-pbuilder-environment-instead-of-on-the-host-syst)</ins></span> |

    # Arguments for $PBUILDERSATISFYDEPENDSCMD.
    # PBUILDERSATISFYDEPENDSOPT=()

    # You can optionally make pbuilder accept untrusted repositories by setting
    # this option to yes, but this may allow remote attackers to compromise the
    # system. Better set a valid key for the signed (local) repository with
    # $APTKEYRINGS (see below).
    ALLOWUNTRUSTED=no

    # Option to pass to apt-get always.
    export APTGETOPT=("--force-yes")
    # Option to pass to aptitude always.
    export APTITUDEOPT=()

    #APT configuration files directory
    APTCONFDIR=""

### Build-User

    # the username and ID used by pbuilder, inside chroot. Needs fakeroot, really
    BUILDUSERID=1234
    BUILDUSERNAME=pbuilder

Building of packages inside the chroot as a regular user rather than root. This can be important since some packages won't build correctly as root.

|                                             |                                                                                                                                                                                                                                                                                                          |
|---------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [left|Note](/File:Important.png "wikilink") | <span style="color:blue">This won't create the user for pbuilder base packages that already exist. For those base packages the user will need to be added manually **inside** chroot. See <ins>[Troubleshooting: Add User 'pbuilder' in chroot](/#Add_User_'pbuilder'_in_chroot "wikilink")</ins></span> |

### Debootstrap & Aptkeyrings

    # The name of debootstrap command, you might want "cdebootstrap".
    DEBOOTSTRAP="debootstrap"

    # Set the debootstrap variant to 'buildd' type.
    DEBOOTSTRAPOPTS=(
        '--variant=buildd'
        '--keyring' '/usr/share/keyrings/tanglu-archive-keyring.gpg'
        '--include' 'tanglu-archive-keyring'
        )
    # or unset it to make it not a buildd type.
    # unset DEBOOTSTRAPOPTS

    # Keyrings to use for package verification with apt, not used for debootstrap
    # (use DEBOOTSTRAPOPTS). By default the tanglu-archive-keyring package inside
    # the chroot is used.
    #APTKEYRINGS=("/usr/share/keyrings/debian-archive-keyring.gpg")
    APTKEYRINGS=()

### Miscellaneous

    # Set the PATH I am going to use inside pbuilder: default is "/usr/sbin:/usr/bin:/sbin:/bin"
    export PATH="/usr/sbin:/usr/bin:/sbin:/bin"

    # SHELL variable is used inside pbuilder by commands like 'su'; and they need sane values
    export SHELL=/bin/bash

    # default file extension for pkgname-logfile
    PKGNAME_LOGFILE_EXTENTION="_${ARCH}.build"

    # default PKGNAME_LOGFILE
    PKGNAME_LOGFILE=""

    #default COMPRESSPROG
    COMPRESSPROG="gzip"

    # info output for building
    echo "I: Used distribution is ${DIST}."
    echo "I: Build architecture is ${ARCH}."
    echo "I: Used threads for building: ${CONCURRENCY_LEVEL}."

Creating pbuilder environments
------------------------------

Now you can start creating the build environment(s). Type the following command(s) in a terminal.

For **i386** build environment:

    $ sudo DIST=bartholomea ARCH=i386 pbuilder create

For **amd64** build environment:

    $ sudo DIST=bartholomea ARCH=amd64 pbuilder create

|                                        |                                                                                                                                                                                                              |
|----------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [left|Note](/File:Note.png "wikilink") | <span style="color:blue">These settings are for the stable release. If you want to build packages for an upcoming release replace "bartholomea" with the development version (currently chromodoris).</span> |

Add staging suite
-----------------

If you want to upload packages to the Tanglu development version (chromodoris), you need to add the staging suite. To do so, follow these steps:

1. Make a copy of your chromodoris-\[i386|amd64\]-base.tgz and rename it to staging-\[i386|amd64\]-base.tgz

    $ cd /var/cache/pbuilder
    $ sudo cp chromodoris-[i386|amd64]-base.tgz staging-[i386|amd64]-base.tgz

2. Login into pbuilder:

    $ sudo ARCH=[i386|amd64] DIST=staging pbuilder --login --save-after-login

3. Edit /etc/apt/sources.list and add the following entries:

    deb http://archive.tanglu.org/tanglu staging main contrib non-free
    deb-src http://archive.tanglu.org/tanglu staging main contrib non-free

4. make a copy of the sources.list (needed if 'pbuilder update --override-config' is executed =&gt; removes the chromodoris lines):

    $ cp /etc/apt/sources.list /etc/apt/sources.list_full

5. Exit the pbuilder environment with CTRL+AD

6. Update your environment:

    $ sudo ARCH=[i386|amd64] DIST=staging pbuilder --update

Helper Functions
----------------

I've written some helper functions to make life easier with pbuilder because if several chroots exist it will be a pain to update all of them. The same if something was changed in .pbuilderrc.

Create **pbuilder_utils** on a place you want with the following content:

    # functions to make life easier with pbuilder
    # check if "all" is used as parameter
    function set_dists() {
        dists=$@
        if [ `echo $dists|grep -ic "all"` == 1 ]
        then
            dists="aequorea bartholomea chromodoris staging"
        fi
    }

    # check if the host system is i386 only
    function set_arches() {
        arches="i386"
        local current=`dpkg --print-architecture`
        if [ "$current" = "amd64" ]
        then
            arches="$arches $current"
        fi
    }

    # updates each declared distribution
    # if 'all' is given all available base.tgz will be updated
    function pbuilder_update() {
        set_dists $@
        set_arches
        for a in $arches; do
            for d in $dists; do
                echo "Update $d base.tgz for $a ..."
                DIST=$d ARCH=$a sudo pbuilder update
                echo
            done
        done
    }

    # cleans the aptcache of the declared distribution(s)
    # if 'all' is given all apt caches will be cleaned
    function pbuilder_clean() {
        set_dists $@
        set_arches
        for a in $arches; do
            for d in $dists; do
                echo "Cleaning aptcache of $a $d chroot ..."
                DIST=$d ARCH=$a sudo pbuilder --clean
                echo
            done
        done
    }

    # updates all base.tgz if pbuilderrc has been changed
    function pbuilder_override_cfg() {
        set_arches
        for d in `ls -1 /var/cache/pbuilder/*.tgz| xargs -n1 basename|cut -d"-" -f1| uniq`; do
            for a in $arches; do
                echo "override-config: update $d base.tgz for $a ..."
                DIST=$d ARCH=$a sudo pbuilder update --override-config
                echo
            done
        done
    }

Then add to the end of your .bashrc

    source [your_path]/pbuilder_utils

Now each time you open a terminal the following commands are available:

    pbuilder_update [distribution1 distribution2 ... | all]

=&gt; updates each declared distribution or all

    pbuilder_clean [distribution1 distribution2 ... | all]

=&gt; cleans the aptcache of the declared distribution(s) or all

    pbuilder_override_cfg

=&gt; updates all base.tgz if .pbuilderrc has been changed

Troubleshooting
===============

Add User 'pbuilder' in chroot
-----------------------------

If you have created a base.tgz before using the .pbuilderrc described in this howto you have to setup the user 'pbuilder' in your chroot environment belatedly.

1. Login into your chroot:

    $ sudo DIST=[distribution] ARCH=[architecture] pbuilder --login --save-after-login

2. issue the command:

    root@pbuildxyz:/ # useradd -m pbuilder

3. logout

Changing .pbuilderrc
--------------------

If you change anything in the .pbuilderrc you have to update **ALL** chroot environments:

    $ sudo DIST=[distribution] ARCH=[architecture] pbuilder update --override-config

|                                             |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
|---------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [left|Note](/File:Important.png "wikilink") | <span style="color:blue">For **staging** you have to backup your /etc/apt/sources.list as described in <ins>[Add staging suite](/#Add_staging_suite "wikilink")</ins> because --override-config will remove the development entries! If you have add the hook script **E50restore-sources.list** described under <ins>[Hook directory and Files](/#Hook_directory_and_Files "wikilink")</ins>, you don't need to login with --save-after-login and add them again anymore. Also the script makes the needed "apt-get update" inside the chroot for you.</span> |

Speed up Package Installation
-----------------------------

To speed up the package installation inside chroot you have two options: tmpfs and a dpkg setting.

**dpkg setting**

To change the dpkg setting login into your pbuilder

    $ sudo DIST=[distribution] ARCH=[architecture] pbuilder --login --save-after-login

and run th following:

    # echo "force-unsafe-io" > /etc/dpkg/dpkg.cfg.d/02apt-speedup

This forces dpkg not to call sync() after package extraction and leads to 1-2 packages installed per second.

For more see [PbuilderHowto - Ubuntu Wiki](https://wiki.ubuntu.com/PbuilderHowto#Speeding_up_the_package_installation).

References
==========

[PbuilderHowto - Ubuntu Wiki](https://wiki.ubuntu.com/PbuilderHowto)

[The ultimate Debian/Ubuntu package building system](http://www.davromaniak.eu/index.php?post/2011/03/03/The-ultimate-package-building-system)

[PbuilderTricks - Debian Wiki](https://wiki.debian.org/PbuilderTricks)

[pbuilder User's Manual](http://www.netfort.gr.jp/~dancer/software/pbuilder-doc/pbuilder-doc.html)

[pbuilder FAQ](http://pbuilder-docs.readthedocs.org/en/latest/faq.html)

[Manpage pbuilder](http://manpages.ubuntu.com/manpages/trusty/en/man8/pbuilder.8.html)

[Manpage pbuilderrc](http://manpages.ubuntu.com/manpages/trusty/man5/pbuilderrc.5.html)

Bugs and Improvements
=====================

If you find bugs or have improvements send me an [email](mailto:t.funk@web.de). I would be happy to hear from you ^^

-- Thomas --

Thanks
======

Many thanks to all people who published the information, tips and scripts on the web that I used in this Howto!