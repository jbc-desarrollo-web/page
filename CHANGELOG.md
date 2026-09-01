# Changelog

Formato basado en [Keep a Changelog](https://keepachangelog.com/es-ES/).

## 2026-09-01 — Rediseño visual, página de entrada y legales

Pasa de un tema oscuro genérico de plantilla a un minimalismo editorial construido
sobre el logo real. Detalle completo de cada decisión en `docs/decisiones.md`; paleta
y ratios de contraste verificados en `docs/design-system.md`.

### Marca (`assets/brand/`, todas las páginas)

- Logo real reemplaza al ícono genérico (flecha en cuadrado azul): SVG del wordmark
  "JBC" inline con `currentColor`, favicons, `apple-touch-icon` y `og-image` reales.
  Assets renombrados a sus nombres canónicos (traían un sufijo `" (1)"` de descarga
  duplicada).
- Nombre de marca **"JBC" → "Estudio JBC"** en `<title>`, hero/entrada, footer,
  `og:site_name`, `alt`/`aria-label` del logo y ambos documentos legales (confirmado
  con Justin).

### Paleta y tipografía (todas las páginas)

- Tema oscuro `oklch()` genérico → paleta clara derivada del logo (`bone`/`sand`/
  `navy`/`acento`), con navy reservado para la entrada, el footer y los CTA.
- Google Fonts CDN → fuentes **autohosteadas** en `assets/fonts/*.woff2` (Space
  Grotesk, Inter, JetBrains Mono). Ver `docs/decisiones.md` para el motivo (privacidad
  + performance) y cómo se armaron los archivos.
- Nueva familia mono (JetBrains Mono) para etiquetas y números de orden en
  versalitas — no existía antes.

### Componentes (todas las páginas)

- Tarjetas: fuera sombra difusa, degradado, íconos en círculo de color. Ahora borde
  de 1px, radio de 4px, sin sombra en reposo, `translateY(-2px)` en hover, número de
  orden en mono. "Proceso" y "Problema" dejan de ser tarjetas y pasan a listas
  tipográficas separadas por una línea de 1px.
- Botones: de pastilla (`border-radius: 999px`) a radio de 4px, parejo con el resto
  del sistema.
- Formulario de contacto y wizard de cotización: mismos campos, validación y envío
  (Formspree) — solo cambia el vestido. Bordes de input/foco/error recalculados a
  `--navy-400` para pasar el mínimo de contraste de 3:1 de componentes UI (el valor
  anterior, más claro, no llegaba).

### Página de entrada (`index.html`, nueva sección)

- `index.html` arranca con una sección `100dvh` en navy: logo grande, una frase
  (“Análisis, desarrollo y mantenimiento de sistemas y sitios web.”), línea de
  metadatos en mono (ubicación, disciplina, año), prueba concreta (+3 años
  programando, +10 proyectos en curso — únicos números reales) y dos acciones:
  **Explorar** (scroll a la cotización) y **Contacto** (WhatsApp, `wa.me`, con
  mensaje precargado). Sin scroll interno, respeta `prefers-reduced-motion`.

### Reordenamiento (`index.html`)

- Orden nuevo: entrada → **cotización** (nueva sección teaser) → problema → solución
  → proceso → evidencia de trabajo → FAQ → contacto → footer. El teaser de
  cotización se reescribió para funcionar en frío, sin depender de haber leído la
  sección de problema todavía.
- Testimonios reemplazados por **evidencia de trabajo entregado**: mismos 3 clientes
  reales (Colonia Michis, LP Slots, JIT Arquitectura) y mismas citas, sin estrellas
  decorativas, con espacio para captura real (ver `docs/decisiones.md` — todavía sin
  capturas).
- Nav y menú móvil actualizados con el nuevo anchor `#cotizacion`.

### Pantalla de carga — eliminada (`index.html`)

- Se sacó el loader (HTML, CSS y JS) completo. Mitigado el riesgo de FOUC/salto de
  fuente con `font-display: swap`, `preload` de los 3 pesos usados arriba del
  pliegue, y `size-adjust`/`ascent-override` de fallback en los `@font-face`.

### Legales

- `privacidad.html`: responsable identificado (Justin Baltasar Chaparro,
  `desarrollos.jbc@gmail.com`), agregado GitHub Pages como tercero que procesa
  solicitudes técnicas, aclarado que no hay analítica, rediseñada con la paleta
  nueva.
- `terminos.html` — **nueva.** Alcance de servicios, cotizaciones estimativas y no
  vinculantes, propiedad intelectual de los entregables, límite de responsabilidad.

### SEO

- `robots.txt` y `sitemap.xml` — nuevos (no existían). El sitemap solo lista
  `index.html` y `cotizar.html` (los legales llevan `<meta name="robots" content="noindex">`).

### Documentación

- `CLAUDE.md` actualizado (marca, diseño, páginas, rutas relativas por el subpath
  de GitHub Pages). `docs/design-system.md` y `docs/decisiones.md` — nuevos.

## 2026-09-01 — Reposicionamiento + página de cotización

### Reposicionamiento del mensaje (`index.html`, `privacidad.html`)

El sitio comunicaba, en la práctica, "hacemos landing pages". Ahora comunica
"hacemos soluciones web": la landing pasa a ser **uno** de los servicios.

- `<title>`, `<meta description>`, h1 y subtítulo del hero reescritos para reflejar el
  alcance real (sitios, landing pages, herramientas y sistemas web a medida).
- Sección `#solucion`: de 3 tarjetas de atributos (diseño / desarrollo / mantenimiento)
  a 4 tarjetas de **tipos de servicio**: sitios y landing pages · herramientas y sistemas
  a medida · rediseño y optimización · mantenimiento y soporte. Título de la sección:
  "Una solución web para cada necesidad".
- FAQ: reescrita la respuesta de "¿Cuánto demora?" (ya no arranca por "una landing en
  pocos días") y sumadas dos preguntas: "¿Hacen solo páginas o también sistemas web?" y
  "¿Pueden integrarlo con herramientas que ya uso?".
- Testimonios sin cambios (son reales y de landings; no se tocan).
- `privacidad.html`: descripción de la actividad actualizada al alcance nuevo.

### Página de cotización (`cotizar.html`, nueva)

Wizard de 5 pasos + pantalla de resumen, para que quien ya sabe lo que quiere arme un
brief ordenado. **No calcula ni muestra precios** (ver `docs/cotizador.md` para el
porqué). Mismo header, footer, estilos y tokens que el index.

- Paso 1 tipo de proyecto (elección única) → Paso 2 proyecto → Paso 3 funcionalidades
  (multi-select cuyas opciones dependen del paso 1) → Paso 4 plazos y alcance → Paso 5
  contacto → resumen editable por bloque.
- Validación por paso con el error debajo del campo; botón "Atrás"; estado en
  `sessionStorage` para sobrevivir un F5 (se limpia al enviar); honeypot anti-spam;
  navegable por teclado, foco al encabezado del paso y `aria-live` al cambiar de paso.
- Envío por el **mismo mecanismo que el formulario del index** (Formspree, endpoint
  `maqrzekp`). El cuerpo del mail es el brief formateado en texto plano con etiquetas
  (`Tipo de proyecto:`, `Problema a resolver:`, `Funcionalidades:`, etc.) en un único
  campo `mensaje`.
- Enganche desde el index: botón "Cotizar mi proyecto" en el nav (desktop y mobile) y
  en el CTA final. El formulario de contacto del index queda como la vía rápida.

### Privacidad (`privacidad.html`)

El cotizador recolecta más datos personales que el formulario de contacto, así que la
política se actualiza en el mismo commit:

- Nuevos datos listados: WhatsApp, URL del sitio actual, rubro, tipo de proyecto y
  funcionalidades, plazo, situación de presupuesto (sin montos), estado del contenido.
- Aclarado el uso de `sessionStorage` local del navegador.
- Finalidad nueva: elaborar propuesta y presupuesto.
- Formspree: aclarado que procesa **ambos** formularios y que aloja datos fuera de la Argentina.
- Plazo de conservación concreto: 12 meses si la consulta/cotización no deriva en trabajo.
- Fecha de última actualización: 1 de septiembre de 2026.

### Documentación (nueva)

`CLAUDE.md`, `README.md`, `CHANGELOG.md` y `docs/cotizador.md`.
