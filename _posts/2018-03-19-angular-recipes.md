---
layout: post
title:  "Tutorial Angular 5 - App recetas de comida"
author: brayvasq
image: assets/images/angular_logo.png
description: "Traducción al español del tutorial 'tour of heroes' de google para aprender Angular 5, utilizando un modelo de recetas básico en reemplazo de los heroes."
featured: true
categories: [tutorial, Angular]
tags: [Angular5, AngularCLI, básico, desarrollo web, framework,setup,desarrollo]
toc: true
---

Angular es un framework para la creación de páginas web SPA (single page aplication) o aplicaciones de una sola página. Es de código abierto y es mantenido por Google.Además, está programado en el lenguaje Typescript de microsoft.

Se puede decir que Angular es la evolución de la libreria angularJS y actualmente es uno de los frameworks para el desarrollo front-end más populares junto a ReactJS y VueJs. En comparación con VueJS, se puede notar que angular es un poco más robusto.

En el presente tutorial se creará una aplicación para guardar recetas de comida. El tutorial está basado en el tour of heroes ofrecido por angular y que se puede encontrar en el siguiente enlace: https://angular.io/tutorial

## Creación del Proyecto

Para la creación del proyecto usaremos Angular CLI que nos permite crear un proyecto a través de la linea de comandos.

Antes de empezar debemos asegurarnos de tener instalado en nuestro sistema tanto `node` como `npm`. Para verificar si tenemos instalados `node` y `npm` podemos usar los siguientes comandos:

```bash
node -v
```

```bash
npm -v
```

### Instalación Angular

El siguiente comando se debe ejecutar como superusuario `sudo` en sistemas Linux.

```bash
npm install -g @angular/cli
```

### Crear un nuevo proyecto

```bash
ng new recipes
```

### Ejecutar Aplicación

Ubicarse en la raiz del proyecto

```bash
cd recipes
```

Ejecutar el servidor

```bash
ng serve
```

Abrimos un navegador y vamos a la siguiente url: http://localhost:4200/. Verificamos que la app funcione correctamente.

### Directorios del proyecto.

- **src/** es el directorio principal ya que aquí van todos los componentes, plantillas, estilos, imagenes, etc. que nuestro proyecto necesita. Todo contenido fuera de este directorio ayuda a construir la aplicación.
- **app/app.component.** Define los elementos del componente principal, equivale a {`.css`|`.ts`|`.html`}
- **app/app.module.ts**  Define el modulo raíz que le indica a angular cómo construir la aplicación.
- **assets/** Una carpeta donde se pueden poner las imagenes y otros.
- **index.html** Página principal.
- **main.ts** Principal punto de entrada a la aplicación.
- **styles.css** Estilos globales.  

Para más información de los directorios revisar: https://angular.io/guide/quickstart#project-file-review

## Componentes de Angular

Al ejecutar la app, la página que se ve, está controlada por un componente de angular llamado AppComponent.

Los componentes son los elementos fundamentales de las aplicaciones en angular. Muestran datos en la pantalla, escuchan la entrada del usuario y toman medidas en función de esa entrada.

#### Cambiando el titulo de la aplicación

Vamos a ir al directorio `src/app` y vamos a ver cada elemento de nuestro componente.

- `app.component.ts` el código de la clase del componente, escrito en typescript.
- `app.component.html` la plantilla del componente, escrita en html.
- `app.component.css` los estilos especificos del componente.

Abriremos la clase `app.component.ts` y cambiaremos el valor de la propiedad `title`, así:

```typescript
title = 'Recipes App';
```

En el archivo `app.component.html`, reemplazamos su contenido por lo siguiente:

```html
<h1>{{title}}</h1>
```

En el archivo global de estilos `src/styles.css`, copiaremos lo siguiente:

```css
/* You can add global styles to this file, and also import other style files */
/* Application-wide Styles */
h1 {
    color: #11956c;
    font-family: Arial, Helvetica, sans-serif;
    font-size: 250%;
    display: flex;
    justify-content: center;
}
h2, h3 {
    color: #11956c;
    font-family: Arial, Helvetica, sans-serif;
    font-weight: lighter;
}
body {
    display: flex;
    justify-content: center;
}
body, input[text], button {
    color: #ffff;
    font-family: Cambria, Georgia;
}
  /* everywhere else */
* {
    font-family: Arial, Helvetica, sans-serif;
}
```

Ahora la aplicación tiene un titulo básico. A continuación, crearemos un nuevo componente para mostrar información de las recetas.

### Crear componente recetas

Para esto usaremos Angular CLI.

```bash
ng generate component recipes
```

Angular CLI crea una nueva carpeta `src/app/recipes` y genera los respectivos archivos del componente.

En el archivo `recipes.component.ts` podemos observar la anotación `@component`; `@Component` es una función de decorador que especifica los metadatos de angular para el componente.

También podemos observar el método `ngOnInit` que es llamado poco despues de que el componente se cree.

Para mostrar el componente recetas vamos al archivo `app.component.html` y añadimos la siguiente etiqueta.

```html
<app-recipes></app-recipes>
```

### Crear Clase Receta

Crearemos la clase recetas en la carpeta `src/app` con el nombre `recipe.ts`. Le daremos las propiedades de `id`, `nombre` y `descripción`. Quedado así:

```typescript
export class Recipe{
    id: number;
    name: string;
    description: string;
}
```

En el archivo `recipes.component.ts` vamos a crear una receta, quedando así el archivo:

```typescript
import { Component, OnInit } from '@angular/core';
import { Recipe } from '../recipe'

@Component({
  selector: 'app-recipes',
  templateUrl: './recipes.component.html',
  styleUrls: ['./recipes.component.css']
})
export class RecipesComponent implements OnInit {

  recipe : Recipe = {
    id: 1,
    name: 'Sandwich',
    description: 'Recipe Sandwich',
  };

  constructor() { }

  ngOnInit() {
  }
}
```

El archivo `recipes.component.html` va a quedar así:

```html
<h2>{{ recipe.name | uppercase }} Details</h2>
<div><span>id: </span>{{recipe.id}}</div>
<div><span>name: </span>{{recipe.name}}</div>
<div><span>description: </span>{{recipe.description}}</div>
```

Ejecutamos el proyecto y podemos ver lo siguiente:

![AngularBasic]({{ "/assets/media/imagenes/recipes/Inicio.PNG" | absolute_url }})

### Editando la Receta

vamos a modificar el archivo `recipes.component.html`

```html
<h2>{{ recipe.name | uppercase }} Details</h2>
<div><span>id: </span>{{recipe.id}}</div>
<div>
    <label>name:
      <input [(ngModel)]="recipe.name" placeholder="name">
    </label>
</div>
<div>
    <label>description:
      <input [(ngModel)]="recipe.description" placeholder="description">
    </label>
</div>
```

Podemos ver dos cosas interesantes:

1. la palabra `uppercase` despues del operadore de tuberia `|`. Las tuberias son una buena forma de formatear cadenas, cantidades de moneda,fechas y otros datos para visualizar.
2. la notación `[(ngModel)]` nos indica que hay un enlace bidireccional entre la propiedad del objeto `recipes` y el campo de texto de entrada,  de manera que si se modifica uno también se modificará el otro.

Para que la directiva `ngModel` funcione debemos hacer lo siguiente:

- Importar `FormsModule` en el archivo `app.module.ts`

```typescript
import { FormsModule } from '@angular/forms'; // <-- NgModel lives here
```

- Añadir el `FormsModule` al arreglo de importaciones de metadatos `@NgModule` que contiene una lista de módulos externos para que la aplicación funcione.

El archivo `app.module.ts` quedará así:

```typescript
import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';
import { FormsModule } from '@angular/forms'; // <-- NgModel lives here
import { AppComponent } from './app.component';
import { RecipesComponent } from './recipes/recipes.component';

@NgModule({
  declarations: [
    AppComponent,
    RecipesComponent
  ],
  imports: [
    BrowserModule,
    FormsModule
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }
```

Como podemos observar, al crear el componente `recipes` mediante AngularCLI, este añade el mismo a los modulos de la aplicación.

Ahora podemos ejecutar la aplicación y verificar su funcionamiento.
![ngmodule]({{ "/assets/media/imagenes/recipes/ngmodule.gif" | absolute_url }})

## Mostrando Lista de Recetas

Es hora de mostrar una lista de recetas, así cómo permitir al usuario seleccionar una y ver sus detalles.

### Creando lista de recetas

Necesitaremos crear algunas recetas para mostrar.

Eventualmente las recetas se obtendrán de un servidor de datos remoto. Por ahora, vamos a crear algunas recetas falsas.

Crearemos un archivo llamado `mock-recipes.ts` en la ruta `src/app/`. Deninimos una constante llamada `RECIPES` como un arreglo de 3 recetas (pueden agregar más) y la exportaremos. El archivo debería verse así:

```typescript
import { Recipe } from './recipe';

export const RECIPES: Recipe[] = [
    {
        id: 11,
        name: 'Tostadas integrales energéticas',
        description: 'Es absolutamente delicioso, nos da energía y es una buena idea para desayunar o comer algo de media mañana o media tarde'
    },
    {
        id: 12,
        name: 'Tabule de quínoa',
        description: 'Aunque se ven bastantes ingredientes en realidad es bastante sencillo'
    },
    {
        id: 13,
        name: 'Ensalada refrescante',
        description: 'Esta es un buen acompañamiento para un almuerzo o comida con proteína animal como carne o pollo.'
    },
];
```

**Nota :** La fuente de las recetas es la siguiente: https://tuhogar.com/co/receta/3-recetas-para-cocineros-primerizos-axion/.

### Mostrando las recetas

Modificaremos el componente `RecipesComponent` en `src/app/recipes/recipes.component.ts`.

Importamos el arreglo de recetas.

```typescript
import { RECIPES } from '../mock-recipes';
```

Cambiaremos la propiedad `recipe` por `recipes` y le asignaremos el arreglo `RECIPES`, así:

```typescript
recipes = RECIPES;
```

### Listar recetas con `*ngFor`

Abrimos el archivo de la plantilla `recipes.component.html` y reemplazamos su contenido por el siguiente:

```html
<h2>My Recipes</h2>
<ul class="recipes">
  <li *ngFor="let recipe of recipes">
    <a>
        <span class="badge">{{ recipe.id }}</span> {{ recipe.name }}
    </a>
  </li>
</ul>
```

El `*ngFor` es la directiva de repetición de Angular. Repite un elemento por cada elemento de la lista, en este caso repite el elemento `li` por cada receta de la lista.

Podemos actualizar la página del navegador y ver el resultado.

![ngfor]({{ "/assets/media/imagenes/recipes/ngFor.PNG" | absolute_url }})

### Añadiendo estilos a las recetas

La lista de recetas debe ser atractiva y debe responder visualmente cuando los usuarios interactuen con ellas.

Vamos a añadir los estilos al archivo de estilos del componente `recipes.component.css`, esto con el fin de no extender o saturar el archivo de estilos global `styles.css`. Recordemos que en la anotación `@Component` del archivo `recipes.component.ts` está definida la ruta de los estilos privados del componente.

Vamos a añadir lo siguiente al archivo `recipes.component.css`

```css
/* RecipesComponent's private CSS styles */
.center-element{
    display: flex;
    justify-content: center;
}
.selected {
    background-color: #CFD8DC !important;
    color: white;
}
ul {
    list-style-type: none;
}
.recipes a{
    float: left;
    width:90%;
	margin-top: 1em;
    text-decoration: none;
    color: white;
    padding: 10px;
    text-align: center;
    max-height: 120px;
    min-width: 120px;
    background-color: #11956c;
    border-radius: 2px;
}
.recipes a:hover{
    color: #11956c;
    background-color: #DDD;
    left: .1em;

}
.recipes li.selected:hover {
    background-color: #BBD8DC !important;
    color: white;
}
.recipes .badge{
    background-color: white;
    border-radius: 50%;
    padding: 4px;
    float: left;
    color: #11956c;
}
.recipes .text {
    position: relative;
    top: -3px;
}
```

### Detalles

Cuando el usuario hace click sobre algún elemento de la lista, el componente debe mostrar los detalles de la receta seleccionada.

Ahora veremos el evento de click para las recetas.

#### Añadir evento click

En el elemento `<li>` del archivo `recipes.component.html` vamos a agregar un enlace al evento click así:

```html
<li *ngFor="let recipe of recipes" (click)="onSelect(recipe)">
```

Los parentecis al rededor del `click` le dicen a angular que escuche el evento click de el elemento `<li>`. Cuando el usuario hace click sobre el elemento `<li>`, Angular ejecutará la expresión `onSelect(recipe)`.

#### Añadir manejador de evento

En el archivo `recipes.component.ts` vamos a añadir el método `onSelect()` y la propiedad `selectedRecipe`.

```typescript
selectedRecipe: Recipe;

onSelect(recipe: Recipe): void {
  this.selectedRecipe = recipe;
}
```

#### Actualizar la plantilla

Vamos a añadir lo siguiente a la plantilla `recipes.component.html`

```html
<h2>{{ selectedRecipe.name | uppercase }} Details</h2>
<div><span>id: </span>{{selectedRecipe.id}}</div>
<div>
  <label>name:
    <input [(ngModel)]="selectedRecipe.name" placeholder="name">
  </label>

  <label>description:
      <textarea [(ngModel)]="selectedRecipe.description" placeholder="description">
  </label>
</div>
```

Si recargamos la pestaña de la app, veremos que muestra un error en la consola del navegador, indicando que no puede leer la propiedad `name`. Lo anterior se debe a que en principio la propiedad `selectedRecipe` es `undefined`.

Para solucionar el problema planteado anteriormente basta con dar click en alguno. Sin embargo, no es el deber ser; Por lo que veremos cómo solucionar el problema con la directiva `*ngIf` de Angular.

#### Ocultar detalles vacíos con `*ngIf`

Reemplazaremos la sección de la receta selecionada con lo siguiente:

```html
<div *ngIf="selectedRecipe">
  <h2>{{ selectedRecipe.name | uppercase }} Details</h2>
  <div><span>id: </span>{{selectedRecipe.id}}</div>
  <div>
    <label>name:
      <input [(ngModel)]="selectedRecipe.name" placeholder="name">
    </label>

    <label>description:
        <input [(ngModel)]="selectedRecipe.description" placeholder="description">
    </label>
  </div>
</div>
```

Después de refrescar el navegador, la lista de recetas se muestra correctamente. El área de detalles entá en blanco y al hacer click sobre alguna receta aparecerán sus detalles.

![ngIf]({{ "/assets/media/imagenes/recipes/ngIf.gif" | absolute_url }})

#### Añadir estilo a la receta seleccionado

Los enlaces de clase de Angular hacen que sea fácil agregar y eliminar una clase de css condicionalmente en algún elemento del html.

Agregaremos el enlace `[class.selected]` al elemento `<li>` en la plantilla `app.component.html`.

```html
<li *ngFor="let recipe of recipes" [class.selected]="recipe === selectedRecipe" (click)="onSelect(recipe)">
    <a>
      <span class="badge">{{recipe.id}}</span> {{recipe.name}}
    </a>
</li>
```

## Componente Detalles

### Creando el componente

Vamos a usar Angular CLI para generar el componente de detalles.

```bash
ng generate component recipe-detail
```

### Escribiendo la plantilla

Vamos a cortar la sección de la receta seleccionada en el archivo `recipes.component.html` y lo pegamos en el archivo `recipe-detail.component.html`.

También vamos a reemplazar la propiedad `selectedRecipe` por `recipe` en el archivo `recipe-detail.component.html`. Lo anterior, debido a que el nuevo componente puede mostrar los detalles de cualquier receta.

El archivo `recipe-detail.component.html` quedará así:

```html
<!-- Sección Recceta Seleccionada -->
<div *ngIf="recipe">
    <h2>{{ recipe.name | uppercase }} Details</h2>
    <div><span>id: </span>{{recipe.id}}</div>
    <div>
      <label>name:
        <input [(ngModel)]="recipe.name" placeholder="name">
      </label>

      <label>description:
          <input [(ngModel)]="recipe.description" placeholder="description">
      </label>
    </div>
</div>
```

El archivo `recipe-detail.component.css` quedará así:

```css
button{
    float: right;
    padding: 10px;
    background-color: #11956c;
    border: none;
    border-radius: 2px;
    margin-top: 4em;
}

button:hover{
    background-color: #DDD;
    color: #11956c;
}

label{
    float: left;
    color: black;
    display: flex;
    justify-content: center;
    margin-top: 2em;
}

input{
    margin-left: 3em;
    box-sizing: border-box;
    padding: 10px 1px;
    border:none;
    outline:none;
    background-color:transparent;
    border-bottom: 2px solid #11956c;
}
```

### Añadir la propiedad @Input() para la receta

La plantilla `RecipeDetailComponent` se une a la propiedad `recipe` del comopente de tipo `Recipe`.

Vamos a modíficar el archivo `recipe-detail.component.ts`

```typescript
import { Recipe } from '../recipe';
```

La propiedad `recipe` debe ser una propiedad de entrada, anotada con el decorador `@Input()` por que el componente externo `RecipesComponent` se enlazará de esta manera.

Añadiremos lo siguiente al archivo `recipes.component.html`

```html
<app-recipe-detail [recipe]="selectedRecipe"></app-recipe-detail>
```

[recipe] = "selectedRecipe" es un enlace de Angular unidireccional desde la propiedad `selectedRecipe` a la propiedad `recipe` del elemento objetivo.

Modificamos el import de `@angular/core` para incluir el simbolo `Input` en el archivo `recipe-detail.component.ts`.

```typescript
import { Component, OnInit, Input } from '@angular/core';
```

Añadimos la propiedad `recipe`, precedida del decorador `@Input()`.

```typescript
@Input() recipe: Recipe;
```

Este componente no tendrá logíca, solo recibirá una receta y la mostrará.

## Servicios

Por el momento estamos obteniendo y mostrando datos falsos.

Después de esta sección, el componente `RecipesComponent` se centrará en apoyar la vista.

### ¿Por Qué Servicios?

Los componentes no deben buscar ni guardad datos directamente y, desde luego, no deberían presentar datos falsos. Deben enfocarse en presentar datos y delegar el acceso a datos a un servicio.

Los servicios son una excelente manera de compartir información entre clases que no se conocen entre sí.

### Creando el Servicio de recetas

Usaremos la herramieta Angular CLI y crearemos un servicio llamado `recipe`

```bash
ng generate service recipe
```

Lo anterior nos genera la clase `RecipeService` en `src/app/recipe.service.ts`

```typescript
import { Injectable } from '@angular/core';

@Injectable()
export class RecipeService {

  constructor() { }

}
```

#### Servicios @Injectable()

Observemos que el nuevo servicio tiene el decorador @Injectable(), esto le dice a Angular que ese servicio podría haber inyectado dependencias.

### Obteniendo datos de recetas

El servicio `RecipeService` podría obtener datos de recetas desde cualquier lugar: un web service, almacenamiento local o una fuente de datos simulada.

Vamos a importar lo siguiente en el archivo `recipe.service.ts`

```typescript
import { Recipe } from './recipe';
import { RECIPES } from './mock-recipes';
```

Añadiremos el método `getRecipes` que retornará el arreglo de recetas.

```typescript
getRecipes(): Recipe[] {
    return RECIPES;
}
```

Se debe proveer el servicio de recetas en el sistema de inyección de dependecias.

Lo anterior se podría hacer de la siguiente manera, siempre y cuando ese servicio no exista.

```bash
ng generate service recipe --module=app
```

Actualmente el servicio ya existe en la aplicación, por lo que se deberá añadir manualmente.

Importamos el servicio en el archivo `app.module.ts`

```typescript
import { RecipeService } from './recipe.service';
```

Añadimos el servicio al arreglo de providers

```typescript
providers: [
	RecipeService,
]
```

Este arreglo de proveedores le dice a Angular que cree una única instancia compartida de `RecipeService` y la inserte en cualquier clase que lo solicite.

### Actualizando RecipesComponent

Vamos a borrar el import del arreglo `RECIPES` en el archivo `recipes.component.ts`. Importamos el servicio en su lugar.

```typescript
import { RecipeService } from '../recipe.service';
```

Reemplazamos la definición de la propiedad `recipes`

```typescript
recipes : Recipe[];
```

#### Inyectar el servicio

Añadimos un parámetro privado de tipo `RecipeService` al constructor.

```typescript
constructor(private recipeService: RecipeService) { }
```

Cuando angular crea el componente de recetas, el sistema de inyección de dependencia establece el parámetro de tipo `RecipeService` a la instancia única de `RecipeService`.

#### Añadiendo el método getRecipes()

Creamos una función que recupera los datos de recetas desde el servicio

```typescript
getRecipes(): void {
    this.recipes = this.recipeService.getRecipes();
}
```

Llamaremos el anterior método en el `ngOnInit()`

```typescript
ngOnInit() {
    this.getRecipes();
}
```

Se llama en el método `ngOnInit()` porque no es buena práctica llamarlo en el constructor. El constructor se debería reservar para inicializaciones simples.

Ahora la aplicación debería estar funcionando correctamente.

### Observable RecipeService

Actualmente el método `RecipeService.getRecipes()` es un método sincrono; Esto no trabaja así en la vida real.

Vamos a hacer qué la obtención de datos sea asincrona y usaremos RxJS para simular que se obtiene datos de un servidor.

Vamos a abrir el archivo `recipe.service.ts` y añadiremos los siguientes imports

```typescript
import { Observable } from 'rxjs/Observable';
import { of } from 'rxjs/observable/of';
```

Reemplazamos el método `getRecipes` por el siguiente:

```typescript
getRecipes(): Observable<Recipe[]> {
    return of(RECIPES);
}
```

Ahora vamos a suscribirnos desde `RecipesComponent`. Abrimos el archivo `recipes.component.ts` y reemplazamos el método `getRecipes` por lo siguiente:

```typescript
getRecipes(): void {
    this.recipeService.getRecipes()
      .subscribe(recipes => this.recipes = recipes);
}
```

El `Observable.subscribe()` hace que la obtención de datos se de de manera asincrona.

## Rutas

En esta  sección añadiremos la vista dashboard y la navegación entre las vistas de la aplicación.

### Añadiendo el modulo AppRouting

Una buena práctica en Agular es cargar y configurar el enrutador de la aplicación en un módulo separado e importarlo en el `AppModule` raíz.

Generearemos el modulo mediante Angular CLI

```bash
ng generate module app-routing --flat --module=app
```

- `--flat` añade el fichero en la ruta `src/app`.
- `--module=app` le dice al CLI que registre el modulo en  el arreglo de imports del `AppModule`.

Generalmente en un modulo de rutas no se declara componentes por lo qué deberíamos borrar el arreglo `declarations` y  el módulo `CommonModule` de los imports en el archivo `app-routing.module.ts`.

Se configurará el enrutador con `Routes` en el módulo `RouterModule` por lo que se deben importar ambos.

En la matriz exports de `@NgModule` vamos a añadir el módulo `RouterModule`. Lo anterior para que pueda ser usado en el `AppModule`.

El fichero debería lucir así:

```typescript
import { NgModule }             from '@angular/core';
import { RouterModule, Routes } from '@angular/router';

@NgModule({
  exports: [ RouterModule ]
})
export class AppRoutingModule {}
```

### Añadiendo rutas

Las rutas le dicen al enrutador qué vista mostrar cuando un usuario hace click en un enlace o pega una URL en la barra de direcciones del navegador.

En el módulo `AppRoutingModule` vamos a importar el componente `RecipesComponent` y añadiremos este a una ruta.

```typescript
import { RecipesComponent }      from './recipes/recipes.component';

const routes: Routes = [
  { path: 'recipes', component: RecipesComponent }
];
```

Vamos a añadir el import de `RouterModule ` en el archivo `app-routing.module.ts`. Quedando así el `@NgModule`.

```typescript
@NgModule({
  imports: [ RouterModule.forRoot(routes) ],
  exports: [ RouterModule ]
})
```

Vamos reemplazar el `<app-recipes>` en el archivo `app.component.html` por `<router-oulet>`.

Ahora si colocamos la ruta http://localhost:4200/recipes en el navegador nos llevara a la lista de recetas.

Vamos a añadir un link que nos lleve a la dirección planteada anteriormente. Este link se añadirá en el archivo `app.component.html`  quedando así.

```html
<h1>{{title}}</h1>
<nav>
  <a routerLink="/recipes">Recipes</a>
</nav>
<router-outlet></router-outlet>
```

El routerLink es el selector de la directiva RouterLink que convierte el click del usuario en una ruta de navegación. Es otra de las directivas públicas en el RouterModule.

Ahora se puede correr de nuevo la aplicación y verificar su funcionamiento.

![routes]({{ "/assets/media/imagenes/recipes/routes.gif" | absolute_url }})

### Añadiendo el componente Dashboard

Vamos a usar el CLI para generar el componente

```bash
ng generate component dashboard
```

Reemplazaremos el contenido por defecto de cada elemento del componente.

- `dashboard.component.html`

```html
<h3>Top Recipes</h3>
<div class="grid grid-pad">
  <a *ngFor="let recipe of recipes" class="col-1-4">
    <div class="module recipe">
      <h4>{{recipe.name}}</h4>
    </div>
  </a>
</div>
```

- `dashboard.component.ts`

```typescript
import { Component, OnInit } from '@angular/core';
import { Recipe } from '../recipe';
import { RecipeService } from '../recipe.service';

@Component({
  selector: 'app-dashboard',
  templateUrl: './dashboard.component.html',
  styleUrls: [ './dashboard.component.css' ]
})
export class DashboardComponent implements OnInit {
  recipes: Recipe[] = [];

  constructor(private recipeService: RecipeService) { }

  ngOnInit() {
    this.getRecipes();
  }

  getRecipes(): void {
    this.recipeService.getRecipes()
      .subscribe(recipes => this.recipes = recipes.slice(1, 3));
  }
}
```

- `dashboard.component.css`

```css
/* DashboardComponent's private CSS styles */
[class*='col-'] {
    float: left;
    padding-right: 20px;
    padding-bottom: 20px;
  }
  [class*='col-']:last-of-type {
    padding-right: 0;
  }
  a {
    text-decoration: none;
  }
  *, *:after, *:before {
    -webkit-box-sizing: border-box;
    -moz-box-sizing: border-box;
    box-sizing: border-box;
  }
  h3 {
    text-align: center; margin-bottom: 0;
  }
  h4 {
    position: relative;
  }
  .grid {
    margin: 0;
  }
  .col-1-4 {
    width: 25%;
  }
  .module {
    margin-left: 2em;
    margin-right: 2em;
    padding: 20px;
    text-align: center;
    color: #eee;
    max-height: 120px;
    min-width: 120px;
    background-color: #11956c;
    border-radius: 2px;
  }
  .module:hover {
    background-color: #EEE;
    cursor: pointer;
    color: #11956c;
  }
  .grid-pad {
    padding: 10px 0;
  }
  .grid-pad > [class*='col-']:last-of-type {
    padding-right: 20px;
  }
  @media (max-width: 600px) {
    .module {
      font-size: 10px;
      max-height: 75px; }
  }
  @media (max-width: 1024px) {
    .grid {
      margin: 0;
    }
    .module {
      min-width: 60px;
    }
  }
```

La plantilla presenta un grid que contiene enlaces de recetas.

En el componete podemos ver que el método `getRecipes` reduce la canditad de recetas a mostrar.

#### Añadiendo la ruta del dashboard

En el módulo `AppRoutingModule` vamos a importar el componente `DashboardComponent`.

```typescript
import { DashboardComponent }   from './dashboard/dashboard.component';
```

Añadiremos una entrad de ruta en el arreglo `routes`.

```typescript
{ path: 'dashboard', component: DashboardComponent },
```

Además añadiremos una ruta por defecto.

```typescript
{ path: '', redirectTo: '/dashboard', pathMatch: 'full' },
```

Añadiremos un enlace hacia el `dashboard` en el template `app.component.html`.

```html
<h1>{{title}}</h1>
<nav>
  <a class="menu-link" routerLink="/dashboard">Dashboard</a>
  <a class="menu-link" routerLink="/recipes">Recipes</a>
</nav>
<router-outlet></router-outlet>
```

Añadiremos estilos a los enlaces; En el archivo `app.component.css` colocaremos lo siguiente:

```css
.menu-link{
    display: flex;
    justify-content: center;
    margin-top: 0.5em;
    margin-bottom: 0.5em;
    width: 100%;
    margin-left: 2em;
    margin-right: 2em;
    text-decoration: none;
    color: white;
    padding-top: 5px;
    padding-bottom: 5px;
    text-align: center;
    background-color: #11956c;
    border-radius: 2px;
}

.menu-link:hover{
    background-color: #DDD;
}
```

En el archivo `recipes.component.css` buscaremos el estilo `.recipes a` y cambiaremos la propiedad `width` por lo siguiente:

```css
width:100%;
```

Quedando así:

![dashboard]({{ "/assets/media/imagenes/recipes/dashboard.PNG" | absolute_url }})

### Navegación hacia la vista de detalles

Primero borraremos la sección de detalles `<app-recipe-detail>` de la vista `recipes.component.html`.

En el modulo `AppRoutingModule` vamos a importar el componente `RecipeDetailComponent`.

```typescript
import { RecipeDetailComponent } from './recipe-detail/recipe-detail.component';
```

y añadiremos una ruta con parametro en el arreglo `routes`.

```typescript
{ path: 'detail/:id', component: RecipeDetailComponent },
```

El arreglo de rutas quedará así:

```typescript
const routes: Routes = [
  { path: '', redirectTo: '/dashboard', pathMatch: 'full' },
  { path: 'dashboard', component: DashboardComponent },
  { path: 'detail/:id', component: RecipeDetailComponent },
  { path: 'recipes', component: RecipesComponent }
];
```

Crearemos los links en el template del dashboard `dashboard.component.html`, quedando así el archivo:

```html
<h3>Top Recipes</h3>
<div class="grid grid-pad">
    <a *ngFor="let recipe of recipes" class="col-1-4"
    routerLink="/detail/{{recipe.id}}">
    <div class="module recipe">
      <h4>{{recipe.name}}</h4>
    </div>
  </a>
</div>
```

También crearemos los links en el template `recipes.component.html`

```html
<h2>My Recipes</h2>
<ul class="recipes">
    <li *ngFor="let recipe of recipes">
      <a routerLink="/detail/{{recipe.id}}">
        <span class="badge">{{recipe.id}}</span> {{recipe.name}}
      </a>
    </li>
</ul>
```

Ahora vamos a remover el método `onSelect()` del componente `RecipesComponent` que ya no es usado.

### Enrutar Compoenten Detalles

Añadiremos los siguientes imports al archivo `recipe-detail.component.ts`.

```typescript
import { ActivatedRoute } from '@angular/router';
import { Location } from '@angular/common';

import { RecipeService }  from '../recipe.service';
```

Inyectaremos los elementos importados en el constructor.

```typescript
constructor(
    private route: ActivatedRoute,
    private recipeService: RecipeService,
    private location: Location
) {}
```

- `ActivateRoute` contiene información sobre la ruta.
- `RecipeService` obtiene los datos de las recetas.
- `Location` es un servicio de angular para interactuar con el navegador.

#### Extraer el Id de la ruta

En el `ngOnInit` llamaremos al método `getRecipe` que definieremos de la siguiente manera.

```typescript
ngOnInit(): void {
    this.getRecipe();
}

getRecipe(): void {
    const id = +this.route.snapshot.paramMap.get('id');
    this.recipeService.getRecipe(id)
      .subscribe(recipe => this.recipe = recipe);
}
```

Añadiremos el método `getRecipe()` en el servicio `RecipeService`.

```typescript
getRecipe(id: number): Observable<Recipe> {
    return of(RECIPES.find(recipe => recipe.id === id));
}
```

Ahora podemos refrescar el navegador y verificar que todo funcione correctamente.

### Añadiendo botón de retorno

Vamos a añadir un botón de retorno para volver a la página anterior.

En el archivo `recipe-detail.component.html` añadiremos un botón.

```html
<button (click)="goBack()">go back</button>
```

En el archivo `recipe-detail.component.ts` crearemos el método `goBack()`.

```typescript
goBack(): void {
  this.location.back();
}
```

#### Resultado

![final]({{ "/assets/media/imagenes/recipes/final.gif" | absolute_url }})

## Fin

Ahora conocemos cuestiones básicas del framework angular. El tutorial se basó en el tour of heroes de angular https://angular.io/tutorial. En este tutorial se pueden profundizar conceptos de servicios, rutas y http services.

Pueden encontrar el código aquí: [count-down](https://github.com/brayvasq/recipes)
