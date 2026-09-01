# Decisión de arquitectura — página de cotización

**Fecha:** 2026-09-01
**Archivo:** `cotizar.html`

## Qué es

Un wizard de 5 pasos que le hace al cliente las preguntas correctas y arma con las
respuestas un brief ordenado que llega por mail. El cliente ve un resumen de lo que
pidió y un mensaje de "gracias, te respondo dentro de las 24-48hs hábiles".

## Por qué NO calcula precios

El sitio es 100% estático y se sirve desde GitHub Pages. Cualquier lógica de precios
(tarifas base, multiplicadores por funcionalidad, reglas de descuento) quedaría escrita
en el JS público de la página, visible con "Ver código fuente" para cualquiera,
competencia incluida.

Además, la preferencia del negocio es **conversar el precio, no automatizarlo**: el
presupuesto depende de matices que un formulario no captura bien, y un número
automático mal calibrado genera más fricción que valor.

Por eso el wizard solo **recolecta y ordena información**. El paso 4 pregunta si el
cliente ya tiene un presupuesto definido, pero **sin pedir montos**: es para saber en
qué etapa de decisión está, no para cotizar.

## Por qué NO tiene backend

- El repo no tiene backend ni pipeline, y sumarlo (una función serverless, un servicio)
  agrega infraestructura, costo y una superficie de mantenimiento que hoy no existe.
- El formulario de contacto del index ya resuelve el envío sin backend, con **Formspree**.
  El cotizador reusa exactamente ese mecanismo: mismo endpoint (`maqrzekp`), mismo
  `fetch` con `FormData` y `Accept: application/json`.
- Todo el brief viaja en un **único campo `mensaje`** en texto plano con etiquetas
  (`Tipo de proyecto:`, `Problema a resolver:`, `Funcionalidades:`, …). Así el mail se
  lee de un vistazo y se evita el límite de cantidad de campos de Formspree.

### Consecuencias

- El formulario es público y sin filtro de servidor → hay un **honeypot** oculto
  (`_gotcha`) contra spam de bots.
- No hay validación de servidor → toda la validación es client-side, por paso.
- El estado del wizard se guarda en `sessionStorage` del navegador del cliente para que
  un F5 accidental no borre todo. Se limpia después de un envío exitoso. Ese dato nunca
  sale del dispositivo del cliente.
- Sin JavaScript el wizard no funciona; hay un `<noscript>` que deriva al formulario de
  contacto.

## Qué datos se recolectan

| Paso | Datos |
|---|---|
| 1 | Tipo de proyecto (una opción) |
| 2 | Rubro/actividad, problema a resolver, URL del sitio actual (opcional) |
| 3 | Funcionalidades requeridas (multi-select; las opciones dependen del paso 1) |
| 4 | Plazo, si hay presupuesto definido (sin montos), estado del contenido |
| 5 | Nombre, email, WhatsApp (opcional), empresa (opcional), aceptación de privacidad |

Todos estos datos están reflejados en `privacidad.html` (secciones 2 a 5). **Si se
agrega, quita o cambia un campo, hay que actualizar esa política en el mismo commit.**

## Si en el futuro se quiere cotización automática

Recién ahí conviene un backend mínimo (función serverless) que tenga la lógica de
precios del lado del servidor y devuelva un rango. No meter esa lógica en el cliente.
