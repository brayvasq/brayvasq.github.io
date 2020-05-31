---
layout: post
title:  "Dropbox - Instalación Fedora"
author: brayvasq
description: ""
categories: [tutorial]
tags: [dropbox, linux, fedora, setup]
---


Para instalar dropbox basta con ejecutar el siguiente comando en fedora:

```bash
sudo dnf install dropbox
```

Ahora, vamos a instalar el paquete que nos permitirá conectar dropbox con nuestro gestor de archivos.

**Caja** :

```bash
sudo dnf install caja-dropbox
```

**Nautilus**

```bash
sudo dnf install nautilus-dropbox
```

**Nemo**

```bash
sudo dnf install nemo-dropbox
```

Para otros gestores de archivos se debe instalar o añadir otro repositorio mediante el siguiente comando.

```bash
sudo wget -P /etc/yum.repos.d/ https://raw.github.com/kuboosoft/postinstallerf/master/postinstallerf.repo
```

Ahora podremos instalar el soporte de dropbox para Thunar y Dolphin

**Dolphin**

```bash
sudo dnf install -y dolphin-dropbox-plugin
```

**Thunar**

```bash
sudo dnf install -y thunar-dropbox
```
