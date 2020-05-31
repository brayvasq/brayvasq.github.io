---
layout: post
title:  "Instalación de lenguajes y herramientas de desarrollo en Fedora"
author: brayvasq
description: ""
categories: [tutorial]
tags: [NodeJS, VScode,Sublime Text, Atom, setup]
toc: true
---

A continuación se indica cómo se realiza la instalación de algunos lenguajes de programación y herramientas de desarrollo en Fedora.

## Atom

Descargar el paquete `.rpm` en : https://atom.io/

Ubicarse en la carpeta donde se descargó atom.

Instalar atom mediante el siguiente comando:

```bash
sudo rpm -ivh atom.x86_64.rpm 
```

## VSCode

Actualmente  VSCode posee un repositorio en un repositorio yum, el siguiente script instalará la clave y el repositorio:

```bash
sudo rpm --import https://packages.microsoft.com/keys/microsoft.asc
```

```bash
sudo sh -c 'echo -e "[code]\nname=Visual Studio Code\nbaseurl=https://packages.microsoft.com/yumrepos/vscode\nenabled=1\ngpgcheck=1\ngpgkey=https://packages.microsoft.com/keys/microsoft.asc" > /etc/yum.repos.d/vscode.repo'
```

Luego se debe actualizar la caché de paquetes e instalar el paquete con dnf (Fedora 22 y superior):

```bash
dnf check-update
sudo dnf install code
```

## NodeJS

Instalar nodejs mediante dnf en fedora :

```bash
sudo dnf install -y nodejs
```

Para versiones superiores a fedora 23, `npm` también es instalado. Por lo tanto, si la versión de fedora es 23 o menor, se debe instalar npm por separado:

```bash
sudo dnf install -y npm
```

## Lenguaje R

Para instalar el lenguaje r bastará con usar el siguiente comando

```bash
sudo dnf install R
```

### R Studio

Para instalar el entorno RStudio debemos descargar el paquete correspondiente a la distribución usada, en este caso fedora.

Link de descarga: https://www.rstudio.com/products/rstudio/download/#download

Ahora solo queda instalar el paquete.

```bash
sudo rpm -ivh nombre_paquete
```

## GNU Octave

Para instalar el proyecto GNU Octave bastará con usar el siguiente comando.

```bash
sudo dnf install octave
```

## Eclipse

Para instalar eclipse en fedora basta con ejecutar el siguiente comado:

```bash
sudo dnf install eclipse
```

