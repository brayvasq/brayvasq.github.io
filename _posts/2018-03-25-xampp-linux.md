---
layout: post
title:  "Xampp - Instalación Linux"
author: brayvasq
description: "Instalación del entorno Xampp en linux"
categories: [tutorial]
tags: [servidor, xampp, setup, web]
toc: true
featured: true
---
Vamos a ir a la página oficial de xampp: https://www.apachefriends.org/es/index.html y daremos click en el botón XAMPP para Linux, empezara a descargarse un archivo `.run`.

Una vez descargado el archivo, nos dirigimos a la carpeta donde se descargó y abriremos una terminal. En la terminal haremos lo siguiente:

- Dar permisos de ejecución al archivo `.run`

```bash
chmod +x xampp-linux-x64-version-installer.run
```

- Ejecutar el archivo

```bash
sudo ./xampp-linux-x64-7.2.2-0-installer.run
```

Seguiremos los pasos de instalación. Es una instalación al estilo siguiente de windows.

Xampp quedará instalado en la carpeta `/opt/lampp` de nuestro sistema operativo; Para usar Xampp solo tendremos que ejecutar el archivo `manager-linux-x64.run`. Sin embargo, lo anterior es un proceso un poco tedioso por lo que es recomendable crear un archivo `.desktop`. A continuación, se indican los pasos para crearlo:

1. Crear un archivo llamado `Xampp.desktop`

```bash
touch Xampp.desktop
```

2. Vamos a escribir lo siguiente en el archivo `Xampp.desktop`

```bash
[Desktop Entry]
Name=Xampp
Type=Application
Exec=sudo /opt/lampp/manager-linux-x64.run
Terminal=true
Icon=/opt/lampp/htdocs/favicon.ico
Comment=Lampp Server
NoDisplay=false
Categories=Utility;Development;
Name[en]=Xampp.desktop
```

3. Finalmente vamos a mover el archivo `Xampp.desktop` a la carpeta que contiene los archivos `.desktop` de las aplicaciones instaladas.

```bash
sudo cp Xampp.desktop /usr/share/applications
```

4. Como paso opcional, vamos a copiar el archivo `.desktop` al escritorio.

```bash
sudo cp Xampp.desktop /home/nombre_usuario/Escritorio
```

​	o dependiendo del idioma del sistema operativo.

```bash
sudo cp Xampp.desktop /home/nombre_usuario/Desktop
```

Ahora ya tenemos instalado Xampp en Linux, para usarlo basta con escribir xampp en el buscador de aplicaciones de nuestro entorno de escritorio y dar click sobre él. Al ejecutar el archivo aparecerá una terminal donde digitaremos la clave de usuario y finalmente se ejecutará xampp.
