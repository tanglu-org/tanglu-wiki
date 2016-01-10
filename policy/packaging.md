---
title: Policy packaging
permalink: /Policy/packaging/
---

Tanglu Packaging Policy
=======================

The general rule is: All Debian policies apply to Tanglu the same way they do for Debian, see the Debian Policy Manual [1](https://www.debian.org/doc/debian-policy/) for the details. However, a few of these rules are relaxed in Tanglu, currently:

1.  9.6 [2](https://www.debian.org/doc/debian-policy/ch-opersys.html#s-menus): The Debian Menu System is not used in Tanglu, and menu entries does not have to be included for any applications.
2.  12.1 [3](https://www.debian.org/doc/debian-policy/ch-docs.html#s12.1): Man pages do not have to be included for GUI applications normally launched using an included .desktop file.
3.

    TODO

As we ship newer and updated packages in Tanglu, there are also a few additional rules which have to be followed:

1.  No increase of package epoch - This should never be done in Tanglu, as it creates incompatibilities between Tanglu and Debian which cannot be resolved anymore. Increasing an epoch in Tanglu will result in an instant reject of the package.
2.

    TODO

There are also some rules regarding the release version and changelog entries used in Tanglu packages:

1.  A package should use the same version number as in Debian if, and only if, it has been synced directly from Debian using synchrotron.
2.  When uploading a source package identical to a Debian package (excluding the changelog) to Tanglu *staging*, append "bX" to the Debian version number, where X is a small integer, starting at 1 and increasing for every upload.
    1.  If the package version is new to Tanglu the new changelog entry should state "No-change rebuild for Tanglu."
    2.  Otherwise it should state why the package is being rebuilt, typically using a sentence on the form "No-change rebuild against XXXX.".

3.  When uploading a modified source package to Tanglu *staging*, you should strip any suffix from the Debian release version, if present, and then append a "tangluX" suffix, where X is a small integer, starting at 1 and increasing for every upload.
    1.  A source package not based on a Debian package, or based on a Debian package for a different upstream version, should use a release version on the form "0tangluX", as if it had been based on a package with release version "0".
    2.  (A modified Tanglu package should thus always have a release version on the form "0tangluX", "NtangluX" or "N.NtangluX")

4.  When uploading a source package identical to a Debian package (excluding the changelog) to Tanglu *(old)stable(-updates)*, you should strip any suffix from the Debian release version, if present, and then append a "~tangluZ" suffix, where Z is the Tanglu release version.
    1.  If this would result in a *lower* version number than the package already in Tanglu Z, use the same suffix as when uploading a modified source package to Tanglu *(old)stable(-updates)* (see below).

5.  When uploading a modified source package to Tanglu *stable(-updates)*, you should strip any suffix from the Debian release version, if present, and then append a "tangluX.Y" suffix, where X is the Tanglu revision already in Tanglu *stable* (or 0 if there is none), and Y is a small integer, starting at 1 and increasing for every upload.
    1.  If this would result in a *lower* version number than the package already in Tanglu *stable*, use suffix "+tanglu0.Y", and make sure that staging has a package with a higher version number, possibly by uploading the same package to staging but with suffix "+tangluY" instead.

6.  When uploading a modified source package to Tanglu *oldstable(-updates)*, you should use the same version number as for Tanglu *stable(-updates)*, but with a "~tangluZ" suffix appended, where Z is the Tanglu release version.

<!-- -->

1.  If you merge changes from Ubuntu, remove the "XubuntuY" revision from the package's version string and replace it with "XtangluY", because we diff against Debian, not Ubuntu. But please always add a note "Merged from Ubuntu VERSION" to the changelog, and keep the previous Ubuntu changelog intact. Remember that Tanglu is not Ubuntu and rebuild the package before uploading it, to ensure that it builds on Tanglu.
2.

    TODO

