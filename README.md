# Estudio JBC — sitio web

Sitio estático de Estudio JBC: sitios institucionales, landing pages,
herramientas y sistemas web a medida, rediseño y mantenimiento.

- **En vivo:** https://jbc-desarrollo-web.github.io/page/
- **Stack:** HTML + CSS + JS vanilla. Sin build, sin dependencias, sin backend.

## Estructura

```
index.html        Landing principal (entrada + cotización + contenido)
cotizar.html      Wizard de cotización (5 pasos + resumen)
privacidad.html   Política de privacidad
terminos.html     Términos y condiciones
robots.txt        Directivas para crawlers
sitemap.xml       Mapa del sitio (páginas indexables)
assets/brand/     Logo real, favicons, og-image — ver assets/brand/COMO-USAR.md
assets/fonts/     Fuentes autohosteadas (.woff2)
docs/             Decisiones de arquitectura y sistema de diseño
```

Cada página lleva su CSS y su JS embebidos (no hay `.css`/`.js` compartidos, es la
convención del repo — ver `CLAUDE.md`).

## Correr local

Abrí `index.html` en el navegador, o serví la carpeta (recomendado, para que el
envío de formularios y las rutas relativas de assets/fuentes funcionen igual que en
producción):

```bash
npx http-server -p 8123
# abrí http://localhost:8123
```

Cualquier servidor de archivos estáticos sirve (`python -m http.server`, la
extensión Live Server de VS Code, etc.).

## Editar

- No hay paso de compilación: editás el `.html` y recargás.
- Los formularios usan [Formspree](https://formspree.io) (endpoint en el `<script>` de
  cada página). Si cambiás qué datos se piden, actualizá `privacidad.html`.
- Paleta, tipografía y ratios de contraste: [`docs/design-system.md`](docs/design-system.md).
- Más contexto en [`CLAUDE.md`](CLAUDE.md), [`docs/cotizador.md`](docs/cotizador.md) y
  [`docs/decisiones.md`](docs/decisiones.md).

## Deploy

GitHub Pages. Push a la rama publicada del repo `jbc-desarrollo-web/page`. El sitio se
sirve bajo el subpath `/page/`, por eso todos los assets usan rutas relativas.

## Changelog

Ver [`CHANGELOG.md`](CHANGELOG.md).
