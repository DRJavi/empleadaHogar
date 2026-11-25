---
name: web-seo-checklist
description: Control de SEO, AEO y GEO para dar por terminada una web, punto por punto. Cubre metadata, imagen para compartir, datos estructurados (JSON-LD), reseñas, arquitectura e indexación a escala, sitemap, redirects, blog, analítica, higiene técnica, Core Web Vitals y contenido citable por ChatGPT, Perplexity y Google AI Overviews. Cada punto termina con estado (hecho, fallido, N/A, pendiente o diferido) y dueño; un punto sin estado es un control no pasado. Nunca inventa datos. Usala al lanzar o tocar una web, una landing, un blog o páginas a escala, o cuando pidas "auditá el SEO de esta web", "revisá el schema", "¿está lista para lanzar?" o "¿por qué no nos citan las IAs?".
license: MIT
---

# web-seo-checklist

Es el control que usamos en [Claura](https://claura-ai.com) antes de dar por terminada cualquier web de un cliente. Cada punto existe porque algo se rompió en una web real; donde sirve, el punto cuenta el caso.

Está escrito para sitios hechos con Next.js (App Router), pero casi todo vale para cualquier stack: donde dice `metadata` o `generateStaticParams`, leé "el lugar donde tu framework define eso".

## Con el MCP de Claura (opcional)

La skill funciona sola, revisando el código y el sitio publicado a mano. Si sos cliente de Claura y tu agente tiene
conectado el [MCP de Claura](https://claura-ai.com/mcp) (Claude, ChatGPT o Claude Code conectados a tu cuenta de
Claura), usá sus herramientas para lo que de otro modo se mide a mano:

- **Auditar el sitio publicado** página por página (estado, canonical, schema, enlaces rotos).
- **Leer Search Console y Analytics** de la marca para los puntos de verificación y medición.
- **Preguntarles a ChatGPT y a Google** por la marca para el punto de menciones sin clic.

Antes de usar una herramienta que gasta o publica, mostrá qué vas a hacer y esperá la confirmación. Si el MCP no está
conectado, seguí a mano: ningún punto depende de él.

## 0. Cómo se corre (regla dura)

1. **Punto por punto.** Armá una lista de tareas por sección y recorré cada punto contra el sitio real (el código y el HTML servido).
2. **Cada punto termina con un estado**: `hecho`, `fallido`, `N/A`, `pendiente` o `diferido`.
   - `N/A` lleva el motivo.
   - `pendiente` y `diferido` llevan el motivo y **quién lo destraba** (el cliente, el lanzamiento, una decisión del equipo).
3. **Un punto sin estado en el informe es un control no pasado.** Pasa seguido: se corre "todo" y una sección queda afuera sin que nadie lo note.
4. **Los datos que faltan no se inventan ni frenan el control.** Van a una lista de "pedidos al cliente": horario de atención, rango de precios, medios de pago, perfil de Google Business, LinkedIn del fundador, premios, foto del vocero.
5. **Web que todavía no está en su dominio**: los puntos que necesitan la URL pública (Search Console, caché de redes, prueba de resultados enriquecidos) quedan `diferido` hasta el lanzamiento, nunca salteados ni marcados como hechos.

Cada punto lleva un ID entre comentarios (`<!-- seo.meta.canonical-all-pages -->`). Usalo en el informe: no cambia aunque reordenes el documento.

### Formato del informe

```
SECCIÓN 1. Metadata
- seo.meta.metadata-base: hecho
- seo.meta.hreflang-real-locales: N/A (sitio en un solo idioma)
- seo.meta.gsc-verification: diferido · dueño: lanzamiento (el dominio todavía no apunta)
...
PEDIDOS AL CLIENTE
- Horario de atención real (para openingHoursSpecification)
- Link del perfil de Google Business
RESULTADO: N hechos · N fallidos · N pendientes · N diferidos
```

## 1. Metadata base

- [ ] <!-- seo.meta.metadata-base --> `metadataBase`, `title` (por defecto y plantilla `%s | Marca`) y `description` con intención, entidad, ubicación, prueba social y llamado a la acción.
- [ ] <!-- seo.meta.favicon-coherence --> **Favicon completo y coherente.** En Next, declarar `metadata.icons` apaga la convención por archivo: el `icon.svg` deja de enlazarse si no está en la lista (ponelo primero). Set mínimo: `icon.svg`, `favicon.ico` (16 y 32), 192, 512, apple-touch 180 y manifest. El ícono sale del isotipo real de la marca, simplificado para leerse a 16 px.
- [ ] <!-- seo.meta.canonical-all-pages --> `canonical` en **todas** las páginas; en paginación, uno distinto por página. **Nunca en el layout raíz**: se hereda y manda todas las páginas a la home. Va por página, con un helper tipo `pageAlternates(locale, path)`.
- [ ] <!-- seo.meta.hreflang-real-locales --> **hreflang honesto**: solo idiomas con contenido traducido de verdad, más `x-default`. Cada idioma declara su propia canonical y se referencia a sí mismo y a los demás. El selector de idioma usa `<a href>` rastreables sin `nofollow`.
- [ ] <!-- seo.meta.robots-googlebot --> `robots` y `googleBot` con `max-snippet: -1` y `max-image-preview: large`.
- [ ] <!-- seo.meta.geotargeting-signals --> **Ubicación con señales que Google usa**: URLs por idioma o región, `hreflang`, dirección local visible, perfil de Google Business y `address`/`geo` en el schema. Los meta `geo.position`, `geo.region`, `ICBM` y similares no sirven. Las coordenadas salen de una dirección real, no se estiman.
- [ ] <!-- seo.meta.noindex-out-sitemap --> Páginas `noindex` **fuera del sitemap**: las dos cosas juntas se contradicen.
- [ ] <!-- seo.meta.gsc-verification --> Verificación de Search Console (archivo en `/public` o meta tag).

## 2. Arquitectura e indexación a escala

- [ ] <!-- seo.arch.template-inventory --> **Inventario de tipos de página antes de optimizar**: por plantilla, propósito, intención, entidad, URL, datos, llamado a la acción, canonical, indexación y schema. Mínimo: home, hubs, categorías o servicios, combinaciones con ubicación, fichas, editorial y utilitarias.
- [ ] <!-- seo.arch.navigable-hierarchy --> **Jerarquía navegable**: hubs, categorías, ubicación o filtro indexable y detalle, solo cuando refleja cómo busca la gente. Header, footer, migas y relacionados con `<a href>` reales. El footer enlaza destinos estratégicos, no cientos de ciudades.
- [ ] <!-- seo.arch.programmatic-landing-index-gate --> **Landings locales o programáticas**: una combinación se indexa solo si tiene demanda, oferta, contenido propio, datos y enlaces. Sin valor propio, no se genera o queda fuera del índice. No hay un mínimo universal de fichas: se define por plantilla.
- [ ] <!-- seo.arch.intent-template-onpage --> **Plantilla por intención**: `title`, meta description y H1 únicos; H2 y H3 cubren preguntas y modificadores locales. Nunca la misma frase repetida en alt, títulos, preguntas y anchors.
- [ ] <!-- seo.onpage.titulos-con-busquedas --> **Títulos que suman búsquedas (H1 a H4)**: cada título de la página, también los de bloques como el listado, los relacionados, las preguntas y el llamado, suma una búsqueda: otra forma de la principal, un caso de uso, una herramienta, un público, un lugar o una pregunta. Ningún H2 repite el H1 ni es un rótulo sin tema («Preguntas frecuentes» a secas, «Todas las notas», «Contacto»): «Preguntas frecuentes sobre la indexación en Google» sí. En plantillas el subtítulo es un dato propio de cada página, no la etiqueta de la categoría repetida. Las palabras salen de Search Console de esa página y de la investigación de palabras. Después de cambiar un subtítulo, revisá los enlaces que apuntaban a su ancla.
- [ ] <!-- seo.arch.contextual-bidirectional-interlinks --> **Enlaces internos en las dos direcciones**: hubs, categorías, ubicación, fichas y artículos enlazan a las páginas que venden, y estas devuelven. Anchors descriptivos y variados que prometen exactamente el destino.
- [ ] <!-- seo.arch.stable-readable-urls --> **URLs estables y legibles**. No migrar URLs indexadas solo por meterles palabras clave. Si cambia un slug: 301 directo, y actualizar enlaces, canonical, hreflang y sitemap.
- [ ] <!-- seo.arch.redirect-map --> **Redirects 1 a 1**: 301 o 308 del servidor al reemplazo equivalente, sin cadenas ni loops. Nada de mandar todo lo desconocido a la home (Google lo trata como 404 blando). Sin reemplazo real, 404 o 410 útil.
- [ ] <!-- seo.arch.facets-search-zero-results --> **Filtros, búsquedas internas y cero resultados con regla escrita**: solo combinaciones elegidas se indexan; una combinación imposible da 404; una búsqueda vacía puede dar 200 con `noindex` y sugerencias. El canonical no reemplaza a un 404 ni a un `noindex`.
- [ ] <!-- seo.arch.canonical-vs-redirect --> **Canonical y redirect no son lo mismo**: 301 para una URL reemplazada; canonical para duplicados accesibles. Toda página indexable se apunta a sí misma.
- [ ] <!-- seo.arch.sitemap-source-of-truth --> **Sitemap desde una fuente de verdad**: el conjunto de URLs indexables conocidas es exactamente el que se emite. Solo URLs absolutas, canónicas, indexables y 200, sin borradores. `lastmod` sale de una fecha real de contenido (`updatedAt ?? publishedAt`) o se omite; **nunca la hora del build** ni cambios cosméticos.
- [ ] <!-- seo.arch.support-content-crawlability --> **Contenido de ayuda rastreable**, con canonical y enlaces, aunque viva en otra herramienta. Subdominio o subcarpeta no dan ranking por sí solos.

## 3. Imagen para compartir (Open Graph)

- [ ] <!-- seo.og.dedicated-1200x630 --> **Dedicada, 1200×630.** Nunca una foto genérica ni otra proporción: WhatsApp, Facebook y LinkedIn la recortan.
- [ ] <!-- seo.og.brand-composition --> Logo, frase con la propuesta, prueba social, dominio y la cara del vocero si la marca tiene uno. Fondo de la marca. Si la generás por código, cargá las fuentes reales del sitio desde un archivo y mirá el resultado: un servicio de fuentes que falla puede devolver una página de error en vez de la fuente, y la imagen sale con la letra de respaldo sin ningún aviso.
- [ ] <!-- seo.og.large-text --> **Textos grandes**: título de 60 px o más y secundarios de 25 px o más. Se ve en miniatura en el feed.
- [ ] <!-- seo.og.metadata-wiring --> Declarada en el layout raíz (con `twitter: summary_large_image`, ancho, alto y alt), en las páginas con metadata propia y como respaldo de las páginas dinámicas.
- [ ] <!-- seo.og.measured-dimensions --> **`og:image:width` y `height` se miden del archivo, nunca se escriben a mano.** En claura-ai.com, 103 de 114 artículos declaraban 1920×1080 sobre imágenes de otro tamaño. Leé la medida de los bytes del archivo (no de la extensión: un `.png` puede ser un JPEG) y, si no se puede medir, omitila.
- [ ] <!-- seo.og.verify-against-file --> **Verificar contra el archivo publicado**: bajar la imagen que anuncia la página y medirla. Leer las etiquetas solo prueba que existen.
- [ ] <!-- seo.og.no-empty-images --> **Ningún `openGraph` sin `images`.** En Next, el `openGraph` de una página reemplaza el del layout raíz: si omite `images`, esa página se comparte sin imagen. Cerralo con un test que falle ante un `openGraph` sin `images`.
- [ ] <!-- seo.og.no-portrait-no-logo --> **Nunca una foto vertical ni un logo como imagen social.** Si cada ficha tiene foto propia, generá una tarjeta de 1200×630 por foto con recorte al centro.

## 4. Datos estructurados (JSON-LD)

Un solo `@graph`, renderizado en el servidor, en el `<head>`.

- [ ] <!-- seo.schema.page-type --> **Schema por tipo de página y por lo que se ve**: Organization o LocalBusiness, WebSite, WebPage y BreadcrumbList cuando aplican; Article en editorial; ItemList y la entidad del rubro en listados y fichas. `keywords` no da ranking. `Service` es semántica pero no da resultado enriquecido; tampoco `HowTo`, el cuadro de búsqueda de sitelinks ni, desde mayo de 2026, `FAQPage`.
- [ ] <!-- seo.schema.organization-specific-types --> **Organization con su tipo específico** como lista (`["Organization","ProfessionalService"]`): `name`, `legalName`, `alternateName` como lista (todas las formas en que buscan la marca), `slogan`, `description`, `url`, `logo`, `image`, `telephone`, `email`, `foundingDate`, `priceRange`, `award`, `knowsAbout`, `sameAs`.
- [ ] <!-- seo.schema.sameas-verification --> **`sameAs` verificado**: cada URL se abre y es el perfil, no una publicación suelta. Los canales de un producto no se mezclan con los de la empresa. Los mismos perfiles van en el footer.
- [ ] <!-- seo.schema.localbusiness-signals --> **Señales locales**: `contactPoint`, `hasMap`, `address` y `geo`, `areaServed` (país y ciudad), `openingHoursSpecification` (**el horario real, nunca inventado**), `currenciesAccepted`, `paymentAccepted`. Lo que falta va a pedidos al cliente.
- [ ] <!-- seo.schema.website-searchaction --> `WebSite` con `publisher` al `@id` de la organización. `SearchAction` solo si existe una búsqueda real.
- [ ] <!-- seo.schema.entity-ids --> **Una entidad, un `@id`.** Organization, WebSite y cada autor se describen una vez (`<dominio>#organization`) y el resto los referencia. Nada de copias de la marca dentro de cada artículo. La página del autor es `ProfilePage` con la `Person` como `mainEntity`.
- [ ] <!-- seo.schema.entity-stubs --> **Cada página que referencia un `@id` lo incluye**: un buscador resuelve los `@id` dentro de una sola página. Si una subpágina apunta a un nodo que solo está en la home, el enlace queda roto sin que el validador se queje. Lleva una versión mínima del nodo (`@id`, `@type`, `name`, `url`).
- [ ] <!-- seo.schema.person --> `Person` por fundador o socio (`jobTitle`, `worksFor`, `alumniOf`, `knowsAbout`, `sameAs`).
- [ ] <!-- seo.schema.breadcrumb-visible --> **`BreadcrumbList` y miga visible armadas desde la misma lista** (un helper), con la misma raíz en todas las plantillas. El schema sin miga visible es un punto a medias.
- [ ] <!-- seo.schema.faq-visible-only --> **FAQ visible y `FAQPage` son decisiones separadas.** No agregues preguntas por costumbre. Si emitís `FAQPage`, sale del mismo componente que se ve, con texto idéntico. Google dejó de mostrar el resultado enriquecido de FAQ para todos los sitios el 7 de mayo de 2026: emitilo solo si sale gratis.
- [ ] <!-- seo.schema.faq-answer-in-html --> **Las respuestas de cada pregunta están en el HTML con la pregunta cerrada.** Un acordeón que monta la respuesta al abrirla la esconde de Google y de las IAs, que no hacen clic. Usá `<details>` o el contenido montado y plegado. Verificalo con `curl` en una página de cada plantilla.
- [ ] <!-- seo.schema.plain-text --> **Texto plano en todo el JSON-LD**: nada de `[texto](url)`, `**` ni etiquetas. Si el texto sale de MDX, pasa por un solo limpiador compartido.
- [ ] <!-- seo.schema.template-matrix --> **Matriz de schema por plantilla sobre el sitemap entero**: agrupá las URLs por plantilla y anotá los `@type`. Marcá plantillas indexables sin JSON-LD, preguntas marcadas que no se ven, migas distintas de las visibles y `url` distinto del canonical. Se repite cada vez que nace una plantilla.
- [ ] <!-- seo.schema.logo-square --> **`Organization.logo` es el logo real**: cuadrado o casi, de 112 px o más. Nunca la imagen de 1200×630.
- [ ] <!-- seo.schema.free-tool --> **Herramientas gratis**: `WebApplication` con `offers.price: "0"`, `applicationCategory` de la lista de Google (por ejemplo `BusinessApplication`), `operatingSystem: "Web"` y `provider`. El resultado enriquecido necesita reseñas reales: sin ellas, no se agregan.
- [ ] <!-- seo.schema.clone-parity --> **Si la web reemplaza a otra, la referencia es la vieja**: antes de cambiar el dominio, guardá qué `@type` servía cada página del sitio original y compará todas las rutas, no una muestra. Después del cambio la vieja ya no existe para comparar.
- [ ] <!-- seo.schema.data-integrity --> **Cada dato del schema sale de una fuente**: cantidades, precios, fechas, superficies, servicios. Lo no confirmado no se afirma. El JSON-LD es la versión más citada y menos revisada de la web: un dato inventado ahí termina en Google y en las respuestas de las IAs.

## 5. Reseñas (reglas duras)

- [ ] <!-- seo.reviews.no-fabrication --> **Jamás inventar reseñas** ni atribuir texto que el cliente no dijo.
- [ ] <!-- seo.reviews.schema-authorized --> `Review` solo con texto real, autor real y permiso del cliente.
- [ ] <!-- seo.reviews.aggregate-rating-self-serving --> **Nunca `aggregateRating` con reseñas alojadas en el propio sitio** de un negocio local: Google lo ignora y puede penalizar. Las estrellas salen del perfil de Google Business.
- [ ] <!-- seo.reviews.gbp-review-link --> Si faltan reseñas, armá el link para dejar reseña desde el perfil de Google Business y pasáselo al cliente.

## 6. Verificación antes de dar por bueno

- [ ] <!-- seo.qa.home-social-meta --> `curl` a la home: `og:image`, ancho, alto, alt y `twitter:image` correctos. Sin servidor a mano, vale el HTML prerenderizado del build.
- [ ] <!-- seo.qa.jsonld-parse --> El JSON-LD parsea y tiene los campos nuevos, en la home **y en una página de cada tipo**.
- [ ] <!-- seo.qa.jsonld-images-resolve --> **Toda imagen del JSON-LD es una URL absoluta bien formada que responde 200**, pedida tal cual. Ojo con armar `${SITE}${cover}` cuando la imagen ya es absoluta: sale `https://dominio.comhttps://...`.
- [ ] <!-- seo.qa.og-http-size --> La imagen social responde 200, es `image/jpeg` o `png` y pesa menos de 300 KB.
- [ ] <!-- seo.qa.rich-results --> Prueba de resultados enriquecidos de Google sobre la URL publicada.
- [ ] <!-- seo.qa.sitemap-robots-200 --> **Sitemap y robots, completo**: cada URL emitida responde 2xx sin redirect, en el host canónico, sin `noindex` (ni en meta ni en `X-Robots-Tag`) y con canonical a sí misma.
- [ ] <!-- seo.qa.template-locale-matrix --> **Una URL de cada tipo de página y estado** (con resultados, sin resultados, paginada, otro idioma): estado, indexación, título, H1, canonical, schema y enlaces. Auditar solo la home no valida un sitio a escala.
- [ ] <!-- seo.qa.visual-breakpoints --> **Revisión visual en escritorio, tablet y celular**: sin scroll horizontal, títulos, tarjetas, tablas y botones conservan jerarquía, contraste y tamaño táctil.
- [ ] <!-- seo.qa.internal-links --> **Ningún enlace interno roto** en las páginas nuevas: cada `href="/..."` responde 200. Los relacionados escritos a mano son los que más se rompen.

## 7. Después de publicar (cada vez que cambia la imagen social o la metadata)

- [ ] <!-- seo.postdeploy.social-rescrape --> **Refrescar la caché de Facebook y WhatsApp**, que guardan la imagen vieja semanas: `POST https://graph.facebook.com/v26.0/` (o la versión vigente de la Graph API) con `id=<url>`, `scrape=true` y un token de app (`APP_ID|APP_SECRET`). Sin token ya no funciona. Validá por la URL de la imagen en la respuesta.
- [ ] <!-- seo.postdeploy.sharing-debugger --> Revisión visual en el depurador de contenido compartido de Facebook.

## 8. Blog y artículos

- [ ] <!-- seo.blog.exists --> **El sitio de la marca tiene blog.** Sin blog no tiene dónde crecer en búsqueda. N/A solo para piezas que no son el sitio de la marca (una propuesta, una landing de campaña).
- [ ] <!-- seo.blog.hub-categories --> **Hub y categorías indexables**: el hub explica el alcance con texto propio; cada tema con demanda tiene su URL, título, H1, introducción y migas. Un filtro en el navegador no reemplaza a la página de categoría. Si el menú ofrece categorías, enlaza solo a las que existen como página, con `<a href>` reales, y sigue llevando al blog completo.
- [ ] <!-- seo.blog.cards --> **Tarjetas completas**: título, descripción propia, categoría, imagen con medidas y enlace. Si existe la página de la categoría, la etiqueta enlaza ahí.
- [ ] <!-- seo.blog.pagination --> **Paginación con URL propia** y enlaces `<a href>`. Nada de botones que solo reordenan en el navegador.
- [ ] <!-- seo.blog.hub-schema --> `CollectionPage` en el hub; `ItemList` solo si lista exactamente lo que se ve.
- [ ] <!-- seo.blog.schema --> Cada artículo emite `BlogPosting` o `Article` y `BreadcrumbList` (y `FAQPage` si corresponde). Canonical, URL, imagen, fechas y autor coinciden entre HTML y schema.
- [ ] <!-- seo.blog.authorship --> **Autoría transparente**: firma enlazada a un perfil con bio, redes verificadas y artículos. `Person` solo con identidad real; si no, un equipo editorial explícito. Fecha publicada y modificada reales; `dateModified` cambia solo con una edición de fondo.
- [ ] <!-- seo.blog.bilingual-canonical --> En sitios bilingües con artículos en un solo idioma, la ruta del otro idioma apunta al idioma real y el sitemap lleva solo la canónica.
- [ ] <!-- seo.blog.drafts-excluded --> Borradores fuera de producción, del sitemap y de las rutas generadas.
- [ ] <!-- seo.blog.extractable-structure --> **Estructura extraíble**: un H1, índice con anclas que funcionan, H2 y H3 enfocados, resumen al principio y tablas o listas solo cuando ayudan.
- [ ] <!-- seo.blog.faqs-by-level --> **Preguntas por nivel**: el artículo, las de su subtema; la categoría, un set chico distinto; la home del blog, normalmente ninguna. Ninguna pregunta repetida entre niveles. Cada respuesta con 1 a 3 enlaces y al menos una fuente externa en la sección.
- [ ] <!-- seo.blog.sources --> **Fuentes y datos**: estadísticas, precios, plazos, temas legales y ahorros con fuente primaria enlazada; sin fuente, se sacan.
- [ ] <!-- seo.blog.interlinks --> **Red de enlaces del artículo**: el artículo enlaza a las páginas que venden y a 2 notas relacionadas o más; al menos 2 páginas relevantes enlazan al artículo. Un blog sin enlaces de vuelta desde las páginas comerciales no reparte autoridad.
- [ ] <!-- seo.blog.related-block --> **Relacionados con nombre**: "Más sobre {tema}" con enlace a la categoría, no "Artículos relacionados".
- [ ] <!-- seo.blog.images --> Portada con alt útil y prioridad de carga solo si es la imagen principal; el resto, carga diferida, con `sizes` y medidas. Las decorativas, con alt vacío.

## 9. Analítica

- [ ] <!-- seo.analytics.evidence-state --> **Estado con evidencia, no "tiene el ID"**: reportá por separado configurado, etiqueta servida en el dominio, evento verificado y contacto conciliado con el CRM. Tener el ID de GA4 solo prueba que existe.
- [ ] <!-- seo.analytics.public-domain --> **Verificar en el dominio público**: `curl` al dominio real buscando el ID de medición. Una etiqueta publicada en una URL que nadie visita cuenta como no instalada.
- [ ] <!-- seo.analytics.lead-events --> **Eventos de contacto en todas las páginas**: cada botón que trae plata (formulario, WhatsApp, agenda, teléfono, mail) dispara su evento, verificado en DebugView. Un botón del footer que mide en una sola página es medición rota.
- [ ] <!-- seo.analytics.resilient-cta --> **El botón funciona antes y después de cargar JavaScript**: el enlace o el formulario apuntan a un destino válido sin JS; el modal es una mejora.
- [ ] <!-- seo.analytics.crm-match --> **Un contacto, un evento**: el navegador mide la intención; el servidor emite el evento de contacto una sola vez, cuando el contacto quedó guardado, con el mismo identificador. Nunca desde los dos lados.
- [ ] <!-- seo.analytics.consent-regions --> **Consentimiento probado por región**: antes y después de aceptar, en un país con ley de cookies y en otro sin ella.
- [ ] <!-- seo.analytics.ai-channel --> **Canal de tráfico de IAs en GA4**: un grupo de canales propio, mirando antes qué referencias de IAs ya te llegan. Por ejemplo `chatgpt\.com|chat\.openai\.com|perplexity\.ai|claude\.ai|gemini\.google\.com|copilot\.microsoft\.com|deepseek\.com|grok\.com|meta\.ai`, más las que aparezcan en tus referencias.
- [ ] <!-- seo.analytics.gtm-access --> **Google Tag Manager publicado no es lo mismo que editable**: anotá qué etiquetas, destinos y consentimiento tiene el contenedor publicado, y por separado si alguien del equipo puede abrirlo y editarlo. No lo saques hasta haber migrado cada etiqueta y verificado que su evento sigue llegando.

## 10. Higiene técnica

- [ ] <!-- seo.tech.canonical-host --> **Un solo host**: `www` y sin `www` resueltos con 301 o 308 al que declara la canonical. Probalo con `curl -sI` a los dos.
- [ ] <!-- seo.tech.https-redirect --> HTTP a HTTPS permanente, directo al destino final.
- [ ] <!-- seo.tech.real-404 --> **404 real**: página propia con salidas, que responde 404 y no 200. Probalo con una ruta inventada.
- [ ] <!-- seo.tech.trailing-slash --> Barra final consistente: `/pagina` y `/pagina/` no sirven las dos 200 sin canonical.
- [ ] <!-- seo.tech.safe-environments --> **Entornos de prueba no indexables** y fuera del sitemap; producción no conserva un `noindex` temporal.
- [ ] <!-- seo.tech.international-routing --> **Sin redirección automática por idioma o IP**: selector rastreable y URL propia por idioma.
- [ ] <!-- seo.tech.image-seo --> Imágenes con nombre corto y descriptivo y alt que describe la imagen en su contexto. Sin relleno de palabras clave.
- [ ] <!-- seo.tech.llms-txt --> **`llms.txt` opcional**, generado desde los mismos datos que el sitemap. No es un estándar ni cuenta como "GEO hecho".
- [ ] <!-- seo.cwv.assets --> **Peso y velocidad**: componente de imagen optimizada en todas las imágenes (en un sitio real, la home pasó de 3,5 MB a 0,65 MB); fuentes en WOFF2; caché larga e `immutable` para imágenes, video y fuentes (y desde ahí, archivo nuevo, nombre nuevo); nada sin uso en `/public`. Meta en datos de campo, percentil 75: LCP de 2,5 s o menos, INP de 200 ms o menos, CLS de 0,1 o menos.
- [ ] <!-- seo.security.headers --> Encabezados mínimos: `X-Content-Type-Options: nosniff`, `Referrer-Policy` y CSP si hay scripts de terceros.

## 11. GEO y AEO: que las IAs puedan leerte y citarte

El SEO técnico hace que la página se indexe. Esta sección hace que ChatGPT, Perplexity, Claude y Google AI Overviews puedan leerla y reutilizarla. No es una estrategia nueva: es el mismo SEO de siempre, con la pregunta en vez de la palabra clave como unidad.

| SEO clásico | SEO de preguntas |
|---|---|
| Palabra clave | Pregunta en lenguaje natural |
| Tema | Grupo de preguntas con la misma intención |
| Subtemas | Repreguntas, que van como H2 o H3 dentro del contenido |

### 11.1 Acceso técnico

- [ ] <!-- seo.aeo.ssr-content --> **El contenido importante está en el HTML inicial**: servicios, preguntas, datos, precios, canonical y robots. Probalo con `curl -s <url> | grep "<frase>"`. Si solo aparece después de hidratar, las IAs pueden no verlo.
- [ ] <!-- seo.aeo.initial-h1 --> **Un `h1` en el HTML de cada página**, incluidas las que abren con un video o una foto sin texto. Si el diseño no muestra título, un `<h1 class="sr-only">` descriptivo.
- [ ] <!-- seo.aeo.robots-retrieval-training --> **robots.txt: búsqueda y entrenamiento son decisiones separadas.** Los rastreadores de búsqueda (`OAI-SearchBot`, `PerplexityBot`, `Claude-SearchBot`) no pueden estar bloqueados, ni por un comodín heredado. Los de entrenamiento (`GPTBot`, `CCBot`, `Google-Extended`) los decide la marca. `ChatGPT-User` y `Perplexity-User` responden a un pedido de una persona y sus dueños avisan que pueden no respetar robots.txt: anotalos, pero no frenan el control. `Google-Extended` no controla tu aparición en Google.
- [ ] <!-- seo.aeo.performance --> Las páginas que venden no son las lentas.

### 11.2 Contenido citable

- [ ] <!-- seo.aeo.topic-clusters --> **Temas, no palabras sueltas**: cada página que vende cubre un grupo de preguntas reales (de "Otras preguntas de los usuarios", del propio asistente de IA y de los mensajes que recibe la marca).
- [ ] <!-- seo.aeo.intent-followups --> **Un H2 por pregunta**, con la respuesta autocontenida en el primer párrafo. Mirá cómo el asistente de IA arma su respuesta al tema: esos subtemas son las repreguntas.
- [ ] <!-- seo.aeo.information-gain --> **Algo que los demás no dicen**: un dato propio, un caso real, un método, una opinión. La página que repite el top 10 no se cita.
- [ ] <!-- seo.aeo.extractable-blocks --> **Bloques que se pueden levantar tal cual**: definición corta, lista de pasos, tabla comparativa, "en resumen" al principio de la sección.
- [ ] <!-- seo.aeo.summary-under-h1 --> **Resumen debajo del H1 en servicios y landings**: 2 o 3 oraciones con qué se ofrece, para quién y en cuánto tiempo, con la marca nombrada. Texto propio de cada página y en el HTML, no dentro de un acordeón ni de una imagen.
- [ ] <!-- seo.aeo.citable-data --> **Datos con fuente y entidades con nombre**: los números salen de datos reales o de fuentes enlazadas. Escribí "Claura", "Buenos Aires", "abogados de inmigración", no "nosotros": las IAs citan entidades, no pronombres.
- [ ] <!-- seo.aeo.freshness --> Fecha de actualización real, visible y en `dateModified`. No se toca sin un cambio de fondo.

### 11.3 Fuera de la web

No frena el lanzamiento, pero sale en el informe como plan con dueño.

- [ ] <!-- seo.offsite.presence-inventory --> **Dónde está la marca y dónde falta**: perfil de Google Business, reseñas, directorios del rubro, YouTube, prensa, comunidades.
- [ ] <!-- seo.offsite.entity-consistency --> **Mismo nombre, descripción y datos de contacto en todos lados**, igual que en el `sameAs` y el `alternateName` del schema. Si las fuentes no coinciden, la IA duda y cita a otro.
- [ ] <!-- seo.offsite.editorial-backlinks --> **Enlaces que existen porque aportan**: nada de comprar enlaces, automatizar altas ni exigir anchors exactos. Lo pago lleva `rel="sponsored"`.

### 11.4 Medición

- [ ] <!-- seo.aeo.zero-click-mentions --> **Menciones sin clic**: al cerrar el control, hacé 5 a 10 preguntas típicas de tu rubro en ChatGPT, Perplexity y Gemini y anotá si aparece la marca y a quién citan. Repetilo al mes. Es el equivalente, para las IAs, de la prueba de resultados enriquecidos.

## Reglas

1. **Nunca inventes datos**: horarios, reseñas, precios, coordenadas, cifras. Lo que falta va a pedidos al cliente.
2. **Cada punto con estado**, y los pendientes con dueño.
3. **Verificá contra el sitio publicado**, no contra el código: el código dice lo que querías, el dominio dice lo que pasa.
4. **Una plantilla se revisa con una URL de cada tipo**, nunca solo con la home.
5. **No cambies una página que ya trae visitas sin mirar antes qué búsquedas la traen**: reescribir una página con tráfico en tanda puede costarte sus clics.

---

Hecha por [Claura](https://claura-ai.com), agencia de marketing con IA. Es la misma lista que corremos en las webs de nuestros clientes. Si querés que te armemos o auditemos la web, mirá [claura-ai.com/agencia-web-profesional](https://claura-ai.com/agencia-web-profesional).
