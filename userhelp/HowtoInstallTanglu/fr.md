---
title: HowtoInstallTanglu fr
permalink: /HowtoInstallTanglu/fr/
---

Comment installer Tanglu
========================

Depuis une clé USB ou CD/DVD
----------------------------

Suivez simplement les instructions jusqu'à la fin de l'installation.

Depuis Debian (stable)
----------------------

Si vous avez déjà Debian (version stable) d'installé, vous pouvez suivre ces instructions pour installer Tabglu. Note: Il est déconseillé d'installer Tabglu depuis Sid (version instable de Debian) étant donné que Tabglu est basé essentiellement sur Debian testing avec quelques paquets d'Unstable et d'experimental.

1. Éditez votre fichier /etc/apt/sources.list en tant que root et mettez ce-ci:

`deb `[`http://archive.tanglu.org/tanglu`](http://archive.tanglu.org/tanglu)` bartholomea main contrib non-free`
`deb-src `[`http://archive.tanglu.org/tanglu`](http://archive.tanglu.org/tanglu)` bartholomea-updates main contrib non-free`

2. Rechargez la liste des paquets, appliquez les mises à jour puis installer Tanglu-minimal

Pour cela entrez dans un terminal en tant que root:

`apt-get update`
`apt-get dist-upgrade`
`apt-get install tanglu-minimal`

3. Installez un environement de bureau (facultatif)

Vous pouvez installer les meta-paquets pour avoir un bureau Gnome ou Kde complet: Pour Gnome:

`apt-get install tanglu-gnome`

Pour Kde

`apt-get install tanglu-kde`