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

Primero creamos un modulo para los componentes

```bash
ionic generate module components
```

Automáticamente nos crea la carpeta components
Ahora creamos el componente:

```bash
ionic generate component components/encabezado
```

Que nos crea 3 archivos. Trasladamos el html del encabezado que tenemos en home al html del encabezdo componente.
Para que siga funcionando el título dinámico, debemos agregar al typescript del componente, justo debajo del inicio de la clase, una variable que se avisa que el valor debe ser entregado al momento de usar el compoenente:

```ts
@Input() pageTitle;
```

Se debe importar el decorador Input desde **'@angular/core'**

El html debe usarse en el titulo `{{pageTitle}}`. No usamos el mismo nombre de variable que teníamos originalmente porque, como veremos mas adelante, la variable de la página le dará el valor al título a traves de una directiva.

El código css que teníamos en _home.page.scss_ lo trasladamos al css del componente

Ahora se debe configurar el archivo _components/components.module.ts_ para que funcionen correctamente los componentes que usen componentes de ionic (esto se hace solo una vez), asi como también disponibilizar los componentes para el resto de la aplicación (este se hace por cada componente). El modulo quedaría asi:

```ts
import { NgModule } from '@angular/core';
import { CommonModule } from '@angular/common';
import { IonicModule }  from '@ionic/angular';

import { EncabezadoComponent } from './encabezado/encabezado.component';

@NgModule({
  declarations: [
    EncabezadoComponent,
    //aqui declaramos todos los componentes
  ],
  imports: [
    CommonModule,
    IonicModule
  ],
  exports: [
    EncabezadoComponent,
    //aqui exportamos todos los componentes
  ]
})
export class ComponentsModule { }
```

Debemos importar este modulo en cada una de las páginas donde queramos usar los componentes, importándolo en el archivo de _home.module.ts_ y _detalle.module.ts_

```ts
import { ComponentsModule } from './../../components/components.module';

@NgModule({
  imports: [
    //...//
    ComponentsModule,
  ],

```

Ahora esta listo para usarlo en el html tanto del home como del detalle, usando el selector del componente (definido en el controlador del componente). Como hemos definido el titulo como variable, en el html del las páginas donde lo usamos, se define a traves de directiva:

```html
<ion-header>
  <app-encabezado [pageTitle]="tituloPagina" ></app-encabezado>
</ion-header>
```

### 9 Navegar a vista detalle, traspasando información

Como debemos traspasar un objeto y no solo un valor, conviene manejarlo desde el controlador, para eso crearemos una método dentro de la clase que maneja la vista **home** y será activada desde la flecha, en el html, pasando el objeto completo:

```html
<ion-icon class="ff" name="arrow-forward" (click)="gotoDetalles(personaje)"></ion-icon>
```

Agregamos entonces la función dentro de la clase en el _home.page.ts_:

```ts
import { NavController } from '@ionic/angular';
import { NavigationExtras } from '@angular/router';

//...
//Dentro de la clase, debajo de las variables que tenemos declaradas como parámetros
constructor(public navCtrl: NavController) {}

  gotoDetalles(personaje: any) {
    const navigationExtras: NavigationExtras = {
      queryParams: {
          personaje: JSON.stringify(personaje)
      }
    };
  this.navCtrl.navigateForward(['detalle/'], navigationExtras);
  };
```

Ahora en _detalle.page.ts_ debemos recibir esos datos:

```ts
import { ActivatedRoute } from '@angular/router';

//...
export class DetallePage implements OnInit {

  tituloPagina = 'Detalle';
  personaje = null;

  constructor(private activatedRoute: ActivatedRoute) {
    this.activatedRoute.queryParams.subscribe(params => {
        this.personaje = JSON.parse(params.personaje);
    });
  }

  ngOnInit() {
      //Esto nos sirve para revisar en la consola si efectivamente llegó el objeto
    console.log(this.personaje);
  }
```

### 10 Usar componentes ionic para visualización del detalle
