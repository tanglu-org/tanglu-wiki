---
title: Iceweasel
permalink: /Iceweasel/
---

**Instalando a última versão do Iceweasel no Tanglu**

Para termos sempre a última versão do Iceweasel, iremos utilizar o repositório Debian Mozilla Team. Adicione no seu arquivo /etc/apt/sources.list as seguintes entradas:

1.  Debian Mozilla Team

<!-- -->

1.  1.  Chave GPG: apt-get update; apt-get install pkg-mozilla-archive-keyring -y --force-yes; apt-get update

deb <http://mozilla.debian.net/> wheezy-backports iceweasel-release

Em seguida faça o procedimento a seguir em seu Terminal:

$ sudo apt-get update $ sudo apt-get install pkg-mozilla-archive-keyring -y --force-yes $ sudo apt-get update

Para instalar a versão em Português, prossiga com o comando:

$ sudo apt-get -t wheezy-backports install iceweasel iceweasel-l10n-pt-br SE FOR Português de Portugal, basta trocar o pt-br POR pt-pt

Simples não? a última versão do navegador Iceweasel no Tanglu !