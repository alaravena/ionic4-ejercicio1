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

### 5 Agregar imágenes en la barra de navegación

### 6 Crear una lista

### 7 Utilizar un objeto json para el contenido

### 8 Transformar el header en componente

### 9 Navegar a vista detalle, traspasando información

### 10 Usar componentes ionic para visualización del detalle
