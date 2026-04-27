# nece ser (du voyage) — Bitácora técnica

Registro del proceso de construcción del sitio neceser.co.
Cada sección documenta qué hicimos, por qué y qué aprendemos de ello.

---

## Índice
1. [Herramientas y entorno](#1-herramientas-y-entorno)
2. [El generador de sitios — Hugo](#2-el-generador-de-sitios--hugo)
3. [El tema — PaperMod](#3-el-tema--papermod)
4. [GitHub y deployment](#4-github-y-deployment)
5. [El dominio — neceser.co](#5-el-dominio--neceserco)
6. [Identidad visual y CSS](#6-identidad-visual-y-css)
7. [La biblioteca — layout personalizado](#7-la-biblioteca--layout-personalizado)

---

## 1. Herramientas y entorno

### Qué instalamos
- **Homebrew** — gestor de paquetes para Mac. Equivalente al App Store para herramientas de desarrollo. No modifica el sistema operativo.
- **Ruby 3.3** — lenguaje de programación. Lo necesitamos como dependencia.
- **Hugo** — el generador de sitios estáticos que usamos para construir neceser.co.
- **Git** — sistema de control de versiones. Registra cada cambio que hacemos al código.
- **VS Code** — editor de código. Reemplaza TextEdit para editar archivos del proyecto sin errores de formato.

### Por qué estas herramientas
Hugo genera sitios **estáticos** — archivos HTML, CSS y JS puros, sin base de datos ni servidor dinámico. Eso tiene tres ventajas para neceser: seguridad (no hay nada que hackear), velocidad (los archivos se sirven directamente) y costo cero de hosting via GitHub Pages.

### Comandos clave
```bash
# Instalar Homebrew
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

# Instalar Hugo
brew install hugo

# Verificar instalación
hugo version

# Crear nuevo sitio
hugo new site neceser

# Levantar servidor local
hugo server --buildDrafts
```

---

## 2. El generador de sitios — Hugo

### Cómo funciona
Hugo toma archivos de contenido escritos en **Markdown** (`.md`) y los convierte en páginas HTML usando plantillas (layouts). La estructura del proyecto es:
### El archivo hugo.toml
Es el cerebro de la configuración. Aquí definimos el nombre del sitio, el idioma, el tema y los parámetros editoriales:

```toml
baseURL = 'https://neceser.co/'
languageCode = 'es'
title = 'nece ser'
theme = 'PaperMod'

[params]
  description = "Cine · Literatura · Camino"
  defaultTheme = "dark"

[markup]
  [markup.goldmark]
    [markup.goldmark.renderer]
      unsafe = true  # Permite HTML dentro de archivos Markdown
```

### Crear contenido
```bash
# Crear nueva publicación
hugo new content posts/nombre-del-post.md
```

Cada publicación tiene un **frontmatter** — metadata en la parte superior del archivo:

```yaml
---
title: "Si una noche de invierno un viajero"
date: 2026-04-27
draft: false
tags: ["literatura", "calvino"]
categoria: "literatura"
tipo: "lomo"          # "lomo" o "portada" — define cómo aparece en la biblioteca
---
```

---

## 3. El tema — PaperMod

### Qué es
PaperMod es un tema open source para Hugo — define el diseño base del sitio. Lo instalamos como **git submodule**, lo que significa que vive como referencia al repositorio original, no como copia.

```bash
git submodule add https://github.com/adityatelange/hugo-PaperMod themes/PaperMod
```

### Cómo lo personalizamos
Hugo tiene un sistema de **override** — cualquier archivo en `layouts/` sobreescribe el equivalente del tema sin modificarlo. Eso nos permite personalizar sin tocar PaperMod directamente.

El CSS personalizado vive en `assets/css/extended/neceser.css` — PaperMod carga automáticamente cualquier archivo en esa carpeta.

---

## 4. GitHub y deployment

### Cómo funciona el flujo
Cada vez que hacemos cambios al sitio seguimos este flujo:

```bash
git add .                          # Prepara los cambios
git commit -m "descripción"        # Guarda los cambios con un mensaje
git push                           # Sube los cambios a GitHub
```

GitHub Actions detecta el push y ejecuta automáticamente el workflow en `.github/workflows/hugo.yml` — construye el sitio con Hugo y lo publica en GitHub Pages. El proceso tarda entre 30 segundos y 2 minutos.

### Por qué GitHub Pages
- Hosting gratuito y permanente
- HTTPS automático
- Dominio personalizado sin costo adicional
- El sitio es un archivo estático — máxima seguridad

---

## 5. El dominio — neceser.co

### Dónde se compró
Namecheap — registrador de dominios con precios competitivos y WhoisGuard gratuito (protege los datos personales del registro público).

### Cómo se conectó a GitHub Pages
En Namecheap → Advanced DNS se configuraron estos registros:

| Tipo | Host | Valor |
|------|------|-------|
| A Record | @ | 185.199.108.153 |
| A Record | @ | 185.199.109.153 |
| A Record | @ | 185.199.110.153 |
| A Record | @ | 185.199.111.153 |
| CNAME | www | neceser26.github.io |

Los registros A apuntan el dominio a los servidores de GitHub. El CNAME conecta `www.neceser.co` al repositorio.

En GitHub → Settings → Pages → Custom domain se agregó `neceser.co`. HTTPS se activa automáticamente en minutos.

---

## 6. Identidad visual y CSS

### La paleta
Cinco colores derivados de la cantimplora de peregrino — el objeto que da nombre y filosofía a la publicación:

| Nombre | Hex | Uso |
|--------|-----|-----|
| Noche | `#1A1814` | Fondo principal |
| Página | `#F5F0E8` | Texto principal, fondos claros |
| Calabaza | `#9E6B2E` | Acento cinematográfico |
| Musgo | `#5C7A54` | Acento literario |
| Ceniza | `#7A8280` | Texto secundario, bordes |

### La tipografía
**Palatino** — serif humanista diseñado por Hermann Zapf en 1949, inspirado en los manuscritos del Renacimiento italiano. Elegido por su carácter académico y su legibilidad en pantalla.

### Cómo se aplica el CSS
El archivo `assets/css/extended/neceser.css` se carga automáticamente sobre PaperMod. Establece la tipografía, los colores y la atmósfera visual del sitio.

---

## 7. La biblioteca — layout personalizado

### La visión
Las publicaciones se muestran como una biblioteca personal — lomos verticales para textos cortos, portadas horizontales para ensayos o textos considerables. Cada categoría tiene su color de lomo.

### Categorías y colores
| Categoría | Color | Hex |
|-----------|-------|-----|
| cine | Calabaza | `#9E6B2E` |
| literatura | Musgo | `#5C7A54` |
| ciencia | Ceniza | `#7A8280` |
| filosofia | Tierra oscura | `#3D3830` |

### Cómo definir el tipo en cada publicación
En el frontmatter de cada post:

```yaml
tipo: "lomo"        # Aparece como lomo vertical
tipo: "portada"     # Aparece como portada horizontal
categoria: "cine"   # Define el color del lomo
```

### El archivo de layout
`layouts/_default/list.html` — sobreescribe la lista de publicaciones de PaperMod con el diseño de biblioteca.

---

*Bitácora iniciada el 27 de abril de 2026.*
*Construido con Hugo, GitHub Pages y criterio editorial.*
---

### Estado actual de la biblioteca
- ✅ Layout funcional en `layouts/_default/list.html`
- ✅ CSS de biblioteca en `assets/css/extended/neceser.css`
- ✅ Primer lomo visible: Calvino, verde Musgo
- ⬜ Agregar más publicaciones
- ⬜ Ajustar posición del estante en la página
- ⬜ Hover y animaciones pulidas

### Lecciones aprendidas
- Hugo v0.160+ no acepta bloques `{{ define }}` vacíos — lanza "unexpected EOF"
- CSS roto antes de `.biblioteca` invalida todos los estilos siguientes
- Limpiar caché con `rm -rf resources/` al cambiar CSS
- Los campos del frontmatter deben coincidir exactamente con los del template: `categoria` para color, `tipo` para formato
