# Operació NordTec
**Daryl Hernández ASIX1B**

## :pushpin: Índex
- [Operació NordTec](#operació-nordtec)
  - [:pushpin: Índex](#pushpin-índex)
  - [**:traffic\_light: Estado del proyecto**](#traffic_light-estado-del-proyecto)
  - [**:hammer\_and\_wrench: Arquitectura**](#hammer_and_wrench-arquitectura)
  - [**:gear: Configuraciones**](#gear-configuraciones)
  - [**:construction: Incidéncias y soluciones**](#construction-incidéncias-y-soluciones)
  - [**:bulb: Decisiones técnicas**](#bulb-decisiones-técnicas)
  - [**:memo: Reflexiones técnicas**](#memo-reflexiones-técnicas)
  - [**:computer: Tabla de comandos Git**](#computer-tabla-de-comandos-git)
  - [**:file\_folder: Guardar en Github**](#file_folder-guardar-en-github)
  - [:traffic\_light: Estado del proyecto](#traffic_light-estado-del-proyecto-1)
  - [:hammer\_and\_wrench: Arquitectura](#hammer_and_wrench-arquitectura-1)
    - [Estructura de la página (HTML)](#estructura-de-la-página-html)
    - [Contenido de cada sección](#contenido-de-cada-sección)
  - [:gear: Configuraciones](#gear-configuraciones-1)
  - [:construction: Incidencias y soluciones](#construction-incidencias-y-soluciones)
  - [:bulb: Decisiones técnicas](#bulb-decisiones-técnicas-1)
  - [:memo: Reflexiones técnicas](#memo-reflexiones-técnicas-1)

## **:traffic_light: Estado del proyecto**
- :white_check_mark: Fase 1 del proyecto
- :arrows_counterclockwise: Fase 2 del proyecto
## **:hammer_and_wrench: Arquitectura**

## **:gear: Configuraciones**

## **:construction: Incidéncias y soluciones**
| Mensaje de error :bangbang: | Cuando :question: | Causa :mag_right: | Solución :o: | Detactada :detective: |
| ---------------- | ------ | ----- | -------- | --------- |
| . | . | . | . | . |

> [!TIP]
> Bien

> [!CAUTION]
> Revisar

> [!WARNING]
> Urgente
## **:bulb: Decisiones técnicas**

## **:memo: Reflexiones técnicas**

## **:computer: Tabla de comandos Git**
| Comandos  | Que hacen? |
| ------------- | ------------- |
| git init  | Crea un repositorio en Git vacío  |
| git add  | Prepara tus cambios para ser gurdados  |
| git status  | Muestra el estado actual de tu repositorio  |
| git --version  | Muestra la version de tu Git  |
| git pull   | Descarga los cambios más recientes  |

## **:file_folder: Guardar en Github**
1. Guardar archivo ctrl + S
2. git add (Nombre del archivo) o (.)
3. git commit -m "mensaje del que es"
4. git push -u (Pasarlo al Github)

## :traffic_light: Estado del proyecto
- :white_check_mark: Fase 1: Estructura HTML5 semántica (header, nav, main, aside, footer)
- :white_check_mark: Fase 2: Hoja de estilos CSS externa (selectores, modelo de caja, flexbox, grid)
- :arrows_counterclockwise: Fase 3: Revisión, limpieza del código y documentación

## :hammer_and_wrench: Arquitectura
```text
proyecto/
├── index.html
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

## :gear: Configuraciones
1. Crear la carpeta del proyecto y abrirla en VS Code.
2. Crear `index.html` y las carpetas `css/` e `img/`.
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
5. Visualizar la página con la extensión **Live Server** (clic derecho en `index.html` → *Open with Live Server*).

## :construction: Incidencias y soluciones
| Mensaje de error :bangbang: | Cuándo :question: | Causa :mag_right: | Solución :o: | Detectada :detective: |
| --- | --- | --- | --- | --- |
| La página se ve sin estilos | Al abrir `index.html` | La ruta de `href` no coincide con la ubicación real del CSS | Comprobar que la ruta es `css/style.css` | 24/11/2026 |
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
- Lo que he aprendido: (por ejemplo, la diferencia entre `class` e `id`, o cómo funciona el modelo de caja).
- Lo que más me ha costado: (por ejemplo, entender `position: absolute` respecto al padre).
- Qué mejoraría: pasar los estilos en línea (`style="..."`) de la imagen y el párrafo al CSS, y validar el código en el [validador de W3C](https://validator.w3.org/).