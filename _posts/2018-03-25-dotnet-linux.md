---
layout: post
title:  ".Net Core - Instalación Linux"
author: brayvasq
description: ""
categories: [tutorial]
tags: [desarrollo, dotnet, setup, C#]
---

Para instalar .net core o dotnet para linux, existen dos opciones; La primera opción es instalar dotnet desde los repositorios de fedora. La segunda opción es instalar mediante los repositorios que brinda microsoft.

### Instalación mediante repositorios copr de fedora

1. Habilitar el repositorio .NET SIG copr

   ```bash
   sudo dnf copr enable @dotnet-sig/dotnet
   ```

2. Instalar .Net Core SDK para poder construir aplicaciones con dotnet.

   ```bash
   sudo dnf install dotnet-sdk-2.0
   ```

3. Instalar .Net Core Runtime

   ```bash
   sudo dnf install dotnet-runtime-2.0
   ```

4. Verificar que dotnet se instaló y su versión.

   ```bash
   dotnet --version
   ```

### Instalación mediante repositorios de microsoft

1. Agregar la llave o signature key de Microsoft.

   ```bash
   sudo rpm --import https://packages.microsoft.com/keys/microsoft.asc
   ```

2. Agregar repositorio de dotnet

   ```bash
   sudo sh -c 'echo -e "[packages-microsoft-com-prod]\nname=packages-microsoft-com-prod \nbaseurl=https://packages.microsoft.com/yumrepos/microsoft-rhel7.3-prod\nenabled=1\ngpgcheck=1\ngpgkey=https://packages.microsoft.com/keys/microsoft.asc" > /etc/yum.repos.d/dotnetdev.repo'
   ```

3. Actualizar.

   ```bash
   sudo dnf update
   ```

4. Instalar dependencias.

   - Para Fedora 26 o más recientes:

     ```bash
     sudo dnf install libunwind libicu compat-openssl10
     ```

   - Para otras versiones:

     ```bash
     sudo dnf install libunwind libicu
     ```

5. Instalar .Net SDK

   ```bash
   sudo dnf install dotnet-sdk-2.1.4
   ```

6. Verificar que dotnet se instaló y su versión.

   ```bash
   dotnet --version
   ```

Ahora tenemos instalado dotnet core en fedora linux y podremos empezar a crear aplicaciones con la versión opensource de .Net, para aprender más sobre dotnet core pueden consultar su documentación, la cual es bastante buena y completa.

Documentación de dotnet core: https://dotnet.github.io/
