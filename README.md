# Operació NordTec
**Daryl Hernández ASIX1B**

## :pushpin: Índice
- [Operació NordTec](#operació-nordtec)
  - [:pushpin: Índice](#pushpin-índice)
  - [:traffic\_light: Estado del proyecto](#traffic_light-estado-del-proyecto)
  - [:hammer\_and\_wrench: Arquitectura](#hammer_and_wrench-arquitectura)
    - [Estructura de la página (HTML)](#estructura-de-la-página-html)
    - [Contenido de cada sección](#contenido-de-cada-sección)
  - [:book: Explicación del HTML](#book-explicación-del-html)
    - [1. Anatomía de una etiqueta](#1-anatomía-de-una-etiqueta)
    - [2. Estructura básica de un documento](#2-estructura-básica-de-un-documento)
    - [3. El `<head>`](#3-el-head)
    - [4. Comentarios](#4-comentarios)
    - [5. Estructura semántica de la página](#5-estructura-semántica-de-la-página)
    - [6. Textos](#6-textos)
    - [7. Enlaces](#7-enlaces)
    - [8. Imágenes](#8-imágenes)
    - [9. Listas](#9-listas)
    - [10. Tablas](#10-tablas)
    - [11. Formularios](#11-formularios)
    - [12. Atributos `class` e `id`](#12-atributos-class-e-id)
    - [13. Caracteres especiales](#13-caracteres-especiales)
    - [14. Anidamiento](#14-anidamiento)
  - [:gear: Configuraciones](#gear-configuraciones)
  - [:construction: Incidencias y soluciones](#construction-incidencias-y-soluciones)
  - [:bulb: Decisiones técnicas](#bulb-decisiones-técnicas)
  - [:memo: Reflexiones técnicas](#memo-reflexiones-técnicas)
  - [:computer: Tabla de comandos Git](#computer-tabla-de-comandos-git)
  - [:file\_folder: Guardar en GitHub](#file_folder-guardar-en-github)

## :traffic_light: Estado del proyecto
- :white_check_mark: Fase 1: Estructura HTML5 semántica (header, nav, main, aside, footer)
- :white_check_mark: Fase 2: Hoja de estilos CSS externa (selectores, modelo de caja, flexbox, grid)
- :arrows_counterclockwise: Fase 3: Revisión, limpieza del código y documentación

## :hammer_and_wrench: Arquitectura
```text
LDM_html/
├── ejercicio1.html
├── css/
│   └── style.css
├── img/
│   └── gta.jpg
└── README.md
```

### Estructura de la página (HTML)
| Etiqueta | Para qué la uso |
| --- | --- |
| `<header>` | Cabecera con el título y el subtítulo |
| `<nav>` | Menú con enlaces internos (`href="#id"`) a cada sección |
| `<main>` | Contenido principal de la página |
| `<section>` | Cada bloque temático (colores, selectores, caja, tabla, formulario) |
| `<aside>` | Barra lateral con contenido relacionado |
| `<article>` | Tarjeta independiente dentro del aside |
| `<footer>` | Pie de página con copyright y enlace "Volver arriba" |
| `<table>` | Tabla con `<caption>`, `<thead>`, `<tbody>` y `colspan` |
| `<form>` | Formulario con `input`, `select`, `textarea` y `label` |

### Contenido de cada sección
1. **Colores**: nombre, hexadecimal, RGB, RGBA, HSL y degradado.
2. **Selectores**: etiqueta, clase, id, descendiente, hijo directo, atributo, pseudoclases y pseudoelementos.
3. **Modelo de caja**: `padding`, `border`, `margin`, `display`, `position` y flexbox.
4. **Tabla**: horario con filas alternas y `hover`.
5. **Formulario**: campos con estilo y `:focus`.

## :book: Explicación del HTML

HTML (*HyperText Markup Language*) es un lenguaje de **marcas**: se usan etiquetas para indicar **qué es** cada parte del contenido (un título, un párrafo, un enlace...). No dice cómo se ve; de eso se encarga el CSS.

### 1. Anatomía de una etiqueta
```html
<a href="https://www.w3.org" target="_blank">Enlace</a>
```
| Parte | Ejemplo | Significado |
| --- | --- | --- |
| Etiqueta de apertura | `<a>` | Dónde empieza el elemento |
| Atributos | `href="..."`, `target="_blank"` | Información extra (`nombre="valor"`) |
| Contenido | `Enlace` | Lo que se ve en la página |
| Etiqueta de cierre | `</a>` | Dónde termina el elemento |

Algunas etiquetas **no tienen cierre** porque no contienen nada: `<img>`, `<meta>`, `<link>`, `<input>`, `<br>`.

### 2. Estructura básica de un documento
```html
<!DOCTYPE html>
<html lang="es">
<head>
  <!-- información que NO se ve en la página -->
</head>
<body>
  <!-- todo lo que SÍ se ve en la página -->
</body>
</html>
```
- `<!DOCTYPE html>`: indica que el documento es HTML5.
- `<html lang="es">`: raíz del documento; `lang` indica el idioma (útil para navegadores y lectores de pantalla).
- `<head>`: información sobre la página (título, codificación, estilos).
- `<body>`: el contenido visible.

### 3. El `<head>`
| Etiqueta | Para qué sirve |
| --- | --- |
| `<meta charset="UTF-8">` | Permite escribir tildes y la ñ sin que salgan caracteres raros |
| `<meta name="viewport" ...>` | Hace que la página se adapte al ancho del móvil |
| `<title>` | Texto que aparece en la pestaña del navegador |
| `<link rel="stylesheet" href="css/style.css">` | Conecta el archivo CSS externo |

### 4. Comentarios
```html
<!-- Esto no se muestra en la página -->
```
Sirven para explicar el código y dividirlo en bloques (por ejemplo `<!-- CABECERA -->`).

### 5. Estructura semántica de la página
Las etiquetas semánticas indican **el significado** de cada zona, y no solo su posición:

| Etiqueta | Zona |
| --- | --- |
| `<header>` | Cabecera con título y subtítulo |
| `<nav>` | Menú de navegación |
| `<main>` | Contenido principal (solo uno por página) |
| `<section>` | Bloque temático con su propio título |
| `<aside>` | Contenido secundario (barra lateral) |
| `<article>` | Contenido independiente (la tarjeta) |
| `<footer>` | Pie de página |
| `<div>` | Caja genérica sin significado, para agrupar |

### 6. Textos
```html
<h1>Título principal</h1>
<h2>Subtítulo</h2>
<p>Un párrafo con <b>negrita</b> y <code>código</code>.</p>
```
- `<h1>` a `<h6>`: títulos por nivel de importancia. Lo normal es **un solo `<h1>`** y no saltarse niveles.
- `<p>`: párrafo.
- `<b>`: negrita. `<code>`: fragmento de código.
- `<br>`: salto de línea.
- `<span>`: etiqueta en línea sin significado, para dar estilo a un trozo de texto.

### 7. Enlaces
```html
<a href="https://www.w3.org" target="_blank">Enlace externo</a>
<a href="#tabla">Enlace interno</a>
```
- Enlace **externo**: `href` con la dirección completa. `target="_blank"` lo abre en otra pestaña.
- Enlace **interno**: `href="#id"` salta al elemento que tenga ese `id`. Así funciona el menú de la página (`<a href="#colores">` lleva a `<section id="colores">`).

### 8. Imágenes
```html
<img src="img/gta.jpg" alt="Descripción de la imagen" width="300">
```
- `src`: ruta de la imagen.
- `alt`: texto alternativo. Se lee si la imagen no carga y lo usan los lectores de pantalla. **Es obligatorio.**
- `width`: ancho en píxeles.

### 9. Listas
```html
<ul>
  <li>Elemento 1</li>
  <li>Elemento 2</li>
</ul>
```
- `<ul>`: lista sin orden (viñetas). `<ol>`: lista numerada.
- `<li>`: cada elemento de la lista.
- El menú de la página es una lista de enlaces dentro de un `<nav>`, y el CSS le quita las viñetas y la pone en horizontal.

### 10. Tablas
```html
<table>
  <caption>Horario de ejemplo</caption>
  <thead>
    <tr><th>Hora</th><th>Lunes</th></tr>
  </thead>
  <tbody>
    <tr><td>8:00</td><td>Lenguaje de marcas</td></tr>
    <tr><td colspan="2">Patio</td></tr>
  </tbody>
</table>
```
| Etiqueta | Significado |
| --- | --- |
| `<table>` | La tabla entera |
| `<caption>` | Título de la tabla |
| `<thead>` / `<tbody>` | Cabecera y cuerpo |
| `<tr>` | Una fila |
| `<th>` | Celda de cabecera |
| `<td>` | Celda de datos |
| `colspan="3"` | La celda ocupa varias columnas |

### 11. Formularios
```html
<form action="#" method="get">
  <label for="nombre">Nombre</label>
  <input type="text" id="nombre" name="nombre" placeholder="Escribe tu nombre" required>
  <button type="submit">Enviar</button>
</form>
```
- `<form>`: contenedor. `action` indica a dónde se envían los datos y `method` cómo (`get` o `post`).
- `<label for="nombre">`: etiqueta del campo. Su `for` debe coincidir con el `id` del campo, así al hacer clic en el texto se selecciona el campo.
- `<input type="...">`: campo de entrada. Según el `type` cambia su comportamiento (`text`, `email`...). `email` comprueba que tenga formato de correo.
- `<select>` + `<option>`: lista desplegable.
- `<textarea rows="4">`: texto de varias líneas.
- `placeholder`: texto de ayuda dentro del campo. `required`: campo obligatorio.
- `name`: nombre con el que viaja el dato al enviarlo.

### 12. Atributos `class` e `id`
```html
<p class="destacado">Se puede repetir en varios elementos</p>
<p id="parrafo-unico">Solo puede haber uno con este id</p>
```
Son el puente con el CSS: en el CSS se usa `.destacado` para una clase y `#parrafo-unico` para un id.

### 13. Caracteres especiales
Algunos símbolos tienen significado en HTML, así que se escriben con entidades:

| Entidad | Resultado |
| --- | --- |
| `&gt;` | `>` |
| `&lt;` | `<` |
| `&copy;` | © |

### 14. Anidamiento
Las etiquetas se abren y se cierran en orden, como cajas dentro de cajas:
```html
<!-- Correcto -->
<p>Texto con <b>negrita</b></p>

<!-- Incorrecto: se cierran en orden cruzado -->
<p>Texto con <b>negrita</p></b>
```
Indentar el código (con espacios) ayuda a ver qué está dentro de qué.

## :gear: Configuraciones
1. Crear la carpeta del proyecto y abrirla en VS Code.
2. Crear `ejercicio1.html` y las carpetas `css/` e `img/`.
3. Enlazar la hoja de estilos dentro del `<head>`:
   ```html
   <link rel="stylesheet" href="css/style.css">
   ```
4. Declarar el idioma y la codificación para evitar problemas con tildes:
   ```html
   <html lang="es">
   <meta charset="UTF-8">
   <meta name="viewport" content="width=device-width, initial-scale=1.0">
   ```
5. Visualizar la página con la extensión **Live Server** (clic derecho en `ejercicio1.html` → *Open with Live Server*).

## :construction: Incidencias y soluciones
| Mensaje de error :bangbang: | Cuándo :question: | Causa :mag_right: | Solución :o: | Detectada :detective: |
| --- | --- | --- | --- | --- |
| La página se ve sin estilos | Al abrir `ejercicio1.html` | La ruta de `href` no coincide con la ubicación real del CSS | Comprobar que la ruta es `css/style.css` | 24/11/2026 |
| La imagen no aparece (se ve el texto `alt`) | Al cargar la página | Ruta o nombre del archivo incorrecto (`img/gta.jpg`) | Revisar mayúsculas, extensión y carpeta | 24/11/2026 |
| Caracteres raros (`Ã³`) | Al abrir la página | Falta `<meta charset="UTF-8">` o el archivo no está guardado en UTF-8 | Añadir el meta y guardar en UTF-8 | 24/11/2026 |

> [!NOTE]
> Rellena esta tabla con los errores que realmente tuviste. Las filas de arriba son ejemplos de errores típicos.

> [!WARNING]
> En el CSS, el comentario del principio dice `href="estilos.css"`, pero el HTML enlaza `css/style.css`. Conviene corregir el comentario para que no confunda.

## :bulb: Decisiones técnicas
- **HTML semántico** (`header`, `nav`, `main`, `aside`, `footer`) en lugar de usar solo `div`, para dar significado a cada zona y mejorar la accesibilidad.
- **CSS en un archivo externo**, separando contenido (HTML) y presentación (CSS), lo que facilita el mantenimiento.
- **Variables CSS** (`:root` con `--color-principal`, etc.): cambiando un color en un solo sitio se actualiza toda la página.
- **CSS Grid** (`3fr 1fr`) para la estructura principal y **Flexbox** para el menú y las demos.
- **Media query** (`max-width: 768px`) para que la página se adapte a móviles.
- **Reset con `*`** (`margin: 0; padding: 0; box-sizing: border-box`) para partir de una base uniforme.
- **Clases en vez de `id`** para dar estilo, ya que se pueden reutilizar y tienen menos especificidad.

## :memo: Reflexiones técnicas

**Lo que he aprendido**
- Vengo de SMIX, donde vimos sobre todo hardware y redes, así que programar era nuevo para mí.
- HTML no es un lenguaje de programación, sino de **marcas**: indica qué es cada cosa (título, párrafo, menú).
- HTML y CSS van separados: el HTML pone la estructura y el CSS el aspecto. Cambié un color en el `:root` y se actualizó toda la página.
- `class` se puede repetir, `id` es único. Y las rutas importan: si fallas una letra, el navegador no avisa.

**Lo que más me ha costado**
- El modelo de caja (`padding`, `border`, `margin`).
- `position: absolute` respecto al padre con `position: relative`.
- Saber cuándo usar Grid y cuándo Flexbox.
- Cerrar bien las etiquetas e indentar el código.

**Qué mejoraría**
- Pasar los estilos en línea (`style="..."`) al CSS.
- Validar el código en el [validador de W3C](https://validator.w3.org/).
- Rehacer la página desde cero, sin copiar, para comprobar que lo he entendido.

## :computer: Tabla de comandos Git
| Comando | ¿Qué hace? |
| --- | --- |
| `git --version` | Muestra la versión de Git instalada |
| `git init` | Crea un repositorio Git vacío |
| `git clone <url>` | Copia un repositorio remoto en tu equipo |
| `git status` | Muestra el estado actual del repositorio |
| `git add <archivo>` / `git add .` | Prepara los cambios para guardarlos |
| `git commit -m "mensaje"` | Guarda los cambios preparados con un mensaje |
| `git push` | Sube los cambios a GitHub |
| `git pull` | Descarga los cambios más recientes |
| `git log --oneline` | Muestra el historial resumido de commits |

## :file_folder: Guardar en GitHub
1. Guardar el archivo: `Ctrl + S`
2. Preparar los cambios: `git add README.md` (o `git add .` para todo)
3. Guardar la versión: `git commit -m "Describe lo que has cambiado"`
4. Subir a GitHub: `git push` (la primera vez: `git push -u origin main`)