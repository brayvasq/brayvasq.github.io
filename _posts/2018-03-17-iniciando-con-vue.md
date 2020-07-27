---
layout: post
title:  "Iniciando Con VueJS"
author: brayvasq
description: ""
categories: [tutorial, VueJS]
tags: [Vue, básico, desarrollo web,desarrollo]
---

VueJS es un framework  de código abierto para la construcción de interfaces de usuario; Que Organiza y simplifica un poco el desarrollo web. Es simple y fácil de entender, proporcionando una gran alternativa a React o Angular.

A continuación, vamos a explorar algunos elementos básicos de VueJS.

## Uso

Vue se puede usar sin necesidad de instalación incluyendo su script en los archivos html.

```html
<script src="https://cdn.jsdelivr.net/npm/vue"></script>
```

Lo anterior puede ser útil en proyectos sencillos, en caso de que la aplicación sea más robusta se recomienda instalar [*vue-cli*](https://cli.vuejs.org/).

Vamos a crear un archivo html (index.html) y colocaremos la plantilla básica de una página web.

El proyecto o directorio se podría ver así:

```bash
vue-ejemplo/
└── index.html
```

El archivo index.html tendrá lo siguiente:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>VueJS-Ejemplos</title>
</head>
<body>
  	<!--Contenedor de nuestra app-->
    <div id="app">
    </div>
  	<!--Script de Vue-->
    <script src="https://cdn.jsdelivr.net/npm/vue"></script>
</body>
</html>
```

Podemos ver que en la estructura se creó un div con id "app", este será el div que usaremos para trabajar con vue. Ahora vamos a proceder a crear el "Hola Mundo" con vue.

Cambiaremos el contenido del div para que quede así:

```html
<div id="app">
  <div>
    <!-- interpolación de cadenas -->
  	<h1>{{ mensaje }}<h1>
  </div>
</div>
```

- `{{ mensaje }}` a esta sintaxis se le llama interpolación de cadenas en vue, esto nos permite renderizar la plantilla con los datos que nos provee vue.

Ahora crearemos un archivo llamado `index.js` y agregaremos el script al archivo `index.html`.

Estructura del proyecto:

```bash
vue-ejemplo/
├── index.html
└── index.js
```

Archivo `index.js`:

```javascript
var app = new Vue({
    el: '#app',
    data: {
      message: 'Hola Mundo Con Vue!'
    }
});
```

- podemos ver que en el objeto data está el dato mensaje que posteriormente se va a renderizar en el template.

En el archivo `index.html` después de importar el script de vue, colocaremos la siguiente línea:

```html
<script src="index.js"></script>
```

Ahora podremos verificar en el navegador que el "hola mundo" funciona.

## Directivas

Las directivas están precedidas por `v-` para indicar que son atributos especiales provistos por vue.

A continuación, vamos a explorar algunas directivas de vue.

### Condicionales y Ciclos

#### v-if

Esta directiva nos permite realizar condicionales desde el template.

Vamos a agregar el siguiente contenido a nuestro `div id="app"` en el archivo `index.html`.

```html
<div>
    <!-- Condicional -->
   	<h1 v-if="ver">Condición Verdadera</h1>
</div>
```

- Ahora el `h1` solo se verá si el dato `ver` es `true`.

Vamos a agregar el dato `ver` a nuestra propiedad data en el archivo `index.js`, quedando así:

```javascript
var app = new Vue({
    el: '#app',
    data: {
      message: 'Hola Mundo Con Vue!',
      ver: true
    }
});
```

Verificamos en el navegador que el código funcione. Si colocamos el dato `ver` en `false` el mensaje de la etiqueta `h1` ya no debería mostrarse.

#### v-else-if y v-else

Para añadir un bloque de condiciones, se puede usar las directivas `v-else-if` y `v-else`.

La directiva `v-else-if` se usa en caso de que la condición que la precede no se cumpla, pero existe otra condición que se debe verificar.

La directiva `v-else` se usa en caso de que las condiciones anteriores no se cumplan pero ya no queda algo que verificar.

Vamos a agregar el siguiente contenido a nuestro `div id="app"` en el archivo `index.html`

```html
<div>
    <!-- Bloque Condicional -->
    <h1 v-if="valor===1">Valor es Uno</h1>
  	<h1 v-else-if="valor===2">Valor es Dos</h1>
	<h1 v-else>Valor es diferente a Uno y Dos</h1>
</div>
```

- Podemos ver que en el apartado de la condición no solo debe ir un valor o dato, sino que también se puede indicar condiciones específicas.

Vamos a cambiar el contenido de nuestra propiedad `data` en el archivo `index.js`

```javascript
var app = new Vue({
    el: '#app',
    data: {
      mensaje: 'Hola Mundo Con Vue!',
      ver: true,
      valor: 3
    }
});
```

Verificamos en el navegador que el código funcione.

#### v-for

Podemos usar la directiva `v-for` para recorrer una lista o matriz y renderizar sus datos en el template.

Vamos a agregar el siguiente contenido a nuestro `div id="app"`  en el template

```html
<div>
	<!-- Ciclo For - Lista-->
    <ul>
    	<li v-for="fruta in frutas">
        	{{ fruta.nombre }}
        </li>
	</ul>
</div>
```

Ahora vamos a añadir la lista en la propiedad `data` del archivo `index.js`

```javascript
var app = new Vue({
    el: '#app',
    data: {
      message: 'Hola Mundo Con Vue!',
      ver: true,
      valor: 3,
      frutas: [
        { nombre: 'Manzana' },
        { nombre: 'Naranja' }
      ]
    }
});
```

Verificamos en el navegador que el código funcione.

También podemos recorrer un diccionario mostrando su llave y su valor, Así:

Agregar el siguiente contenido al `div id="app"` del archivo `index.html`

```html
<div class="mitad-2 center">
	<!-- Ciclo For - LLave valor-->
    <ul>
    	<li v-for="(valor,llave) in persona">
        	{{ llave }} - {{ valor }}
        </li>
    </ul>
</div>
```

Archivo `index.js`

```javascript
var app = new Vue({
    el: '#app',
    data: {
      message: 'Hola Mundo Con Vue!',
      ver: true,
      valor: 3,
      frutas: [
        { nombre: 'Manzana' },
        { nombre: 'Naranja' }
      ],
      persona:{
        nombre:"juanito",
        apellido: "arcoiris",
        edad: 10
      }
    }
});
```

Verificamos en el navegador que el código funcione.

### Interacción con el usuario

#### v-on

Para permitir que los usuarios interactúen con su aplicación, podemos usar la directiva v-on para adjuntar detectores de eventos.

Por ahora solo exploraremos el evento `click` de un botón.

Vamos a hacer que al dar click en un botón muestre u oculte un mensaje, de manera qué modificaremos el `div` que contiene el primer ejemplo de la directiva `v-if` dentro de nuestro `div id="app"`, quedando así:

```html
<div>
    <!-- Condicional -->
  	<h1 v-if="ver">Condición Verdadera</h1>
	<button v-on:click="visualizar">Ver Mensaje</button>
</div>
```

- en la directiva `v-on:click` se colocará la función que se va a invocar al dar click en el botón.

En el archivo `index.js` crearemos la función `visualizar`

```javascript
var app = new Vue({
    el: '#app',
    data: {
      message: 'Hola Mundo Con Vue!',
      ver: true,
      valor: 3,
      frutas: [
        { nombre: 'Manzana' },
        { nombre: 'Naranja' }
      ],
      persona:{
        nombre:"juanito",
        apellido: "arcoiris",
        edad: 10
      }
    },
    methods:{
      visualizar:function(){
        this.ver = !this.ver;
      }
    }
});
```

- En la propiedad `methods` se colocarán las funciones que utilizaremos en nuestra aplicación. Podemos ver que la sintaxis es la de un diccionario y que el método visualizar tiene como valor una función a la cual se le podría especificar parámetros de ser necesario.

Verificamos en el navegador que el código funcione.

#### v-model

La directiva `v-model` nos proporciona una vinculación bidireccional en un elemento o componente de entrada. Sincronizando la entrada del usuario con el resultado en nuestra aplicación.

Vamos a ver la directiva `v-model` en acción a continuación:

En el archivo `index.html` añadiremos a nuestro  `div id="app"` el siguiente contenido:

```html
<div>
    <p>{{ palabra }}</p>
    <input v-model="palabra">
</div>
```

vamos a añadir la variable  o dato `palabra` en la propiedad `data` de vue en el archivo `index.js`

```javascript
var app = new Vue({
    el: '#app',
    data: {
      message: 'Hola Mundo Con Vue!',
      ver: true,
      valor: 3,
      palabra: 'Hola',
      frutas: [
        { nombre: 'Manzana' },
        { nombre: 'Naranja' }
      ],
      persona:{
        nombre:"juanito",
        apellido: "arcoiris",
        edad: 10
      }
    },
    methods:{
      visualizar:function(){
        this.ver = !this.ver;
      }
    }
});
```

Verificamos en el navegador que el código funcione.

## Resultado

Pueden ver el ejemplo funcionando en el siguiente link: [Vue-ejemplo codepen](https://codepen.io/brayvasq/pen/bYXwjb)

<p data-height="265" data-theme-id="0" data-slug-hash="bYXwjb" data-default-tab="result" data-user="brayvasq" data-embed-version="2" data-pen-title="Vue Example" class="codepen">See the Pen <a href="https://codepen.io/brayvasq/pen/bYXwjb/">Vue Example</a> by brayan vasquez (<a href="https://codepen.io/brayvasq">@brayvasq</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

Ahora ya conocemos los elementos básicos de VueJS, logramos ver lo fácil qué es usarlo y podemos empezar a crear aplicaciones con este gran framework.
