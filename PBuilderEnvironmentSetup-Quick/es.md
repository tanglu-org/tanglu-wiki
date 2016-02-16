---
title: Configurar un entorno de trabajo básico para pBuilder
permalink: /SetUpPBuilderEnvironment/es
---

# Configurar un entorno de trabajo básico para pBuilder

Esta página explica cómo configurar un entorno de trabajo básico para _pBuilder_.

## Requisitos previos

En _Tanglu_ encontrará todo lo necesario para esta tarea, únicamente tiene que instalar _pbuilder_:

``` bash
sudo apt-get install pbuilder debootstrap devscripts
```

Si actualmente no dispone de un equipo con Tanglu pero dispone de uno que cuente con alguna versión de _Debian_ o alguna distribución derivada (_Ubuntu_ por ejemplo), entonces necesita hacer lo siguiente:

  1. Descargar e instalar el [paquete debootstrap](http://archive.tanglu.org/tanglu/pool/main/d/debootstrap/) desde los repositorios oficiales de Tanglu.
  2. Descargar e instalar el paquete [archive keyring](http://archive.tanglu.org/tanglu/pool/main/t/tanglu-archive-keyring/) desde los repositorios oficiales de Tanglu.
  3. Instalar _pbuilder_:


``` bash
sudo apt-get install pbuilder devscripts
```

## Crear el entorno de trabajo para pBuilder

Acceda a una terminal y realice lo siguiente según sea el caso.

Desde Tanglu:

``` bash
sudo pbuilder create --distribution aequorea --debootstrapopts --variant=buildd
```

En Debian y derivadas:

``` bash
sudo pbuilder create --distribution aequorea --debootstrapopts --variant=buildd --mirror http://archive.tanglu.org/tanglu --debootstrapopts "--keyring=/usr/share/keyrings/tanglu-archive-keyring.gpg"
```

**Recuerde siempre reemplazar el nombre de la versión, en este caso _"aequorea"_, con el nombre de la versión de Tanglu para la que desee empaquetar las aplicaciones.**

¡Eso es todo! Ahora debería de tener listo un entorno de trabajo que le permita empaquetar aplicaciones para Tanglu.

## Development: Add staging suite

If you want to upload packages to the Tanglu development version, you need to add the staging suite.

Si desea a la versión de desarrollo de Tanglu, necesita agregar el staging suite. Para hacerlo siga los siguientes pasos:

