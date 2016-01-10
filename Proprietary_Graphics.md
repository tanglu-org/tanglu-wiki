---
title: Proprietary Graphics
permalink: /Proprietary_Graphics/
---

Notes on installing proprietary graphics drivers on Tanglu Beta3.


Test installation platform: *Nvidia Geforce 7000M / Nforce 610m*

Reason for Proprietary graphics: Hardware Accelerated Compositing does not function properly on some open-source drivers.

Behavior on loading Tanglu Beta 3: Desktop icons appear correctly. KDE menu bar is graphically broken.


Immediate solution to continue with Tanglu installation and usage: Interrupt KDE compositing with **Alt - Shift - F12.**

With compositing suspended interaction continued as normal. Proceeded with typical steps of installing proprietary driver on Debian base by loading SMXI.

SMXI loaded by opening console and entering command:


`cd /usr/local/bin && wget -Nc smxi.org/smxi.zip && unzip smxi.zip`

Dropped to base terminal with command: **Alt - Control - F1** and logged in as root.

Attempt to run **sgfxi** command from *SMXI* package. Command fails as X cannot be killed.


Identify problem as issue with KDM through inferences of similar behavior with LightDM where terminating X-server generates immediate auto-restart of X server.

<!-- -->


The quick work around is to run the following command

<!-- -->


`/etc/init.d/kdm stop`

With KDM killed it should now be possible for the *sgfxi* script to kill the X-server and allow the creation of an X Org Configuration file.

On reboot the nvidia-glx legacy driver installed fine and is operational.