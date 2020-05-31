---
layout: post
title:  "CountDown en VueJS"
author: brayvasq
description: ""
categories: [tutorial, VueJS]
tags: [Vue, básico, desarrollo web, framework,setup,desarrollo]
---

En el presente tutorial vamos a crear un cownt down o cuenta regresiva con el framework VueJS, esto con el fin de dar un vistazo general al framework.

Antes de empezar debemos tener instalado nodejs y su gestor de paquetes npm.

## Instalación VueJS

Vue se puede usar sin necesidad de instalación incluyendo su script en los archivos html.

```html
<script src="https://cdn.jsdelivr.net/npm/vue"></script>
```

Lo anterior puede servir para proyectos sencillos, en caso de que la aplicación sea más robusta se recomienda instalar *vue-cli*.

En caso de usar linux los siguientes comandos se deben ejecutar como administrador o superusuario (sudo).

```shell
npm -g install vue
```

Ahora vamos a instalar vue-cli

```shell
npm install --global vue-cli
```

Para el proyecto del count-down se puede utilizar la primera alternativa. Sin embargo, para ver la creación de un proyecto con vue-cli utilizaremos la segunda alternativa.

## Iniciar Proyecto CountDown

Para la creación de la estructura del proyecto usaremos el siguiente comando:

```shell
vue init webpack count-down
```

El comando anterior nos creará un proyecto basado en una plantilla webpack, proporcionando así configuraciones por defecto para el proyecto. Al ejecutar el comando se nos preguntara varias cosas para configurar el proyecto.

- **Nombre del proyecto** - Project name (por defecto es el nombre que se colocó en el comando de creación).
- **Descripción** - Project description.
- **Author**.
- etc.

Cuando nos mencione Vue build daremos enter y a las siguientes opciones le diremos no ya que no serán necesarias para el actual ejemplo.

Al llegar a la instalación nos pedirá qué gestor de paquetes usar: npm, yarn.etc. elegiremos el que más nos guste (en mi caso npm). Finalmente, se nos creará el proyecto, quedando así la configuración:

![VueJSCLI]({{ "/assets/media/imagenes/count-down/VueJSCLI.png" | absolute_url }})

La estructura del proyecto quedará así:

```bash
count-down
├── build/
├── config/
├── node_modules/
├── index.html
├── package.json
├── package-lock.json
├── README.md
├── src/
│   ├── App.vue
│   ├── assets/
│   │   └── logo.png
│   ├── components/
│   │   └── HelloWorld.vue
│   └── main.js
└── static/
```

Veremos a continuación, una descripción general de algunos directorios y archivos del proyecto:

- **build/:** Este directorio contiene las configuraciones reales tanto para el servidor en desarrollo como para construcción de la app en producción.
- **config/index.js:** archivo de configuración principal
- **src/:** contendrá el código fuente de nuestros componentes y la lógica del proyecto.
- **index.html:** Plantilla principal de nuestra aplicación SPA.
- **package.json:** Contiene todas las dependencias de compilación y comandos de compilación.

Ahora ejecutaremos el siguiente comando para instalar las dependencias para del proyecto.

```bash
npm install
```

Podemos ejecutar el proyecto mediante el siguiente comando:

```bash
npm run dev
```

Esto iniciará el servidor en el puerto 8080 y podemos verificar ingresando la url: http://localhost:8080 en nuestro navegador, verán lo siguiente:

![VueJSInicio]({{ "/assets/media/imagenes/count-down/VueJSInicio.png" | absolute_url }})

## Componentes

El sistema de componentes es otro concepto importante en Vue, porque es una abstracción que nos permite construir aplicaciones a gran escala compuestas por componentes pequeños, autocontenidos y, a menudo, reutilizables.  Los componentes nos permiten estructurar y hacer nuestro código más legible.

Vamos a abrir el proyecto en el editor de código de preferencia y crearemos un nuevo archivo llamado `Countdown.vue` en la siguiente ruta `src/components`. Podemos borrar el archivo `HelloWorld.vue`.

La estructura del directorio `src/` quedará así:

```bash
src
├── App.vue
├── assets
│   └── logo.png
├── components
│   └── Countdown.vue
└── main.js
```

Vamos a empezar a trabajar en nuestro componente `Countdown`. Empezaremos creando un template, copiaremos el siguiente código al principio del archivo `Countdown.vue`.

```vue
<template>
  <div class="count-down">
    <div>
      <input type="datetime-local" v-model="datend" name="endtime">
    </div>
    <div class="">
      <div class="block">
        <p class="digit">{{ days }}</p>
        <p>Días</p>
      </div>
      <div class="block">
        <p class="digit">{{ hours }}</p>
        <p>Horas</p>
      </div>
      <div class="block">
        <p class="digit">{{ minutes }}</p>
        <p>Minutos</p>
      </div>
      <div class="block">
        <p class="digit">{{ seconds }}</p>
        <p>Segundos</p>
      </div>
    </div>
    <div v-if="noactive" class="texto">
      EXPIRED
    </div>
  </div>
</template>
```

Cómo se puede ver el contenido del template son etiquetas `html` normales, solo qué están dentro de la etiqueta `<template></template>`. Ahora se mostrará una breve explicación del contenido del template:

- Se tiene un input para ingresar la fecha de fin de la cuenta regresiva.
- Los siguientes elementos contienen etiquetas de párrafo `<p>` para mostrar la cuenta regresiva; Nótese que se está usando la interpolación de cadenas.
- Finalmente se tiene un `div` que contiene una directiva `v-if` para mostrar si la cuenta regresiva ya terminó.

**Nota :** Si no se tiene conocimiento acerca de la interpolación de cadenas y las directivas, se recomienda revisar el tutorial [Iniciando con VueJS](http://brayvasq.github.io/desarrollo/tutorial/vuejs/web/2018/03/18/1.iniciando-con-vue.html).

Ahora se añadirá funcionalidad al componente `Countdown.vue`, vamos a añadir el siguiente código después del template en el archivo `Countdown.vue`.

```vue
<script>
export default {
  name: 'Countdown',
  data () {
    return {
      hours:0,
      days: 0,
      minutes: 0,
      seconds:0,
      datend: '2018-12-30T00:01',
      noactive: true
    }
  },
  mounted(){
    setInterval(this.updateCounter,1000)
  },
  methods: {
    updateCounter(){
      let now = new Date();
      let end = new Date(this.datend);
      let distance = end - now;
      if(distance > 0){
        this.hours = Math.floor((distance % (1000 * 60 * 60 * 24)) / (1000 * 60 * 60));
        this.minutes = Math.floor((distance % (1000 * 60 * 60)) / (1000 * 60));
        this.days = Math.floor(distance / (1000 * 60 * 60 * 24));
        this.seconds = Math.floor((distance % (1000 * 60)) / 1000);
        this.noactive = false;
      }else{
        this.hours = 0;
        this.minutes = 0;
        this.days = 0;
        this.seconds = 0;
        this.noactive = true;
      }
    },
  }
}
</script>
```

Nótese que Vue usa la sintaxis de ES5 (Ecmascript 5). Como pueden ver la propiedad `data` en este componente es un método, esto porque el archivo `Countdown.vue` no es el componente principal de la aplicación.

El método `mounted` se invoca una vez la instancia del componente es montada. Por eso, aquí se puso el método setInterval.

Ahora tenemos creado el componente, falta incluirlo en el componente principal de la aplicación.

En el archivo `App.vue` ubicado en la carpeta `src/`, vamos a borrar las referencias al componente `HelloWorld` y vamos a incluir las referencias a el componente `Countdown.vue`. Quedando así la sección del template del archivo `App.vue`:

```vue
<template>
  <div id="app">
    <img src="./assets/logo.png">
    <Countdown/>
  </div>
</template>
```

Y así la sección del script del mismo archivo

```vue
<script>
import Countdown from './components/Countdown'

export default {
  name: 'App',
  components: {
    Countdown
  }
}
</script>
```

Ejecutamos el proyecto e ingresamos a http://localhost:8080.

![VueJSFuncionando]({{ "/assets/media/imagenes/count-down/VueJSFuncionando.gif" | absolute_url }})

Finalmente vamos a añadir estilos para mejorar el aspecto de la aplicación. En el archivo `Countdown.vue` después de la sección de script vamos a añadir lo siguiente:

```vue
<style scoped>
  @import url('https://fonts.googleapis.com/css?family=Varela+Round');
  input{
    margin-left: 2em;
    box-sizing: border-box;
    padding: 10px 1px;
    color: white;
    border:none;
    outline:none;
    background-color:transparent;
    border-bottom: 2px solid white;
    font-size: 25px;
  }
  input:hover{
    border-bottom: 2px solid #9E9E9E;
  }
  .block{
    color:white;
    float:left;
    margin-left: 2em;
    margin-top: 1em;
    margin-bottom: 1em;
    font-size: 40px;
  }
  .texto{
    font-size: 30px;
    color: red;
  }
  .count-down{
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    margin-top: 10%;
  }
</style>
```

En la etiqueta `<style scoped>` se coloca la palabra `scoped` para que ese css se aplique sólo al componente `Countdown`.

En el archivo `App.vue` la sección de estilos `<style>` quedará así:

```vue
<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
body{
  background-color: #212121;
}
</style>
```

## Resultado

![VueJSEstilos]({{ "/assets/media/imagenes/count-down/VueJSEstilos.gif" | absolute_url }})

Ahora ya conocemos cómo crear un proyecto vue mediante vue-cli, logramos ver lo fácil qué es crear componentes y podemos empezar a crear aplicaciones un poco más robustas con este gran framework.

Pueden encontrar el código aquí: [count-down](https://github.com/brayvasq/count-down)
