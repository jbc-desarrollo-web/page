# Changelog

Formato basado en [Keep a Changelog](https://keepachangelog.com/es-ES/).

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
