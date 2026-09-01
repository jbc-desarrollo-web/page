# Assets de marca — Estudio JBC

Vectorizados desde el logo original. Coincidencia con el archivo fuente: **98.36% (IoU)**.
Proporción del wordmark: **439.5 × 100** (aspecto 4.395:1). Respetala siempre.

## Archivos

| Archivo | Uso |
|---|---|
| `logo-jbc.svg` | **El que vas a usar el 90% de las veces.** Usa `currentColor`: hereda el color del CSS |
| `logo-jbc-blanco.svg` | Color fijo blanco, para fondos oscuros |
| `logo-jbc-navy.svg` | Color fijo `#05152F`, para fondos claros |
| `logo-jbc-blanco.png` | 2400px, transparente. Fallback y usos fuera de la web |
| `logo-jbc-navy.png` | 2400px, transparente |
| `favicon.svg` | Wordmark completo sobre cuadrado navy |
| `favicon-monograma-j.svg` | Solo la J. **Más legible a 16px** |
| `favicon-32.png` / `favicon-16.png` | Fallback para navegadores viejos |
| `apple-touch-icon.png` | 180×180, **opaco y sin redondeo** (iOS aplica su propia máscara) |
| `og-image.png` | 1200×630. Miniatura al compartir el link por WhatsApp |

## `logo-jbc.svg` es el importante

Hereda el color del contexto, así que un solo archivo sirve para todos los fondos:

```html
<span class="logo"><!-- pegar acá el contenido del svg --></span>
```
```css
.logo svg { height: 32px; width: auto; display: block; }
.header--claro .logo { color: var(--navy-900); }
.header--oscuro .logo { color: #fff; }
```

Inline en el HTML, no como `<img>`: solo así funciona `currentColor`.

## Favicon: cuál elegir

Ampliá `legibilidad.png` y decidí. A 32px el wordmark completo se lee bien; a 16px se empasta
porque las letras quedan de 3px de alto. El monograma de la J aguanta cualquier tamaño.

Recomendación: serví el SVG y dejá que el navegador escale.

```html
<link rel="icon" href="/assets/brand/favicon.svg" type="image/svg+xml">
<link rel="icon" href="/assets/brand/favicon-32.png" sizes="32x32">
<link rel="apple-touch-icon" href="/assets/brand/apple-touch-icon.png">
```

## Reglas

- **Área de resguardo:** dejá libre alrededor del logo el equivalente a la altura de la J.
- **Tamaño mínimo:** 90px de ancho en pantalla. Abajo de eso los contraformas de la B se cierran.
- No lo estires, no lo rotes, no le pongas sombra, no lo pongas sobre fotos sin una capa de contraste.
- No uses la versión navy sobre fondos oscuros ni la blanca sobre claros: el contraste se va al piso.

## Colores verificados

| Token | Hex | Contraste |
|---|---|---|
| `--navy-900` | `#05152F` | 17.27:1 sobre `#FAF9F6` — AAA |
| Blanco | `#FFFFFF` | 17.73:1 sobre `#05152F` — AAA |

Degradado del og-image: `#061A3D` → `#050D24` a 135°, tomado del archivo original.
