# JBC — sitio

Sitio de JBC Desarrollo Web. Presenta los servicios (sitios, landing pages,
herramientas y sistemas web a medida, rediseño, mantenimiento) y capta consultas.

## Stack

- HTML + CSS + JS **vanilla**. Sin framework, sin build step, sin backend, sin dependencias.
- Cada página es autocontenida: su CSS va en un `<style>` en el `<head>` y su JS en un
  `<script>` antes de `</body>`. No hay `.css` ni `.js` compartidos (es la convención del
  repo; los tokens de diseño se copian entre páginas y se mantienen en sync a mano).
- Fuentes: Google Fonts (Space Grotesk + Inter). Íconos: SVG inline estilo Lucide.
- Tema oscuro fijo. Colores en `oklch()` + `color-mix()`. Se usa `:has()` y `:focus-within`
  (baseline ≈ navegador de 2023).

## Páginas

| Archivo | Qué es |
|---|---|
| `index.html` | Landing principal. Secciones: hero, problema, `#solucion`, `#proceso`, `#testimonios`, `#faq`, CTA final, `#contacto`. |
| `cotizar.html` | Wizard de cotización de 5 pasos + resumen. No calcula precios: arma un brief y lo envía. Ver `docs/cotizador.md`. |
| `privacidad.html` | Política de privacidad (Ley 25.326, Argentina). **Se actualiza junto con cualquier cambio en los datos que se recolectan.** |

## Formularios

Ambos (`#contacto` en el index y el wizard en `cotizar.html`) envían por **Formspree**
(`https://formspree.io/f/maqrzekp`) vía `fetch` con `FormData` y `Accept: application/json`.
El wizard manda el brief completo en un solo campo `mensaje` (texto plano con etiquetas)
más `name`, `email`, `_subject` y el honeypot `_gotcha`.

Si se cambia de servicio de formularios o se agregan campos nuevos → actualizar
`privacidad.html` en el mismo commit.

## Correr local

No necesita servidor para abrir el HTML, pero el `fetch` del formulario y las rutas
relativas andan mejor servidas:

```
npx http-server -p 8123     # o cualquier server estático
# http://localhost:8123
```

## Deploy

GitHub Pages desde el repo `jbc-desarrollo-web/page` (publicado en
`https://jbc-desarrollo-web.github.io/page/`). Push a la rama que Pages tenga
configurada y listo; no hay pipeline.

## Convenciones

- Tono del copy: profesional, orientado a resultado de negocio, sin exagerar ni prometer números.
- Indentación 2 espacios. Nombres de clases en `kebab-case`.
- JS: IIFE con `"use strict"`, sin librerías, compatible con navegadores modernos.
- Accesibilidad: `<label>` reales, foco visible, `aria-live` para cambios dinámicos,
  respeta `prefers-reduced-motion`.
