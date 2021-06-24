# Ionic 4 -Ejercicio 1

Paso a paso de la solución del ejercicio de módulo 1 den curso de entrenamiento de ionic 4 Duoc UC 2021

## Comenzando 🚀

Si descargan este repositorio, no olviden de ejecutar _npm install_
Los commit van en el mismo orden que la ejecución del ejercicio

### 1 Crear una app en blanco

```bash
ionic start ionic4-ejercicio1 blank --type=angular --cordova
```

### 2 Crear iconos y splash

Modificar archivos _icon.png_ y _splash.png_ y luego usar

```bash
ionic cordova resources
```

(sirve para comprobar el funcionamiento de cordova)

### 3 Agregar una página de detalle

```bash
ionic generate page pages/detalle
```

Esto creará 6 archivos dentro de app/pages/detalle y además agregará a _app-routing.module.ts_ el path

Aqui como buena práctica podemos trasladar la carpeta de la página _home_, creada por el template 'blank' dentro de la carpeta pages.
En VSC pregunta por las importaciones y las corrige. Si lo hacemos manualmente, debemos cambiar el path del homeen _app-routing.module.ts_, en las importaciones, por:

```ts
    loadChildren: () => import('./pages/home/home.module').then( m => m.HomePageModule)
```

Es un buen punto para probar si todo funciona correctamente con

```bash
ionic serve
```

Podemos revisar la vista detalle usando la ruta: <http://localhost:8100/detalle>

### 4 Transformar título de barra de navegación en variable

Primero utilizaremos el componente para barra de navegación de ionic 4 y lo incertamos en `<ion-header>` en el html del home y de detalle. Luego el título lo dejamos dentro de doble llave para indicar que el valor viene desde la clase que controla la vista:

```html
<ion-toolbar>
  <ion-buttons slot="start">
    <ion-back-button></ion-back-button>
  </ion-buttons>
  <ion-title>
    {{ tituloPagina }}
  </ion-title>
</ion-toolbar>
```

(de paso también borrar el contenido dentro de `<ion-content>`)

Ahora en el archivo controlador, _home.page.ts_ y _detalle.page.ts_ agregamos la variable **tituloPagina** justo antes del constructor.

### 5 Agregar imágenes en la barra de navegación

Agregamos un logo a _/src/assets_

Y la barra de título nos queda ahora:

```html
<ion-toolbar>
  <ion-buttons slot="start">
    <ion-back-button></ion-back-button>
  </ion-buttons>
  <ion-title>
    {{ tituloPagina }}
  </ion-title>
  <img id="logo-titulo" src="./assets/logo.png" />
</ion-toolbar>
```

En el caso de la imagen de mi ejemplo, esta muy grande, por lo que agregamos al scss (en el del home podemos borrar todo el resto que viene de ejemplo):

```css
#logo-titulo {
    width: 10em;
    position: absolute;
    right: 1em;
    top: 0.4em;
}
```

### 6 Crear una lista

Se utiliza el componente `<ion-list>` y se agrega al html del home.

```html
<ion-list>
    <ion-item>
        <ion-avatar slot="start">
            <img src='...'>
        </ion-avatar>
        <ion-label>
            <h2>titulo</h2>
            <h3>subtitulo</h3>
        </ion-label>
        <ion-icon class="ff" name="arrow-forward"></ion-icon>
    </ion-item>
</ion-list>
```

### 7 Utilizar un objeto json para el contenido de la lista

Utilizamos en json que esta en el drive del curso y lo agregamos al controlador del home antes del constructor.

```ts
listaPersonajes: any = [{'AQUI EL JSON'}
```

En el html agregamos las directivas estrucurales ngIf para evitar errores si no existe el json y ngFor para el loop que repetirá el item de la lista:

```html
<ion-content>
  <ion-list *ngIf="listaPersonajes">
    <ion-item *ngFor='let personaje of listaPersonajes'>
      <ion-avatar slot="start">
        <img [src]='personaje.image'>
      </ion-avatar>
      <ion-label>
          <h2>{{ personaje.name }}</h2>
          <h3>{{ personaje.homeworld }}</h3>
      </ion-label>
      <ion-icon class="ff" name="arrow-forward"></ion-icon>
    </ion-item>
  </ion-list>
</ion-content>
```

### 8 Transformar el header en componente

### 9 Navegar a vista detalle, traspasando información

### 10 Usar componentes ionic para visualización del detalle
