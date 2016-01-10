---
title: Pbuilderrc
permalink: /Pbuilderrc/
---

pbuilderrc file
===============

    #-----------------------------------------------------------------------
    # File:         .pbuilderrc
    # Version:      1.1.3
    # Licence:      GPL 2
    #
    # Description:  pbuilder settings for Tanglu distribution
    #               costumized from default pbuilderrc
    #               For more infos see 'man 5 pbuilderrc'
    #
    # Author:       Thomas Funk <t.funk@web.de>
    #
    # Created:      2014/01/31
    # Changed:      2014/06/09
    #-----------------------------------------------------------------------


    # Codenames for Tanglu suites according to their alias. Update these when
    # needed.
    UNSTABLE_CODENAME="staging"
    TESTING_CODENAME="staging"
    EXPERIMENTAL_CODENAME="staging"
    STABLE_CODENAME="aequorea"

    # List of Tanglu suites.
    TANGLU_SUITES=($EXPERIMENTAL_CODENAME $UNSTABLE_CODENAME $TESTING_CODENAME $STABLE_CODENAME
        "experimental" "unstable" "testing" "stable")

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
            DIST="aequorea"
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

    # Optionally set the CONCURRENCY_LEVEL to specifies the number of allowed
    # concurrent jobs for make while building the package.
    # Default is cores-1.
    : ${CCL:="$(grep -cE "^processor" /proc/cpuinfo)"}
    if [ "${CCL}" -eq "1" ]; then
        CONCURRENCY_LEVEL=${CCL}
    else
        CONCURRENCY_LEVEL=$((${CCL}-1))
    fi

    # export the three variables
    export DIST
    export ARCH
    export CONCURRENCY_LEVEL

    #echo architecture: $ARCH
    #echo distribution: $DIST
    #echo used cpus:    $CONCURRENCY_LEVEL

    # Specifies the default location for the archived chroot image to be created and used.
    BASETGZ="/var/cache/pbuilder/$NAME-base.tgz"

    # specifying the distribution forces the distribution on "pbuilder update"
    DISTRIBUTION="$DIST"

    # Specify the default directory which the build result will be copied over to after
    # the building. Specify a full-path, not a relative path.
    BUILDRESULT="/var/cache/pbuilder/$NAME/result/"

    # additional build results to copy out of the package build area
    #ADDITIONAL_BUILDRESULTS=(xunit.xml .coverage)

    # Specify the location that the packages downloaded by apt should be cached.
    APTCACHE="/var/cache/pbuilder/$NAME/aptcache/"

    # Specify using hard links in apt cache handling. Changing this
    # to "no" will disable hard linking and will copy the files.
    APTCACHEHARDLINK="yes"

    # default AUTOCLEANAPTCACHE
    # If it is set to "yes" always run pbuilder with --autocleanaptcache option.
    AUTOCLEANAPTCACHE=""

    # The default place which the chroot is constructed.
    BUILDPLACE="/var/cache/pbuilder/build/"

    # Use local packages in the build. This is useful if you have to build
    # and upload both a library, then a package depending on it
    # See for more information: https://wiki.debian.org/PbuilderTricks

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

    # Specifies the default location for the user hooks.
    # Default is "/usr/lib/pbuilder/hooks"
    #HOOKDIR="/path/to/hook/dir"
    HOOKDIR="/var/cache/pbuilder/hooks.d"

    # Specifies extra packages which the system should install in the
    # chroot on pbuilder create.
    EXTRAPACKAGES="apt-utils ccache"

    # Specify the packages to be removed on creation of base.tgz
    REMOVEPACKAGES=""

    # When this value is set to yes, pdebuild will invoke debsign command
    # after building.
    #AUTO_DEBSIGN=${AUTO_DEBSIGN:-yes}

    # Specify yes when it is desired to mount /proc interface.
    USEPROC=yes
    # Specify  yes  when  it is desired to mount /dev/pts interface.
    USEDEVPTS=yes
    # Specify yes when it is desired to mount /run/shm mount point.
    USERUNSHM=yes
    # Whether to use DEVFS or not.
    USEDEVFS=no

    # NB: this var is private to pbuilder; ccache uses "CCACHE_DIR" instead
    # CCACHEDIR="/var/cache/pbuilder/ccache"
    CCACHEDIR=""
    export CCACHE_DIR="/var/cache/pbuilder/ccache"
    export PATH="/usr/lib/ccache:${PATH}"

    # make debconf not interact with user
    export DEBIAN_FRONTEND="noninteractive"

    # If this was specified, dpkg-buildpackage command will be passed with
    # the necessary sponsorship option -mMaintainer Name <Mail@Address> on building.
    DEBEMAIL=""

    #for pbuilder debuild
    BUILDSOURCEROOTCMD="fakeroot"
    PBUILDERROOTCMD="sudo -E"
    # use cowbuilder for pdebuild
    #PDEBUILD_PBUILDER="cowbuilder"

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

    # Command-line option passed on to dpkg-buildpackage.
    #DEBBUILDOPTS="-IXXX -iXXX"
    # create sources only
    #DEBBUILDOPTS="-S -sa"
    # create binary AND sources
    DEBBUILDOPTS="-sa"

    #APT configuration files directory
    APTCONFDIR=""

    # the username and ID used by pbuilder, inside chroot. Needs fakeroot, really
    BUILDUSERID=1234
    BUILDUSERNAME=pbuilder

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

    # Set the PATH I am going to use inside pbuilder: default is "/usr/sbin:/usr/bin:/sbin:/bin"
    export PATH="/usr/sbin:/usr/bin:/sbin:/bin"

    # SHELL variable is used inside pbuilder by commands like 'su'; and they need sane values
    export SHELL=/bin/bash

    # The name of debootstrap command, you might want "cdebootstrap".
    DEBOOTSTRAP="debootstrap"

    # default file extension for pkgname-logfile
    PKGNAME_LOGFILE_EXTENTION="_${ARCH}.build"

    # default PKGNAME_LOGFILE
    PKGNAME_LOGFILE=""

    #default COMPRESSPROG
    COMPRESSPROG="gzip"

    # info output
    echo "I: Used distribution is ${DIST}."
    echo "I: Build architecture is ${ARCH}."
    echo "I: Used cpus for building: ${CONCURRENCY_LEVEL}."