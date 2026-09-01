# HANDOFF — desing (DESIGN by COSMART, design.cosmart.com.ar)

Actualizado: 2026-09-01. Léelo antes de tocar código en este repo. El backend de los formularios vive en `AquiVane/cosmart-workers`, worker `cosmart-design` — ver su propio `HANDOFF.md` si lo hay, si no, revisar `workers/cosmart-design/src/index.js` directo (está comentado).

## Formulario de cotización migrado de Formspree a Cloudflare (01/09)

`#mainForm` (sección "Cotizá tu proyecto") mandaba directo a `formspree.io/f/xpqnagap` — sin pasar por Brevo ni por COSMART, no quedaba ningún registro del lado nuestro. Mismo problema que tenían Shows/ComuniCOS/Rumbo Voraz antes de su propia migración (ver comentario "ex-Formspree" en `cosmart-design/src/index.js`). Ahora manda a `POST https://cosmart-design.conglomeradocosmart.workers.dev/design/contacto` (`handleDesignContacto`), que dispara un mail interno a Vaneh y Ger con todos los datos (mismo patrón que `/design/compra` y `/taller/compra`, que sí siempre mandaron por acá).

Se agregó también la pregunta **"¿Sos creador/a de contenido? Sí/No"** (radio, obligatoria) al formulario — pedido explícito de Vaneh, para poder identificar si quien cotiza es una creadora de EUFORIA activando la propuesta de tercerización de contenido (ver abajo).

## ⚠️ Pendiente de Vaneh: confirmar que el dominio esté bien asociado

Vaneh reportó (01/09) que no está segura de que `design.cosmart.com.ar` tenga el dominio custom bien asociado en Cloudflare Pages — dijo que cuando creó esta landing puede que todavía no lo tuviera configurado. Esto es algo que **solo se puede verificar desde el dashboard de Cloudflare** (Workers & Pages → el proyecto de Pages de este repo → Custom domains), no desde acá. Si el dominio no está bien delegado/asociado, el formulario recién migrado (arriba) tampoco va a poder probarse en producción. Confirmar esto es lo primero a hacer antes de dar por resuelta la migración del formulario.

## Oferta de tercerización de contenido (EUFORIA → Design)

Ya existe como email (secuencia 4 de EUFORIA, `tplSecuencia4` en `euforia-worker/src/index.js`): las creadoras que no llegan a producir todo su contenido pueden tercerizar con el equipo de Design — **hasta 10 videos de 1 minuto por $100.000**, cupos limitados por mes — con un botón que ya linkea a `design.cosmart.com.ar`. No hizo falta agregar nada nuevo de ese lado; la pregunta "¿Sos creador/a de contenido?" en el formulario de acá es lo que permite identificar a quien llega por esa oferta al cotizar.
