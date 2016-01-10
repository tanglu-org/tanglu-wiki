---
title: Fvwm-Nightshade Spin
permalink: /Fvwm-Nightshade_Spin/
---

Since Tanglu is on the road I've ogled whith the idea to create a Live-CD with [Fvwm-Nightshade](https://github.com/Fvwm-Nightshade/Fvwm-Nightshade/tree/develop). Now d-i works lastly and I can start with a minimal base system to setup a small Live system.

But first some thoughts have to be made ...

Needed Components
=================

Fvwm-Nightshade (FNS)
---------------------

Develop version - here's a screenshot of it:

[400px|Develop version of FNS (PitchBlack)](/File:Fns_theme_rocken.jpg "wikilink")

Need still some work, but perhaps I find people who want to help me ...

**TODO:**

-   automounting daemon
-   implementing session management

**In Progress:**

-   debianize it. There's deb package support already available, but needs it outside =&gt; orig.tar.gz and debian.tar.gz

FNS-Desktop
-----------

Meta package. I've created a table with apps and tools I thought they're needed and useful:

### Systemtools

[Xfe](http://roland65.free.fr/xfe/) - File Commander/Explorer interface, text editor, text viewer, image viewer

PcManFM - File manager with mount support

[stalonetray](http://stalonetray.sourceforge.net/) - Systray

[network-manager](http://projects.gnome.org/NetworkManager/) - Network interface (GNOME Systray applet)

[VolumeIcon](http://softwarebakery.com/maato/volumeicon.html) - Systray volume applet

xterm, [1](http://wiki.lxde.org/en/LXTerminalLXTerminal) - Terminals

Xarchiver - Archiving tool

\[LightDM\] - Lightweight login manager

[Grun](https://github.com/lrgc/grun) - Application launcher

[clipit](http://sourceforge.net/projects/gtkclipit/) - Clipboard manager

[FileZilla](http://filezilla-project.org/) - FTP client/server

[Gufw](http://gufw.org/) - Firewall

fdpowermon - powermonitor for Systray

Synaptic - Package manager

[Compton](https://github.com/chjj/compton) - Compositor for X

[Conky](https://github.com/brndnmtthws/conky) - System monitor for X

### Configuration

[system-config-printer](http://cyberelk.net/tim/software/system-config-printer/) - Print manager

[LXAppearance](http://lxde.org/lxappearance_change_look_feel/) - Gtk2/3 Theme manager

LXInput - Mouse and keyboard manager

[gnome-system-tools](https://projects.gnome.org/gst/index.html) - users-admin

bluemon - Bluetooth manager

### Applications

[Midori](http://midori-browser.org/) - Web browser

[Balsa](https://pawsa.fedorapeople.org/balsa/) - Email client

[Pidgin](https://pidgin.im/) - Instant messaging client

Xfi - Image viewers

Gimp - Image Manipulation

[gmusicbrowser](https://gmusicbrowser.org/) - Music player

mplayer2 - Video player

[Geany](http://www.geany.org/) - IDE

LibreOffice - Office suite

Xfw - Text editor

Xpdf - PDF viewer

xCHM - CHM viewer

[galculator](http://galculator.mnim.org/) - calculator

### Other Stuff

Iconpack from

-   [Faenza](http://tiheum.deviantart.com/art/Faenza-Icons-173323228)
-   ([RAVEfinity](http://www.ravefinity.com/p/ravefinity-x-icon-theme.html))

Startup
-------

### Login

Using lightdm as Slim is dead.

**Information to configure lightdm:**

[freedesktop.org Wiki](http://www.freedesktop.org/wiki/Software/LightDM/)

[Arch Linux](https://wiki.archlinux.org/index.php/LightDM)

[How to change the LightDM theme/greeter?](http://askubuntu.com/questions/75755/how-to-change-the-lightdm-theme-greeter)

[Customize LightDM with Themes and Backgrounds](http://www.maketecheasier.com/customize-lightdm-themes/)

[Different config options](http://www.faqforge.com/tag/lightdm/)

[LightDM and Xscreensaver](http://roundsl.blogspot.de/2013/03/lightdm-and-xscreensaver.html)

**GUI Config tool**

[Simple LightDM Manager](http://www.ubuntugeek.com/simple-lightdm-manager.html)

**TODO**

-   Creating a Tanglu theme/greeter.
-   Creating a Config GUI with SimpleGtk2 if needed
