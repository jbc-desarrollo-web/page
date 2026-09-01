# JBC — sitio web

Sitio estático de JBC Desarrollo Web: sitios institucionales, landing pages,
herramientas y sistemas web a medida, rediseño y mantenimiento.

- **En vivo:** https://jbc-desarrollo-web.github.io/page/
- **Stack:** HTML + CSS + JS vanilla. Sin build, sin dependencias, sin backend.

## Estructura

```
index.html        Landing principal
cotizar.html      Wizard de cotización (5 pasos + resumen)
privacidad.html   Política de privacidad
docs/             Decisiones de arquitectura
```

Cada página lleva su CSS y su JS embebidos.

## Correr local

Abrí `index.html` en el navegador, o serví la carpeta (recomendado, para que el
envío de formularios y las rutas relativas funcionen igual que en producción):

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
- Más contexto en [`CLAUDE.md`](CLAUDE.md) y [`docs/cotizador.md`](docs/cotizador.md).

## Deploy

GitHub Pages. Push a la rama publicada del repo `jbc-desarrollo-web/page`.

## Changelog

Ver [`CHANGELOG.md`](CHANGELOG.md).
