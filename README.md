cat > README.md << 'READMEEOF'
# nece ser (du voyage) — Bitácora técnica

Registro del proceso de construcción del sitio neceser.co.
Cada sección documenta qué hicimos, por qué y qué aprendemos de ello.

---

## Índice
1. Herramientas y entorno
2. El generador de sitios — Hugo
3. El tema — PaperMod
4. GitHub y deployment
5. El dominio — neceser.co
6. Identidad visual y CSS
7. La biblioteca — layout personalizado
8. Lecciones aprendidas

---

## 1. Herramientas y entorno

- **Homebrew** — gestor de paquetes para Mac.
- **Ruby 3.3** — dependencia del entorno.
- **Hugo** — generador de sitios estáticos (v0.160.1+extended).
- **Git** — control de versiones.
- **VS Code** — editor de código.

Hugo genera sitios estáticos — HTML, CSS y JS puros, sin base de datos ni servidor dinámico. Ventajas: seguridad, velocidad y costo cero de hosting via GitHub Pages.

---

## 2. El generador de sitios — Hugo

Hugo convierte archivos Markdown en HTML usando plantillas (layouts). El sistema de override permite que cualquier archivo en layouts/ sobreescriba el equivalente del tema sin modificarlo.

El archivo hugo.toml es el cerebro de la configuración: define el nombre del sitio, idioma, tema y parámetros editoriales.

Cada publicación tiene un frontmatter con estos campos:

    title: "Título"
    date: 2026-04-27
    draft: false
    tags: ["categoria", "autor"]
    categoria: "literatura"   — define el color del lomo
    tipo: "lomo"              — "lomo" (vertical) o "portada" (horizontal)

---

## 3. El tema — PaperMod

PaperMod es un tema open source instalado como git submodule. El CSS personalizado vive en assets/css/extended/neceser.css — PaperMod lo carga automáticamente.

---

## 4. GitHub y deployment

Cada push a main dispara GitHub Actions, que construye el sitio con Hugo y lo publica en GitHub Pages automáticamente. La carpeta public/ está en .gitignore — el build lo hace Actions, no el repo local.

---

## 5. El dominio — neceser.co

Registrado en Namecheap. DNS configurado con cuatro A Records apuntando a GitHub (185.199.108-111.153) y un CNAME www → neceser26.github.io.

---

## 6. Identidad visual y CSS

Paleta derivada de la cantimplora de peregrino:

- Noche #1A1814 — fondo principal
- Página #F5F0E8 — texto principal
- Calabaza #9E6B2E — acento cinematográfico, madera del librero
- Musgo #5C7A54 — acento literario
- Ceniza #7A8280 — texto secundario, bordes

Tipografía: Palatino — serif humanista, Hermann Zapf, 1949.

---

## 7. La biblioteca — layout personalizado

### La visión
El homepage es un librero personal que se llena orgánicamente con cada publicación. Lomos verticales para textos cortos, portadas horizontales para ensayos. Sin orden predefinido — cronológico de publicación. A futuro: reorganización interactiva por categoría y easter eggs al llenarse.

### Archivos clave
- layouts/index.html — homepage / el librero
- layouts/_default/list.html — lista de posts en /posts/
- assets/css/extended/neceser.css — estilos completos
- .gitignore — excluye public/ y .DS_Store

### Categorías y colores
- cine — Calabaza #9E6B2E
- literatura — Musgo #5C7A54
- ciencia — Ceniza #7A8280
- filosofia — Tierra oscura #3D3830

### Estado actual — 27/04/26
- OK Librero con laterales físicos en layouts/index.html
- OK Homepage = librero (no lista de posts)
- OK public/ en .gitignore — localhost coincide con neceser.co
- OK Primer libro — Calvino, verde Musgo
- PENDIENTE Más publicaciones para llenar el estante
- PENDIENTE Refinamiento visual — iluminación, sombras, posición
- PENDIENTE Reorganización interactiva por categoría
- PENDIENTE Easter eggs al llenarse el librero

---

## 8. Lecciones aprendidas

- Hugo v0.160+ no acepta bloques define vacíos — lanza "unexpected EOF"
- CSS roto antes de .biblioteca invalida todos los estilos siguientes
- Limpiar caché con rm -rf resources/ al cambiar CSS
- public/ debe estar en .gitignore — si se commitea, entra en conflicto con Actions
- Siempre parar el servidor (Ctrl+C) antes de limpiar y reconstruir
- El CSS debe estar limpio — texto literal de comandos adentro lo corrompe
- Los campos del frontmatter deben coincidir exactamente con los del template

---

Bitácora iniciada el 27 de abril de 2026.
Construido con Hugo, GitHub Pages y criterio editorial.
READMEEOF
git add . && git commit -m "README limpio y coherente — sesión 27/04/26" && git push