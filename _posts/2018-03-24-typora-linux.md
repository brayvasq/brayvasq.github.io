---
layout: post
title:  "Typora - Instalación Linux"
author: brayvasq
description: ""
categories: [tutorial]
tags: [markdown, typora, setup]
---
Typora es un editor markdown minimalista que sigue el paradigma wysiwym (What you see is what you mean).
Para instalar typora se debe descargar el paquete `.tar.gz` desde la siguiente url: http://support.typora.io/Typora-on-Linux/

ir a la sección **Other Distributions** y elegir el paquete de acuerdo a la arquitectura del sistema operativo.

ir a la carpeta donde se descargó el paquete y descomprimir con el siguiente comando:

```bash
tar -xvf Typora-linux-x64.tar.gz
```

Ahora Typora se puede utilizar ejecutando el archivo "Typora" ubicado dentro de la carpeta descomprimida. Sin embargo, por orden, vamos a mover la carpeta que resultó de descomprimir el archivo `.tar.gz` a la carpeta `/opt`, esto se hace con el siguiente comando:

```bash
sudo mv Typora-linux-x64 /opt
```

Ahora Typora se encuentra en la carpeta `/opt`. No obstante, para ejecutar typora aún se debe ir a la carpeta y ejecutar el archivo "Typora"; Para evitar esto, vamos a crear un archivo de escritorio `.desktop`. A continuación, se indicarán los pasos para crearlo:

1. Crear archivo `Typora.desktop`

```bash
touch Typora.desktop
```

2. Vamos a escribir lo siguiente en el archivo `Typora.desktop`

```bash
[Desktop Entry]
Name=Typora
Type=Application
Exec=/opt/Typora-linux-x64/Typora
Terminal=false
Icon=/opt/Typora-linux-x64/resources/app/asserts/icon/icon_32x32.png
Comment=Editor Markdown Typora
NoDisplay=false
Categories=Utility;TextEditor;
Name[en]=Typora.desktop
```

3. Finalmente vamos a mover el archivo `Typora.desktop` a la carpeta que contiene los archivos `.desktop` de las aplicaciones instaladas.

```bash
sudo cp Typora.desktop /usr/share/applications
```

4. Como paso opcional, vamos a copiar el archivo `.desktop` al escritorio.

```bash
sudo cp Typora.desktop /home/nombre_usuario/Escritorio
```

​	o dependiendo del idioma del sistema operativo.

```bash
sudo cp Typora.desktop /home/nombre_usuario/Desktop
```

Ahora ya tenemos instalado Typora en Linux, para usarlo basta con escribir typora en el buscador de aplicaciones de nuestro entorno de escritorio y dar click o en caso de haber creado una copia del archivo `.desktop` en el escritorio, simplemente se debe dar doble click sobre él.
