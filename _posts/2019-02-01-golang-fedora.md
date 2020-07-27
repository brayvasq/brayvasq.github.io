---
layout: post
title:  "Go(Golang) - Instalación Fedora"
author: brayvasq
description: ""
categories: [tutorial]
tags: [desarrollo, dotnet, setup, Go,Golang]
---

Para instalar Go o Golang en fedora basta con ejecutar el siguiente comando:

```bash
sudo dnf install golang
```

Ahora los binarios de go y gofmt estarán instalados en el sistema.

Podemos verificar su instalación y su versión con el siguiente comando:

```go
go version
```

Ahora vamos a ver las variables de entorno `$GOPATH` y `$GOROOT`

**GOPATH**: Aquí se van a instalar las librerías externas a la instalación original de golang, generalmente aquí quedan las librerías instaladas mediante `go get`.

Podemos verificar la variable mediante el siguiente comando:

```go
go env GOPATH
```

Lo anterior mostrará la ruta referida a GOPATH, generalmente esta ruta es : `/home/usuario/go` donde usuario es el nombre de usuario, si el usuario es pepito entonces la ruta seria: `/home/pepito/go`.

En caso de qué no esté definida la variable de entorno GOPATH se debe realizar lo siguiente:

1. Crear directorio de go en home

   ```bash
   mkdir -p $HOME/go
   ```

   El parámetro `-p` es para indicarle a `mkdir ` que debe crear el directorio padre en caso de ser necesario.

2. Crearemos la variable de entorno y la enviaremos al archivo de configuración de bash para el usuario actual.

   ```bash
   echo 'export GOPATH=$HOME/go' >> $HOME/.bashrc
   ```

3. Verificamos que la variable de entorno haya sido creada.

   ```bash
   source $HOME/.bashrc
   ```

4. Verificamos de nuevo con `go env`

   ```go
   go env GOPATH
   ```

**GOROOT** : Aquí es donde se encuentra instalado golang, es decir, aquí se van a encontrar los componentes básicos de golang para su funcionamiento.

Podemos verificar la variable mediante el siguiente comando:

```go
go env GOROOT
```

Generalmente la ruta de instalación es `/usr/lib/golang`; Sin embargo, si se hizo una instalación manual la ruta será el directorio donde quedó instalado go.

En caso de qué no esté definida la variable de entorno GOPATH se debe realizar lo siguiente:

1. Crearemos la variable de entorno y la enviaremos al archivo de configuración de bash para el usuario actual.

   ```bash
   echo 'export GOROOT=ruta' >> $HOME/.bashrc
   ```

Donde ruta será el directorio de instalación de golang.

2. Añadiremos a las variables de entorno los binarios de golang

   ```bash
   echo export 'PATH=$PATH:$GOROOT/bin' >> $HOME/.bashrc
   ```

3. Verificamos que la variable de entorno haya sido creada.

   ```bash
   source $HOME/.bashrc
   ```

4. Verificamos de nuevo con `go env`

   ```go
   go env GOROOT
   ```

Ahora tendremos instalado el lenguaje de programación golang.
