# Estudio JBC — sitio

Sitio de Estudio JBC (Justin Baltasar Chaparro). Presenta los servicios (sitios,
landing pages, herramientas y sistemas web a medida, rediseño, mantenimiento), muestra
trabajo entregado y capta consultas.

## Stack

- HTML + CSS + JS **vanilla**. Sin framework, sin build step, sin backend, sin dependencias.
- Cada página es autocontenida: su CSS va en un `<style>` en el `<head>` y su JS en un
  `<script>` antes de `</body>`. No hay `.css` ni `.js` compartidos (es la convención del
  repo; los tokens de diseño se copian entre páginas y se mantienen en sync a mano).
- Fuentes: **autohosteadas** en `assets/fonts/*.woff2` (no Google Fonts CDN — ver
  "Marca y diseño" abajo). Íconos: SVG inline estilo Lucide.
- Tema **claro** fijo (`color-scheme: light`), minimalismo editorial: bone/sand de fondo,
  navy reservado para la entrada, el footer y los CTA. Colores en hex, con `color-mix()`
  puntual. Se usa `:has()` y `:focus-within` (baseline ≈ navegador de 2023).

## Marca y diseño

- **Assets de marca** en `assets/brand/` (logo real vectorizado, favicons, `og-image.png`).
  Ver `assets/brand/COMO-USAR.md`. El logo se usa **inline** (no `<img>`) para heredar
  color vía `currentColor`.
- **Paleta, tipografía, escala y ratios de contraste verificados:** ver
  [`docs/design-system.md`](docs/design-system.md). Es la única fuente de verdad de los
  tokens — si cambiás un color, actualizá ese doc y propagá el valor a mano a los 4 HTML.
- **Por qué de las decisiones de arquitectura** (entrada de pantalla completa,
  autohosteo de fuentes, ausencia de banner de cookies): ver
  [`docs/decisiones.md`](docs/decisiones.md).
- Nombre de marca: **"Estudio JBC"** en todo texto visible/meta (title, hero, footer,
  `og:site_name`, `alt`/`aria-label` del logo, legales). El wordmark del logo se lee
  "JBC"; el nombre completo va en copy y metadatos.

## Páginas

| Archivo | Qué es |
|---|---|
| `index.html` | Landing principal. Orden de secciones: entrada (`#top`, 100dvh) → `#cotizacion` (teaser) → `#problema` → `#solucion` → `#proceso` → `#testimonios` (evidencia de trabajo entregado) → `#faq` → `#contacto`. |
| `cotizar.html` | Wizard de cotización de 5 pasos + resumen. No calcula precios: arma un brief y lo envía. Ver `docs/cotizador.md`. Mismo sistema de diseño que el index; **su marcado, validación y envío no se tocan sin necesidad** — solo el vestido visual. |
| `privacidad.html` | Política de privacidad (Ley 25.326, Argentina; AAIP como organismo de control). **Se actualiza junto con cualquier cambio en los datos que se recolectan.** |
| `terminos.html` | Términos y condiciones: alcance de servicios, cotizaciones estimativas y no vinculantes, propiedad intelectual de los entregables, límite de responsabilidad. |

## Formularios

Ambos (`#contacto` en el index y el wizard en `cotizar.html`) envían por **Formspree**
(`https://formspree.io/f/maqrzekp`) vía `fetch` con `FormData` y `Accept: application/json`.
El wizard manda el brief completo en un solo campo `mensaje` (texto plano con etiquetas)
más `name`, `email`, `_subject` y el honeypot `_gotcha`.

Si se cambia de servicio de formularios o se agregan campos nuevos → actualizar
`privacidad.html` en el mismo commit.

## Correr local

No necesita servidor para abrir el HTML, pero el `fetch` del formulario y las rutas
relativas (assets, fuentes) andan mejor servidas:

```
npx http-server -p 8123     # o cualquier server estático
# http://localhost:8123
```

## Deploy

GitHub Pages desde el repo `jbc-desarrollo-web/page` (publicado en
`https://jbc-desarrollo-web.github.io/page/`, bajo un subpath). Push a la rama que Pages
tenga configurada y listo; no hay pipeline. **Por eso todos los assets se referencian con
rutas relativas** (`assets/...`, nunca `/assets/...`) — una ruta absoluta rompería bajo
el subpath `/page/`.

## Convenciones

- Tono del copy: profesional, orientado a resultado de negocio, sin exagerar ni prometer números.
- Indentación 2 espacios. Nombres de clases en `kebab-case`.
- JS: IIFE con `"use strict"`, sin librerías, compatible con navegadores modernos.
- Accesibilidad: `<label>` reales, foco visible (contraste ≥3:1, ver `docs/design-system.md`),
  `aria-live` para cambios dinámicos, respeta `prefers-reduced-motion`.
- Todas las imágenes con `alt` real y `width`/`height` explícitos (evitar layout shift).
