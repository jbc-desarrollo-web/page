# Sistema de diseño — Estudio JBC

Fuente de verdad de tokens, tipografía y reglas de uso. Si cambiás un valor acá,
propagalo a mano a los `<style>` de `index.html`, `cotizar.html`, `privacidad.html` y
`terminos.html` (no hay CSS compartido — ver `CLAUDE.md`).

## Paleta

Derivada del logo real (`assets/brand/logo-jbc.svg`), no inventada.

```css
/* Marca */
--navy-950: #030B20;
--navy-900: #05152F;  /* color base del logo */
--navy-800: #071836;
--navy-700: #0C2450;
--navy-600: #13356F;
--navy-500: #1E4A8F;
--navy-400: #4A72AD;
--navy-300: #8AA5C8;
--navy-200: #C4D2E3;
--navy-100: #E8EDF4;

/* Neutros cálidos */
--bone:     #FAF9F6;  /* fondo principal */
--sand-100: #F1EDE6;  /* fondo de secciones alternas */
--sand-300: #DCD5C9;  /* separadores decorativos */

/* Acento — con cuentagotas */
--acento: #C8A882;

/* Estado */
--danger:  #8C1D18;
--success: #155A30;
```

### Ratios de contraste verificados (WCAG 2.x, calculados con `contrast.js` — fórmula de
luminancia relativa estándar, no una herramienta externa)

| Par | Ratio | Veredicto |
|---|---|---|
| navy-900 texto / bone | 17.27:1 | AAA |
| navy-600 texto / bone | 11.29:1 | AAA |
| navy-500 texto / bone | 8.20:1 | AAA |
| navy-400 texto / bone (solo texto grande) | 4.64:1 | AA |
| navy-900 texto / sand-100 | 15.58:1 | AAA |
| navy-600 texto / sand-100 | 10.19:1 | AAA |
| navy-500 texto / sand-100 | 7.40:1 | AAA |
| **acento texto / bone — NO USAR** | 2.13:1 | **FALLA** |
| blanco texto / navy-900 | 18.19:1 | AAA |
| navy-200 texto / navy-900 | 11.84:1 | AAA |
| acento texto / navy-900 | 8.12:1 | AAA |
| navy-900 texto / acento | 8.12:1 | AAA |
| danger texto / bone | 8.66:1 | AAA |
| success texto / bone | 7.86:1 | AAA |
| sand-300 borde / bone (decorativo) | 1.38:1 | falla — es decorativo, no funcional |
| navy-200 borde / bone (decorativo) | 1.46:1 | falla — es decorativo, no funcional |
| navy-400 borde / bone (funcional) | 4.64:1 | ≥3:1 OK |
| navy-400 borde / sand-100 (funcional) | 4.18:1 | ≥3:1 OK |
| navy-400 borde / navy-900 (foco sobre navy) | 3.73:1 | ≥3:1 OK |

**Reglas derivadas:**

- Texto sobre `--bone` o `--sand-100`: `--navy-900` (cuerpo/títulos), `--navy-600`
  (secundario), `--navy-500` (terciario/etiquetas chicas). **`--navy-400` como texto
  solo si es texto grande** (≥18.66px) — a tamaño chico (mono labels, números de
  orden) da 4.64:1 en `--bone` pero 4.18:1 en `--sand-100`, que falla AA para texto
  normal. Por eso los números de orden (`.card .num`, `.process-step .num`,
  `.problem-item .num`, `.evidence-media span`) usan **`--navy-500`**, no
  `--navy-400` — tienen que leerse igual en secciones `bone` y `sand-100`
  (alternan, ver "Uso de sección alterna" abajo).
- **`--acento` nunca como texto sobre fondo claro** (2.13:1, falla). Se usa como
  relleno de bloque (línea de acento del hero, progreso del wizard), como
  subrayado/borde, o como texto **sobre `--navy-900`** (8.12:1, AAA).
- Bordes **decorativos** (separadores, marcos de tarjeta): `--sand-300` o
  `--navy-200`. Bordes **funcionales** (inputs, choice-cards, anillos de foco,
  estados de error del cotizador): `--navy-400` o más oscuro (mínimo 3:1 de
  componente UI, no 4.5:1 de texto).
- `--danger` / `--success` (estados de formulario): AAA en `--bone`, no se usan
  como fondo, solo como texto/borde.

## Tipografía

- **Títulos** (`--font-display`): Space Grotesk 500–700. Geométrica y ancha —
  dialoga con el wordmark del logo sin copiarlo.
- **Cuerpo y UI** (`--font-sans`): Inter 400–600. x-height alta, cómoda en formularios.
- **Etiquetas y metadatos** (`--font-mono`): JetBrains Mono 500–600, versalitas
  (`text-transform: uppercase`), `letter-spacing: .12em`, `font-size: .75rem`. Es
  el detalle que más rápido comunica "esto lo hizo alguien que trabaja con sistemas".

Escala fluida con `clamp()`:

| Elemento | `clamp()` |
|---|---|
| Display (entrada) | `clamp(1.75rem, 4.5vw, 3rem)` |
| h2 de sección | `clamp(1.75rem, 3.5vw, 2.75rem)` |
| Cuerpo | `1rem`, `line-height: 1.65` |

- Títulos grandes: `letter-spacing: -0.02em`.
- Medida de párrafo: `max-width: 62–70ch` (nunca texto corriendo a `1200px`).
- Autohosteadas en `assets/fonts/*.woff2` (ver `docs/decisiones.md` — por qué no
  Google Fonts CDN). `font-display: swap` + `preload` de los 3 pesos usados arriba
  del pliegue + `size-adjust`/`ascent-override` de fallback (ver
  `@font-face 'Inter Fallback'` / `'Space Grotesk Fallback'` en cada página).

## Anatomía de las cartas (`.card`)

- Sin sombra en reposo. En hover: `translateY(-2px)`, transición `150ms`.
- Radio: `--radius: 4px` (todo el sitio; el pedido original permitía "0 a 4px", se
  eligió 4px por parejo con los inputs del cotizador).
- Grilla de tarjetas: 1px de `--border-decor` entre celdas (`background` del grid +
  `gap: 1px` simulan el borde compartido sin duplicar bordes).
- Padding: `2rem` desktop, colapsa por `clamp` implícito de `.container` en mobile.
- Número de orden en mono arriba a la izquierda (`01`, `02`…) en `--navy-500`.
- Título en `--font-display`, cuerpo en Inter `.95rem` en `--text-secondary`.
- **No todo es una tarjeta:** "Proceso" y "Problema" son listas tipográficas con
  separador de 1px, no cajas — evita la sopa de tarjetas.

## Uso del acento — con cuentagotas

Lugares donde aparece `--acento` en el sitio (y en ningún otro):

1. Línea decorativa bajo el logo de la entrada (`.entry-rule`, fondo, no texto).
2. Texto de metadatos de la entrada, sobre navy (`.entry-meta`, 8.12:1 AAA).
3. Relleno de la barra de progreso del wizard (`.wizard-progress-track span`).
4. Fondo del botón primario en hover sobre navy (`.btn-fill-light:hover`), con
   texto `--navy-900` encima (8.12:1 AAA).

Si en algún momento aparece en más de 3–4 lugares por pantalla, se está perdiendo
la escasez que lo hace funcionar.

## Uso de sección alterna

El fondo alterna `--bone` / `--sand-100` sección por sección para dar ritmo sin
recurrir a cajas. `--navy-900` queda reservado para la **entrada** y el **footer**
(y como fondo de botón primario, que es un elemento chico, no una sección).

Orden en `index.html`: navy (entrada) → sand (cotización) → bone (problema) → sand
(solución) → bone (proceso) → sand (evidencia) → bone (FAQ) → sand (contacto) →
navy (footer).

## Espaciado y layout

- Escala base de 8px (todos los valores en `rem` son múltiplos de `.25rem`/4px o
  `.5rem`/8px).
- Ritmo vertical entre secciones: `5rem` (80px) mobile, `7.5rem` (120px) desktop
  (`@media (min-width:1024px)`).
- Ancho máximo de contenido: `75rem` (1200px, `.container`). Bloques de texto:
  `40–44rem` (~680px) o `62–70ch` para párrafos.
- Asimetría deliberada: `.section-head.asym` usa una grilla de 12 columnas donde
  el kicker arranca en la columna 2, el h2 en la 2–9 y el párrafo en la 5–12 (en
  desktop; colapsa a bloque apilado en mobile). Se usa en "Solución" y "Proceso"
  para romper el centrado por defecto; FAQ y Contacto se mantienen centrados
  porque son secciones utilitarias, no editoriales.
