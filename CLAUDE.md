# CLAUDE.md

## Proyecto

Tutorial interactivo para configurar un entorno profesional de Data Science (Hazla con Datos). Sitio estático desplegado en GitHub Pages.

## Stack

- **Astro 5** con MDX para contenido
- **Tailwind CSS v4** (via vite plugin, NO PostCSS)
- **astro-expressive-code** para bloques de código
- **canvas-confetti** para animaciones de progreso
- TypeScript

## Comandos

- `npm run dev` — servidor de desarrollo
- `npm run build` — build estático a `dist/`
- `npm run preview` — preview del build

## Estructura

```
src/
├── components/    # Astro components (Sidebar, MobileNav, OsTabs, etc.)
├── content/       # MDX files (step-00 a step-12) — el contenido del tutorial
├── layouts/       # Layout.astro (wrapper principal)
├── pages/         # index.astro (page única, renderiza todos los steps)
└── styles/        # global.css (Tailwind + custom styles)
```

## Arquitectura clave

### Selector OS global (Win/Mac)
- Un solo toggle global en Sidebar y MobileNav (NO tabs por sección)
- Se aplica clase `os-win` / `os-mac` en `<html>` via script inline anti-FOUC en Layout.astro
- CSS en global.css: `html.os-mac .os-content-win { display: none }` y viceversa
- El componente `OsTabs.astro` solo renderiza wrappers `os-content-win` / `os-content-mac` (sin JS propio)
- Los estilos del toggle activo se aplican con clases Tailwind desde JS (no CSS custom), porque Tailwind v4 tiene mayor especificidad

### Orden de secciones
- La página agrupa: primero Fundamentos (9 secciones), luego Avanzado (4 secciones)
- Sidebar, MobileNav y NextSection widget deben mantener el mismo orden
- IDs de secciones son `paso-0` a `paso-12` (los números NO son secuenciales por grupo)

### Progreso
- localStorage key: `hcd-entorno-progress`
- Botones "Marcar como completado" en cada sección con confetti
- Checkmarks en sidebar y mobile nav

### Temas
- Dark mode con clase `dark` en `<html>`, persistido en localStorage key `theme`
- Anti-FOUC: script inline en `<head>` de Layout.astro

## Deploy

- GitHub Pages via `.github/workflows/deploy.yml`
- Base path: `/configuracion-entorno`
- Site: `https://paulovillarroel.github.io`

## Convenciones

- Idioma del contenido: español
- Commits en inglés
- Siempre ejecutar `npm run build` antes de commit para verificar
- Los MDX usan componentes `<OsTabs>`, `<Callout>`, `<CommandTable>` importados manualmente
