# Operació NordTec
**Daryl Hernández ASIX1B**

## :pushpin: Índice
- [Estado del proyecto](#traffic_light-estado-del-proyecto)
- [RA1: Firewall y red en Linux](#ra1-firewall-y-red-en-linux)
- [RA2: HTML y CSS](#ra2-html-y-css)
- [Incidencias y soluciones](#construction-incidencias-y-soluciones)
- [Decisiones técnicas](#bulb-decisiones-técnicas)
- [Reflexiones técnicas](#memo-reflexiones-técnicas)
- [Tabla de comandos Git](#computer-tabla-de-comandos-git)
- [Guardar en GitHub](#file_folder-guardar-en-github)

## :traffic_light: Estado del proyecto
| RA | Contenido | Estado |
| --- | --- | --- |
| RA1 | Laboratorio firewall: red con netplan y reglas iptables | :white_check_mark: Hecho |
| RA2 | Página HTML + CSS | :white_check_mark: Hecho |
| Documentación | README, revisión y limpieza | :arrows_counterclockwise: En curso |

---

## RA1: Firewall y red en Linux

Laboratorio `Lab_0373_firewall`: una máquina Linux con **cinco tarjetas de red** que actúa como **router y firewall** entre Internet y tres redes internas.

### Arquitectura de red
```text
Internet
   │  enp1s0 (DHCP)
┌──┴────────────────────┐
│   lab_0373_firewall   │
└─┬───────┬───────────┬─┘
  │       │           │
enp3s0  enp4s0      enp5s0
10.0.0.1/24   192.168.10.1/24   192.168.20.1/24
 (kali)         (dmz)            (lan)
```

| Interfaz | Red | IP del firewall | Configuración |
| --- | --- | --- | --- |
| `enp1s0` | Salida a Internet | Por DHCP | `dhcp4: true` |
| `enp2s0` | Sin comentario en el archivo | Por DHCP | `dhcp4: true` |
| `enp3s0` | Kali | `10.0.0.1/24` | IP indicada en el archivo |
| `enp4s0` | DMZ (servidor web) | `192.168.10.1/24` | `dhcp4: false` + IP fija |
| `enp5s0` | LAN | `192.168.20.1/24` | `dhcp4: false` + IP fija |

### Configuración de red (netplan)
Archivo editado con `nano`: `50-cloud-init.yaml` (en `/etc/netplan/`).

```yaml
network:
    version: 2
    ethernets:
        enp1s0:
            dhcp4: true
        enp3s0:            # kali
            dhcp4: true
            addresses:
                - 10.0.0.1/24
        enp4s0:            # dmz
            dhcp4: false
            addresses:
                - 192.168.10.1/24
        enp5s0:            # lan
            dhcp4: false
            addresses:
                - 192.168.20.1/24
```
- `network` / `version: 2`: formato de netplan.
- `ethernets`: lista de tarjetas de red, cada una con su nombre.
- `dhcp4: true`: la IP la da un servidor DHCP. `false`: se configura a mano.
- `addresses`: IP fija con su máscara (`/24` = `255.255.255.0`).
- Los `#` son comentarios. Los que hay en el archivo indican la zona de cada red (kali, dmz, lan).
- **YAML** se indenta con espacios (nunca tabuladores) y un espacio mal puesto rompe el archivo.
- El aviso del principio dice que cloud-init genera este archivo y que los cambios pueden no persistir. Para evitarlo se crea `/etc/cloud/cloud.cfg.d/99-disable-network-config.cfg` con `network: {config: disabled}`.

Para aplicar los cambios:
```bash
sudo netplan apply
```

### Script del firewall (`firewall.sh`)
- Se coloca en la máquina `lab_0373_firewall`, en `/home/isard`.
- Se ejecuta con: `sudo ./firewall.sh` (si da "Permission denied": `chmod +x firewall.sh`).

```bash
#!/bin/bash

# netejem taules
iptables -F
iptables -t nat -F
iptables -X

# política per defecte
iptables -P INPUT ACCEPT
iptables -P FORWARD ACCEPT
iptables -P OUTPUT ACCEPT

# sortir a internet
iptables -t nat -A POSTROUTING -s 192.168.10.0/24 -o enp1s0 -j MASQUERADE
iptables -t nat -A POSTROUTING -s 192.168.20.0/24 -o enp1s0 -j MASQUERADE
iptables -t nat -A POSTROUTING -s 10.0.0.0/24 -o enp1s0 -j MASQUERADE

# publicar el servidor web (DNAT)
iptables -t nat -A PREROUTING -i enp3s0 -p tcp --dport 80 -j DNAT --to-destination <IP del servidor web>
iptables -t nat -A PREROUTING -i enp3s0 -p tcp -d 10.0.0.1 --dport 80 -j DNAT ...
```
> [!NOTE]
> Las dos últimas líneas se cortan en la captura. Copia aquí tu versión completa del script.

**Qué hace cada bloque**

| Bloque | Comando | Qué hace |
| --- | --- | --- |
| Limpiar | `iptables -F` | Borra todas las reglas de la tabla `filter` |
| | `iptables -t nat -F` | Borra las reglas de la tabla `nat` |
| | `iptables -X` | Borra las cadenas creadas por el usuario |
| Políticas | `-P INPUT/FORWARD/OUTPUT ACCEPT` | Por defecto se **permite todo** el tráfico entrante, reenviado y saliente |
| Salir a Internet | `-t nat -A POSTROUTING ... -j MASQUERADE` | Las redes internas salen a Internet por `enp1s0` con la IP del firewall |
| Publicar web | `-t nat -A PREROUTING ... -j DNAT` | Las peticiones al puerto 80 que llegan al firewall se redirigen al servidor web |

**Conceptos clave**
| Concepto | Significado |
| --- | --- |
| `-A` | Añadir una regla (*append*) |
| `-t nat` | Usar la tabla de NAT |
| `-s` / `-d` | Origen (*source*) / destino (*destination*) |
| `-i` / `-o` | Interfaz de entrada / de salida |
| `-p tcp --dport 80` | Protocolo TCP, puerto de destino 80 (HTTP) |
| `-j` | Qué hacer con el paquete (*jump*): `ACCEPT`, `MASQUERADE`, `DNAT`... |
| **SNAT / MASQUERADE** | Cambia la IP de origen para que las redes privadas salgan a Internet |
| **DNAT** | Cambia la IP de destino para publicar un servidor interno |

> [!WARNING]
> Con las tres políticas en `ACCEPT` el firewall todavía no filtra nada: solo hace NAT. Para que proteja, habría que cambiar `FORWARD` a `DROP` y permitir solo el tráfico necesario.

> [!NOTE]
> Para que la máquina reenvíe tráfico entre redes debe estar activado el reenvío de IP (`net.ipv4.ip_forward=1`). Comprueba que lo tienes activado.

---

## RA2: HTML y CSS

### Arquitectura
```text
LDM_html/
├── ejercicio1.html
├── css/
│   └── style.css
├── img/
│   └── gta.jpg
└── README.md
```

#### Estructura de la página
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

#### Contenido de cada sección
1. **Colores**: nombre, hexadecimal, RGB, RGBA, HSL y degradado.
2. **Selectores**: etiqueta, clase, id, descendiente, hijo directo, atributo, pseudoclases y pseudoelementos.
3. **Modelo de caja**: `padding`, `border`, `margin`, `display`, `position` y flexbox.
4. **Tabla**: horario con filas alternas y `hover`.
5. **Formulario**: campos con estilo y `:focus`.

### Explicación del HTML

HTML (*HyperText Markup Language*) es un lenguaje de **marcas**: se usan etiquetas para indicar **qué es** cada parte del contenido (un título, un párrafo, un enlace...). No dice cómo se ve; de eso se encarga el CSS.

#### 1. Anatomía de una etiqueta
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

#### 2. Estructura básica de un documento
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
- `<html lang="es">`: raíz del documento; `lang` indica el idioma.
- `<head>`: información sobre la página (título, codificación, estilos).
- `<body>`: el contenido visible.

#### 3. El `<head>`
| Etiqueta | Para qué sirve |
| --- | --- |
| `<meta charset="UTF-8">` | Permite escribir tildes y la ñ sin que salgan caracteres raros |
| `<meta name="viewport" ...>` | Hace que la página se adapte al ancho del móvil |
| `<title>` | Texto que aparece en la pestaña del navegador |
| `<link rel="stylesheet" href="css/style.css">` | Conecta el archivo CSS externo |

#### 4. Comentarios
```html
<!-- Esto no se muestra en la página -->
```
Sirven para explicar el código y dividirlo en bloques.

#### 5. Estructura semántica de la página
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

#### 6. Textos
```html
<h1>Título principal</h1>
<h2>Subtítulo</h2>
<p>Un párrafo con <b>negrita</b> y <code>código</code>.</p>
```
- `<h1>` a `<h6>`: títulos por nivel de importancia. Lo normal es **un solo `<h1>`**.
- `<p>`: párrafo. `<b>`: negrita. `<code>`: código. `<br>`: salto de línea.
- `<span>`: etiqueta en línea sin significado, para dar estilo a un trozo de texto.

#### 7. Enlaces
```html
<a href="https://www.w3.org" target="_blank">Enlace externo</a>
<a href="#tabla">Enlace interno</a>
```
- **Externo**: `href` con la dirección completa. `target="_blank"` lo abre en otra pestaña.
- **Interno**: `href="#id"` salta al elemento que tenga ese `id`.

#### 8. Imágenes
```html
<img src="img/gta.jpg" alt="Descripción de la imagen" width="300">
```
- `src`: ruta de la imagen.
- `alt`: texto alternativo, para cuando la imagen no carga y para lectores de pantalla.
- `width`: ancho en píxeles.

#### 9. Listas
```html
<ul>
  <li>Elemento 1</li>
  <li>Elemento 2</li>
</ul>
```
- `<ul>`: lista con viñetas. `<ol>`: lista numerada. `<li>`: cada elemento.
- El menú de la página es una lista de enlaces dentro de un `<nav>`.

#### 10. Tablas
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
| `<th>` / `<td>` | Celda de cabecera / de datos |
| `colspan` | La celda ocupa varias columnas |

#### 11. Formularios
```html
<form action="#" method="get">
  <label for="nombre">Nombre</label>
  <input type="text" id="nombre" name="nombre" placeholder="Escribe tu nombre" required>
  <button type="submit">Enviar</button>
</form>
```
- `<form>`: contenedor. `action` indica a dónde se envían los datos y `method` cómo (`get` o `post`).
- `<label for="nombre">`: su `for` debe coincidir con el `id` del campo.
- `<input type="...">`: campo de entrada (`text`, `email`...).
- `<select>` + `<option>`: lista desplegable. `<textarea>`: texto de varias líneas.
- `placeholder`: texto de ayuda. `required`: campo obligatorio.

#### 12. Atributos `class` e `id`
```html
<p class="destacado">Se puede repetir en varios elementos</p>
<p id="parrafo-unico">Solo puede haber uno con este id</p>
```
Son el puente con el CSS: `.destacado` para una clase y `#parrafo-unico` para un id.

#### 13. Caracteres especiales
| Entidad | Resultado |
| --- | --- |
| `&gt;` | `>` |
| `&lt;` | `<` |
| `&copy;` | © |

#### 14. Anidamiento
```html
<!-- Correcto -->
<p>Texto con <b>negrita</b></p>

<!-- Incorrecto: se cierran en orden cruzado -->
<p>Texto con <b>negrita</p></b>
```

### Configuración del proyecto HTML
1. Crear la carpeta del proyecto y abrirla en VS Code.
2. Crear `ejercicio1.html` y las carpetas `css/` e `img/`.
3. Enlazar la hoja de estilos dentro del `<head>`:
   ```html
   <link rel="stylesheet" href="css/style.css">
   ```
4. Declarar el idioma y la codificación:
   ```html
   <html lang="es">
   <meta charset="UTF-8">
   <meta name="viewport" content="width=device-width, initial-scale=1.0">
   ```
5. Visualizar la página con **Live Server** (clic derecho en `ejercicio1.html` → *Open with Live Server*).

---

## :construction: Incidencias y soluciones
| RA | Mensaje de error :bangbang: | Cuándo :question: | Causa :mag_right: | Solución :o: | Detectada :detective: |
| --- | --- | --- | --- | --- | --- |
| RA1 | `Permission denied` | Al ejecutar `./firewall.sh` | El script no tiene permiso de ejecución | `chmod +x firewall.sh` y ejecutar con `sudo` | 24/11/2026 |
| RA1 | Error al aplicar netplan | Al ejecutar `sudo netplan apply` | Mala indentación en el YAML | Revisar los espacios (sin tabuladores) | 24/11/2026 |
| RA2 | La página se ve sin estilos | Al abrir `ejercicio1.html` | La ruta del `href` no coincide con la del CSS | Comprobar que la ruta es `css/style.css` | 24/11/2026 |
| RA2 | La imagen no aparece | Al cargar la página | Ruta o nombre incorrecto (`img/gta.jpg`) | Revisar mayúsculas, extensión y carpeta | 24/11/2026 |

> [!NOTE]
> Las filas son ejemplos de errores típicos. Cámbialas por los que hayas tenido de verdad.

## :bulb: Decisiones técnicas
**RA1**
- **Una interfaz por red** (kali, dmz, lan) para separar el tráfico y poder controlarlo en el firewall.
- **IP fija en las redes internas** (`dhcp4: false`), para que el firewall siempre tenga la misma dirección como puerta de enlace.
- **MASQUERADE** para que las redes privadas salgan a Internet con una sola IP.
- **DNAT en el puerto 80** para publicar el servidor web de la DMZ sin exponer toda la red.
- **Script `firewall.sh`**, para repetir la configuración sin escribir cada regla a mano.

**RA2**
- **HTML semántico** (`header`, `nav`, `main`, `aside`, `footer`) en lugar de solo `div`.
- **CSS en un archivo externo**, para separar contenido y presentación.
- **Variables CSS** en `:root`: cambiando un color se actualiza toda la página.
- **CSS Grid** para la estructura principal y **Flexbox** para el menú y las demos.
- **Media query** (`max-width: 768px`) para adaptar la página a móviles.

## :memo: Reflexiones técnicas

**Lo que he aprendido**
- Vengo de SMIX, donde vimos sobre todo hardware y redes, así que el RA1 me resultó más familiar que el RA2.
- RA1: un firewall también puede hacer de router, con NAT para salir a Internet y para publicar un servidor.
- RA1: el orden de las reglas de `iptables` y la indentación del YAML importan mucho.
- RA2: HTML no es un lenguaje de programación, sino de **marcas**, y el CSS va separado.
- RA2: `class` se puede repetir, `id` es único, y las rutas deben ser exactas.

**Lo que más me ha costado**
- Entender la diferencia entre `PREROUTING` y `POSTROUTING`.
- El modelo de caja (`padding`, `border`, `margin`) y `position: absolute`.
- Saber cuándo usar Grid y cuándo Flexbox.

**Qué mejoraría**
- RA1: poner las políticas en `DROP` y permitir solo el tráfico necesario.
- RA2: pasar los estilos en línea (`style="..."`) al CSS y validar el código en el [validador de W3C](https://validator.w3.org/).
- Rehacer los dos ejercicios sin copiar, para comprobar que lo he entendido.

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