---
layout: post
title:  "Guía Básica de Git"
author: brayvasq
date:   2018-03-17 13:29:50 -0500
description: "Guía de algunos comandos básicos de git para familiarizarse con esta herramienta"
categories: tutorial
tags: versiones git setup desarrollo
---

Git es un sistema de control de versiones, es de código abierto y gratuito, diseñado para manejar todo, desde proyectos pequeños a muy grandes, con velocidad y eficiencia.

A continuación, vamos a explorar los comandos básicos de git. Para la ejecución de dichos comandos es necesario tener git instalado en el equipo; en el siguiente link están las instrucciones de descarga e instalación de este: https://git-scm.com/downloads

### Versión

Podemos visualizar la versión de git mediante el comando:

```bash
git --version
```

### Configuración General

Asignación del nombre de usuario de manera global. Generalmente el nombre de usuario (username) es el mismo que se usa en github,bitbucket,etc.

```bash
git config --global user.name username
```

Visualización del nombre de usuario:

```bash
git config --global user.name
```

El correo del usuario corresponde al que se registró en github,bitbucket,etc.

```bash
git config --global user.email email@servidor.com
```

Visualización del correo de usuario:

```bash
git config --global user.email
```

Activar colores de git en la terminal:

```bash
git config --global color.ui true
```

Visualizar la configuración global:

```bash
git config --global --list
```

**Nota:** Cabe resaltar que toda la configuración realizada anteriormente tiene sentido solo si se trabaja con repositorios remotos. Más adelante se explicarán comandos relacionados con repositorios remotos.

### Inicialización de un repositorio

Si la idea es comenzar un repositorio desde cero, se debe ejecutar el siguiente comando dentro de la carpeta del proyecto.

```bash
git init
```

### Estado del repositorio

La verificación del estado del repositorio nos permite visualizar los archivos que se han modificado y que se deberían añadir a un commit. Esto se puede realizar mediante el comando:

```bash
git status
```

### Añadir los archivos con cambios al repositorio local

Se pueden añadir todos los archivos cambiados mediante el comando:

```bash
git add -A
```

o mediante

```bash
git add .
```

Si se quiere especificar qué archivos añadir, se puede hacer de la siguiente manera:

```bash
git add archivo
```

Para verificar si los archivos se añadieron se puede verificar de nuevo el estado del proyecto mediante un `git status`.

### Commits

Para guardar un estado del proyecto, es decir, guardar los cambios realizados se debe realizar un commit. El comando para realizar un commit es el siguiente:

```bash
git commit -m "comentario del commit"
```

Ver los commits realizados:

```bash
git log
```

Exportar los commits a un archivo de texto:

```bash
git log > archivo
```

Volver a un commit anterior sin perder los cambios realizados:

```bash
git reset --soft idcommit
```

El id del commit aparece en el listado de commits que se muestra con `git log` Ej: ff89f3f.....

Volver a un commit anterior reestableciendo el código que estaba hasta dicho cmmit:

```bash
git reset --hard idcommit
```

**Notas:**

- los commits que existían después del commit al que se vuelve por medio de `reset` desaparecen.
- Hay otras opciones además de `--soft` y `--hard`.

### Ramas

Las ramas nos permiten realizar bifurcaciones al proyecto, de manera que se podría decir que se trabaja en otro entorno, sin afectar el proyecto principal, esto para que los cambios se integren a la rama master solo cuando sean aprobados y estén funcionando.

Establecer ubicación en una rama:

```bash
git checkout rama
```

Visualizar ramas:

```bash
git branch
```

Crear ramas:

```bash
git branch rama
```

Borrar una rama:

```bash
git branch -d rama
```

Fusionar ramas:

- Ubicarse en la rama que va a absorber.

```bash
git checkout rama_absorbe
```

- Absorber la rama.

```bash
git merge rama_absorbida
```

### Repositorios Remotos

Clonar repositorio:

```bash
git clone url_repositorio_clonar
```

Acceso remoto al repositorio:

- Añadir repositorio remoto

```bash
git remote add origin url_repositorio
```

- Verificar repositorio remoto

```bash
git remote -v
```

Borrar acceso a repositorio remoto:

```bash
git remote remove origin
```

**Nota**: Si el repositorio es clonado mediante terminal, el acceso remoto se añade automáticamente y tampoco es necesario inicializar el repositorio `git init`.

Subir cambios al repositorio remoto:

```bash
git push origin rama
```

Actualizar repositorio local:

```bash
git pull
```

**Nota:** no confundir con el pull request, lo que hace el `git pull` es actualizar el repositorio local con los datos del repositorio remoto.
