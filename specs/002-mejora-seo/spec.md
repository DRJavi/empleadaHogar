# Spec 002 — Mejora del SEO: Open Graph, datos estructurados, indexación, contenido y medición

## Contexto y objetivo
La spec 001 (`specs/001-chequeo-inicial/`) diagnosticó el sitio y produjo `informe.md`. Esta spec corrige los hallazgos de los bloques de Open Graph y JSON-LD (S5, S7), indexación y arquitectura (P4, S8, S3, A1, A2), contenido y señales locales (S11, S12, S14) y diseño y medición (D1, P8). El objetivo es mejorar el posicionamiento y la forma en que se comparten y citan las páginas, sin cambiar ninguna URL publicada. Lo que depende de datos que el propietario aún no tiene queda aplazado a una spec posterior (ver "Fuera de alcance").

## Usuarios / actores
- Propietario del sitio: aprueba las fuentes oficiales, configura Blogger y publica.
- Agente de OpenCode: edita los archivos del repositorio y verifica.

## Historias de usuario
- H1: Como propietario quiero que mis páginas se vean con imagen al compartirse en redes para atraer más visitas.
- H2: Como propietario quiero datos estructurados completos y correctos para que buscadores e IAs entiendan mi contenido.
- H3: Como propietario quiero que solo se indexen las páginas que aportan valor, para no diluir el posicionamiento.
- H4: Como propietario quiero que cada página compita por una intención de búsqueda propia, para evitar canibalización.
- H5: Como propietario quiero que los contenidos sobre normativa citen fuentes oficiales, para generar confianza.
- H6: Como propietario quiero medir los contactos por WhatsApp, teléfono y correo.

## Requisitos funcionales (criterios de aceptación en EARS)

### Open Graph y datos estructurados
- RF-1: EL SISTEMA declarará la imagen que ya usa `2026/03/contrato-empleada-hogar` como imagen para compartir en las 43 páginas.
- RF-2: EL SISTEMA declarará ancho, alto y texto alternativo de la imagen para compartir en cada página, con las medidas reales del archivo (993×630 píxeles).
- RF-3: EL SISTEMA dejará un único `og:title`, un único `og:url` y un único `og:description` en cada página.
- RF-4: EL SISTEMA hará que el `og:title` de la home coincida con el `<title>` de la home.
- RF-5: EL SISTEMA añadirá JSON-LD válido a toda página indexable que hoy no lo tenga, con el tipo adecuado (`Article`, `CollectionPage`, `ContactPage` o `WebPage`) y `BreadcrumbList`.
- RF-6: EL SISTEMA añadirá `dateModified` al JSON-LD de los posts que no lo tengan, con la fecha de modificación del sitemap publicado.
- RF-7: EL SISTEMA referenciará a la organización con un único `@id` común a todas las páginas, e incluirá en cada página que lo referencie un nodo mínimo (`@id`, `@type`, `name`, `url`).
- RF-8: SI una página emite `FAQPage`, ENTONCES EL SISTEMA comprobará que cada pregunta y respuesta del JSON-LD figura con texto idéntico en el contenido visible.
- RF-9: EL SISTEMA no añadirá `aggregateRating` ni `Review` en ninguna página.
- RF-10: EL SISTEMA no modificará los textos de reseñas ni de puntuación que ya existan en las páginas.

### Indexación y arquitectura
- RF-11 y RF-12 (`noindex` de las 4 páginas utilitarias y su verificación): aplazados a la spec 003, porque requieren ajustes del panel de Blogger a los que el propietario no accede.
- RF-13: EL SISTEMA asignará una intención de búsqueda propia y distinta a cada página de los pares de posible canibalización del informe 001 (S8), reflejada en su título, su H1 y su introducción.
- RF-14: EL SISTEMA enlazará cada página de un par de canibalización con la otra, con un texto de enlace descriptivo que indique su enfoque.
- RF-15: EL SISTEMA no cambiará ni eliminará ninguna URL publicada.
- RF-16: EL SISTEMA hará que `p/blog` enlace directamente a `/p/app-nominas-empleada-hogar.html` en lugar de a la URL que redirige.
- RF-17: EL SISTEMA hará que cada post enlace al menos a una página de servicio y a dos contenidos relacionados.

### Página `informacion-para-ia`
- RF-18: EL SISTEMA mantendrá `p/informacion-para-ia` como página indexable y le dará un título y una `meta description` propios.
- RF-19: EL SISTEMA añadirá JSON-LD válido a `p/informacion-para-ia`.
- RF-20: EL SISTEMA enlazará cada cifra normativa de `p/informacion-para-ia` a su fuente oficial.
- RF-21: EL SISTEMA resumirá en `p/informacion-para-ia` el contenido de los posts con un enlace al post correspondiente, sin repetir su texto completo.

### Contenido y señales locales
- RF-22: EL SISTEMA hará que el H2 "Preguntas frecuentes" de las 14 páginas afectadas incluya el tema de la página.
- RF-23: EL SISTEMA propondrá al propietario una fuente oficial para cada post sobre normativa, cuantías, multas o ayudas que hoy no enlace a ninguna, y la añadirá solo tras su aprobación.
- RF-24: EL SISTEMA mostrará fecha de actualización visible y autor en todos los posts.
- RF-25: EL SISTEMA publicará en el JSON-LD de las páginas de ciudad el teléfono móvil como contacto principal y el teléfono fijo como segundo contacto, con el formato internacional (`+34…`).
- RF-26: EL SISTEMA declarará el `areaServed` de cada página de ciudad con su ciudad o región.
- RF-27: SI un dato del negocio no consta, ENTONCES EL SISTEMA no lo incluirá ni lo inventará.
- RF-28: EL SISTEMA corregirá el enlace `tel:` que no lleva prefijo internacional.

### Diseño y medición
- RF-29: EL SISTEMA declarará el idioma español en las 43 páginas.
- RF-30: EL SISTEMA hará que cada enlace `tel:` dispare el evento `phone_click` y cada enlace `mailto:` dispare `email_click`, con `event_category: Conversion`, `event_label` (ubicación del botón) y `page_path`.
- RF-31: EL SISTEMA comprobará que los enlaces `wa.me` llaman al evento `whatsapp_click` existente y registrará los que no lo hagan.
- RF-32: DONDE ya exista el evento en un botón, EL SISTEMA no lo duplicará.
- RF-33: EL SISTEMA no eliminará ni renombrará ningún evento de medición existente.

### Límites
- RF-34: EL SISTEMA no modificará título, meta description, H1 ni nombre de marca de ninguna página que no pertenezca a un par de canibalización ni sea `p/informacion-para-ia`.
- RF-35: EL SISTEMA aplicará los cambios de cabecera compartida en las 43 páginas o en ninguna.

## Diseño, responsive y accesibilidad
Solo se exige el idioma de la página (RF-29). El resto del diseño queda fuera de esta spec.

## SEO
Es el núcleo de la spec. Criterios: RF-1 a RF-28.

## Requisitos no funcionales
- Cada cambio debe poder comprobarse con un `grep` o `curl` indicado en el informe de cierre.
- Los datos legales y fiscales se citan con fuente oficial y año, según la constitución (principio 7).
- No se añaden dependencias ni se cambia la estructura de carpetas.
- Las lecturas del sitio publicado son solo peticiones GET o HEAD.

## Casos límite
- Páginas que ya llevan JSON-LD parcial, sin `dateModified`.
- Posts que enlazan a fuentes oficiales por varios dominios.
- Páginas de ciudad con contenido muy parecido y sin dirección ni horario disponibles.
- Botones de contacto generados por scripts externos.
- Páginas con `og:title` o `og:url` duplicados de Blogger y del código personalizado.
- La imagen para compartir mide 993×630 píxeles, no 1200×630.

## Fuera de alcance
- Metadatos básicos de otras páginas: títulos de más de 60 caracteres, meta description ausente, H1 ausente, marca única y errata "Politica" (bloque A del informe 001). Irán en una spec posterior.
- Verificación de la afirmación "5/5 en Google con 42 reseñas": se deja tal cual.
- Cambio de URL, redirecciones nuevas o fusión de páginas.
- Publicar los cambios en Blogger (lo hace el propietario con los ficheros), configurar el panel de Blogger y medir el efecto en Search Console.
- Si una etiqueta de `<head>` (Open Graph, `lang`) no aparece en producción tras publicar, se anota "no publicada por la plantilla" y pasa a la spec 003.
- Core Web Vitals, rediseño visual y `llms.txt`.
- Medición del envío de formularios.
- **Aplazado a una spec posterior:**
  - `noindex` de `p/ficha-de-alta`, `p/incidencias-del-mes`, `p/iniciar-contratacion` y `2026/05/formulario-prl` y su verificación en el sitemap (RF-11 y RF-12).
  - Sustituir la imagen común por una diseñada con la marca (1200×630), conservando la del contrato solo en su post.
  - Dirección, horario y perfil de Google Business, y el marcado `LocalBusiness` completo.
  - Contenido propio, real y verificable por ciudad, y el límite de solapamiento del 25 % entre ciudades.
  - `datePublished` real en los 18 posts.

## Criterios de finalización
- Los RF-1 a RF-35, salvo RF-11 y RF-12, están verificados con `grep` en el repositorio y, tras la publicación por el propietario, con `curl` en el sitio publicado. Se anota cada uno como "publicado", "no publicado por la plantilla" o "aplazado".
- Los hallazgos S3, S5, S7, S8, S11, S12, D1 y P8 del informe 001 se repiten y se anota su estado (resuelto, pendiente o con limitación).
- Ningún enlace interno roto ni HTML sin cerrar en los archivos modificados.
- Ninguna URL publicada ha cambiado.

## Dudas abiertas
- [NECESITA ACLARACIÓN: aprobación del propietario de las fuentes oficiales que se propongan para `finiquito`, `empleada-de-hogar-sin-contrato-en-2026`, `prevencion-riesgos-laborales-empleada-hogar-2026` y `ayudas-para-contratar-empleada-de-hogar` (RF-23). No bloquea el resto.]
