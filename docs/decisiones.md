# Decisiones de arquitectura — rediseño

Por qué se resolvió así, no solo qué se hizo. Fecha: 2026-09-01.

## Por qué la entrada es la primera pantalla del index, no una página aparte

Se evaluaron dos opciones: `entrada.html` separada, o una sección `100dvh` al
principio de `index.html`. Se eligió la segunda:

- **Un solo clic menos.** Una página de entrada separada obliga a un clic extra
  ("Explorar") antes de llegar al contenido real. El visitante que llega desde una
  búsqueda de Google cae directo en el contenido, no en una pantalla de portada.
- **Google indexa una sola cosa.** Con dos páginas, el `<title>`/`description` de la
  entrada y los del contenido compiten por relevancia en los resultados de
  búsqueda. Con una sola página, todo el peso de SEO se concentra ahí.
- **Menos superficie para mantener.** Una página HTML menos, un `<style>` menos que
  mantener en sync a mano (ver la convención de "sin CSS compartido" en `CLAUDE.md`).

El costo es que la sección de entrada agrega peso a `index.html` (un archivo ya
grande). Se acepta porque no agrega peticiones de red nuevas — es texto e inline
SVG, no imágenes.

## Por qué autohosteo de fuentes

El sitio usaba el CDN de Google Fonts (`fonts.googleapis.com` / `fonts.gstatic.com`).
Cada visita le entrega la IP del visitante a Google antes de que cargue una sola
línea de contenido propio. Eso:

1. Es un dato personal (IP) procesado por un tercero — hay que declararlo en la
   política de privacidad como parte de "con quién compartimos tus datos", aunque
   Google no lo use para publicidad en ese contexto.
2. Agrega una conexión externa más en la cadena crítica de carga (DNS + TLS a un
   host nuevo) antes de poder pintar texto.

Autohosteando en `assets/fonts/*.woff2` se elimina el problema de raíz: no hay
tercero que reciba la IP, y no hay conexión extra — los `.woff2` viajan por el
mismo origen que el resto del sitio. `privacidad.html` ya no necesita mencionar a
Google Fonts como tercero.

**Cómo se armaron los archivos:** se pidió la hoja de estilos CSS2 de Google Fonts
para Space Grotesk (500/600/700), Inter (400/500/600) y JetBrains Mono (500/600) y
se comprobó que, para cada familia, el subset "latin" es el **mismo archivo físico**
en los tres pesos pedidos — son fuentes variables servidas por Google detrás de
`@font-face` con `font-weight` discreto, no instancias estáticas separadas por peso.
Por eso alcanza con **un `.woff2` por familia** (no uno por peso): el propio
`@font-face` autohosteado declara `font-weight: 400 600` (rango), y el navegador
interpola el eje `wght` del archivo variable. El subset "latin" (`U+0000–00FF` +
puntuación) cubre el español completo (incluida `ñ`, `¿`, `¡`, vocales acentuadas),
así que no hace falta el subset "latin-ext".

Las métricas de `size-adjust`/`ascent-override`/`descent-override` de las fuentes
de reemplazo (`Inter Fallback`, `Space Grotesk Fallback`) son **aproximaciones**
tomadas de valores públicos conocidos para estas familias, no midieron con una
herramienta de introspección de fuentes en esta sesión (no había una disponible sin
agregar una dependencia nueva). Si el CLS medido en producción da alto, ajustar esos
valores con una herramienta como Fontaine o Capsize.

## Por qué todavía no hay banner de cookies

El sitio no usa cookies de seguimiento ni analítica (ver `privacidad.html`, sección
2). El único almacenamiento del lado del cliente es `sessionStorage` para el
progreso del wizard de cotización, que:

- No es una cookie (no viaja en cada request al servidor).
- No es de seguimiento (no identifica al usuario entre sesiones ni sitios).
- Se borra solo al enviar el formulario o cerrar la pestaña.

Bajo la Ley 25.326 y la interpretación general de "cookies esenciales" (equivalente
local al criterio de la ePrivacy Directive europea), este uso no requiere
consentimiento previo vía banner — sí requiere estar declarado en la política de
privacidad, y ya lo está. Si en el futuro se agrega analítica (Plausible, GA, Meta
Pixel, etc.) o cualquier cookie no esencial, ahí sí hace falta un banner de
consentimiento y hay que actualizar `privacidad.html` en el mismo commit (regla ya
documentada en `CLAUDE.md`).

## Por qué el radio de las tarjetas es 4px parejo, no "0 a 4px"

El pedido original daba un rango (2–4px, o directamente 0). Se fijó **4px en todo
el sitio** — tarjetas, inputs, botones — para que no haya dos escalas de radio
conviviendo sin motivo. Ver `docs/design-system.md`.

## Por qué los botones dejaron de ser pastilla (`border-radius: 999px`)

El diseño anterior usaba botones tipo pastilla, un patrón genérico de plantilla
SaaS. Se cambió a `--radius: 4px` (el mismo radio del resto del sistema) para que
los botones se sientan parte del mismo lenguaje editorial, no un componente
importado de otro sistema de diseño.

## Por qué no hay capturas reales en "Trabajo entregado" todavía

Se decidió con Justin (ver conversación) no fabricar capturas de pantalla —
esta sesión no tiene una herramienta de navegador/captura disponible. Cada bloque
de evidencia (`#testimonios` en `index.html`) tiene un `.evidence-media` con un
comentario HTML indicando el `<img>` exacto a pegar cuando esté la captura real
(ruta sugerida: `assets/proyectos/<nombre>.png`), y mientras tanto muestra un
placeholder de texto ("Captura pendiente") en vez de un cuadro roto o vacío.
