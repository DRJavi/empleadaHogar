# Spec 003 — Revisión SEO integral: metadatos, marca, señales locales, Core Web Vitals e IA

## Contexto y objetivo
La spec 002 (`specs/002-mejora-seo/`) cerró Open Graph, JSON-LD, enlaces cruzados, fuentes oficiales y medición. Dejó pendientes los metadatos básicos del informe 001 (bloque A), la unificación de marca, lo que la plantilla de Blogger no publicó, los datos del negocio y el contenido por ciudad. Esta spec los cierra y añade una revisión de Core Web Vitals y de visibilidad en buscadores con IA. El objetivo es mejorar el posicionamiento sin cambiar ninguna URL publicada.

## Usuarios / actores
- Propietario: aporta los datos del negocio y publica en Blogger.
- Agente de OpenCode: edita el repositorio, mide y verifica.

## Historias de usuario
- H1: Como propietario quiero títulos, descripciones y H1 correctos en todas las páginas, para aparecer bien en los resultados.
- H2: Como propietario quiero un nombre de marca coherente, para que buscadores e IAs asocien el sitio a una sola entidad.
- H3: Como propietario quiero que mi negocio local esté bien declarado, para aparecer en búsquedas locales.
- H4: Como propietario quiero conocer y mejorar los Core Web Vitals, para que el sitio cargue bien en móvil.
- H5: Como propietario quiero que las IAs puedan rastrear y citar mis páginas.

## Requisitos funcionales (criterios de aceptación en EARS)

### Metadatos básicos (bloque A del informe 001)
- RF-1: EL SISTEMA dará una `meta description` propia a cada página indexable que hoy no la tiene (S1).
- RF-2: EL SISTEMA dará exactamente un `<h1>` a cada página que hoy no lo tiene (S2).
- RF-3: EL SISTEMA acortará a 60 caracteres o menos, sufijo incluido, el `<title>` de cada página que hoy lo supera (S4).
- RF-4: SI acortar un título lo empeora, ENTONCES EL SISTEMA admitirá hasta 70 caracteres y anotará la excepción en el cierre.
- RF-5: EL SISTEMA dará un título descriptivo a las páginas con título genérico de menos de 40 caracteres (S9).
- RF-6: EL SISTEMA corregirá «Politica» por «Política» en el título de la página de privacidad (S10).
- RF-7: EL SISTEMA mantendrá el `<title>` único en cada página y el `canonical` igual a la URL de su página.
- RF-8: EL SISTEMA no cambiará ninguna URL publicada.

### Marca (S6)
- RF-9: EL SISTEMA usará «AsisteHogar» como nombre de la marca y del negocio en títulos, `og:site_name` y datos estructurados.
- RF-10: EL SISTEMA declarará «Contrato Empleada de Hogar» como `alternateName` del negocio y de la organización.
- RF-11: SI una página menciona un nombre de marca distinto de los dos anteriores, ENTONCES EL SISTEMA lo corregirá o lo anotará en el cierre.

### Open Graph y plantilla
- RF-12: EL SISTEMA mantendrá la imagen actual de 993×630 como imagen para compartir.
- RF-13: SI `og:image:width`, `og:image:height` o el `alt` completo no se publican por la plantilla, ENTONCES EL SISTEMA lo documentará como limitación aceptada.

### Páginas utilitarias e indexación
- RF-14: EL SISTEMA documentará que `ficha-de-alta`, `incidencias-del-mes`, `iniciar-contratacion` y `formulario-prl` siguen en el sitemap aunque devuelven `noindex`, con su causa.
- RF-15: EL SISTEMA no excluirá del sitemap ni eliminará esas cuatro páginas.

### Señales locales
- RF-16: EL SISTEMA añadirá `LocalBusiness` al JSON-LD de la home, y solo de la home, con estos datos del propietario:
  - Nombre: AsisteHogar.
  - Dirección: Ronda de Santo Domingo 2, 13270 Almagro, Ciudad Real.
  - Horario: lunes a viernes, de 10:00 a 14:00 y de 17:00 a 20:00.
  - Teléfonos: `+34685153360` (principal) y `+34926112851`.
  - Enlace al perfil de Google Business, sin parámetros de seguimiento.
  - Ámbito de servicio (`areaServed`): España.
- RF-16b: EL SISTEMA declarará en `areaServed` del `LocalBusiness` de la home, además de España, las seis ciudades con página propia: Madrid, Barcelona, Bilbao, Málaga, Sevilla y Valencia.
- RF-16c: EL SISTEMA mostrará Madrid como primera ciudad y con indicación visible de zona principal en el bloque de zonas de la home, con enlace a su página.
- RF-16d: EL SISTEMA mantendrá visibles las seis ciudades, cada una con enlace a su página, en el bloque de zonas de la home.
- RF-17: EL SISTEMA no declarará horario para sábados y domingos, porque el negocio no trabaja esos días (confirmado por el propietario).
- RF-18: EL SISTEMA mostrará la dirección y el horario como texto visible en la home o en la página de contacto.
- RF-19: SI la dirección o el horario del JSON-LD difieren del texto visible, ENTONCES EL SISTEMA corregirá el JSON-LD para que coincida.
- RF-20: EL SISTEMA no añadirá `LocalBusiness` ni dirección en las 6 páginas de ciudad, que mantienen su `areaServed`.
- RF-21: DONDE el propietario aporte datos propios y verificables de una ciudad, EL SISTEMA los incorporará a su página.
- RF-21b: EL SISTEMA mostrará como texto visible, en la home, la cifra «más de 1.000 contratos gestionados» confirmada por el propietario, sin marcarla como `aggregateRating` ni `Review`.
- RF-22: SI un dato local no consta, ENTONCES EL SISTEMA no lo incluirá ni lo inventará.
- RF-23: EL SISTEMA citará con fuente oficial y año toda cifra o plazo nuevo que añada.

### Core Web Vitals
- RF-24: EL SISTEMA medirá con PageSpeed Insights (solo lectura) la home, un post, una página de ciudad y una calculadora, en móvil y escritorio.
- RF-25: EL SISTEMA documentará LCP, CLS e INP de cada una frente a los umbrales «bueno» de Google.
- RF-26: DONDE un valor supere el umbral y la causa se pueda corregir desde el HTML de las páginas, EL SISTEMA aplicará la corrección en todas las páginas afectadas.
- RF-27: SI la causa está en el tema de Blogger, ENTONCES EL SISTEMA la documentará como limitación y no la corregirá.
- RF-28: EL SISTEMA repetirá la medición tras las correcciones y la comparará con la inicial.

### Visibilidad en IA (AEO/GEO)
- RF-29: EL SISTEMA auditará con la skill `ai-seo` si `robots.txt`, los sitemaps y el contenido permiten a los rastreadores de IA acceder y citar.
- RF-30: EL SISTEMA propondrá, sin aplicarlo, cualquier cambio que dependa del panel de Blogger o de un archivo en la raíz del dominio.
- RF-31: EL SISTEMA revisará que las páginas clave contengan respuestas directas y citables, con fuente y fecha.
- RF-32: EL SISTEMA no añadirá afirmaciones nuevas sin fuente oficial.

### Medición y verificación
- RF-33: EL SISTEMA documentará que el disparo real de `phone_click`, `email_click` y `whatsapp_click` en GA4 queda pendiente de verificación manual por el propietario.
- RF-34: EL SISTEMA dejará la confirmación de indexación y de datos estructurados en Search Console como tarea del propietario.

### Límites
- RF-35: EL SISTEMA aplicará los cambios de cabecera compartida en las 43 páginas o en ninguna.
- RF-36: EL SISTEMA no añadirá `aggregateRating` ni `Review` y no modificará el texto «5/5 en Google con 42 reseñas».

## Diseño, responsive y accesibilidad
Solo el `<h1>` (RF-2) y la medición de Core Web Vitals. Si se corrigen imágenes o scripts, se mantiene el `alt` en todas las imágenes y el aspecto actual.

## SEO
Núcleo de la spec: RF-1 a RF-32. Cada página conserva título único, `meta description`, un solo `<h1>`, `canonical` y JSON-LD válido y coherente con el contenido visible (principios 8 y 9 de la constitución).

## Requisitos no funcionales
- Cada cambio se verifica con `grep`, `curl` o PageSpeed (solo lectura) y se informa en un cierre.
- Cifras legales y fiscales con fuente oficial y año (principio 7).
- Sin dependencias nuevas ni cambios de estructura.
- Las lecturas del sitio publicado son solo peticiones GET o HEAD.

## Casos límite
- Las cifras del informe 001 (10 páginas sin descripción, 5 sin H1, 31 títulos largos) se recuentan al empezar, porque la spec 002 ya modificó alguna.
- Títulos de 60 caracteres que pierden su palabra clave al acortarse.
- Páginas utilitarias con `noindex` que reciben `meta description`.
- Valores de Core Web Vitals que varían entre mediciones.
- Cabecera que Blogger genera y no se puede editar.
- Horario partido en dos tramos diarios.

## Fuera de alcance
- Imagen Open Graph nueva de 1200×630.
- Verificar o cambiar «5/5 en Google con 42 reseñas» (S13).
- `datePublished` real de los 18 posts.
- Reducir el solapamiento de contenido entre páginas de ciudad.
- Revisión de las cifras de cada ciudad frente a fuentes oficiales.
- Intención de búsqueda y SERP (`seo-sxo`).
- Cambios de URL, redirecciones, fusión de páginas y excluir las utilitarias del sitemap.
- Rediseño visual, formularios, `llms.txt` publicado y cambios en el panel de Blogger.
- Publicar en Blogger o hacer commit sin petición expresa.

## Criterios de finalización
- RF-1 a RF-36 verificados en el repositorio y, tras publicar el propietario, en producción. Cada uno queda anotado como «publicado», «no publicado por la plantilla», «aplazado» o «pendiente de datos del propietario».
- Se repiten S1, S2, S4, S6, S9 y S10 del informe 001 y se anota su estado.
- 0 enlaces internos rotos, 0 JSON-LD inválidos, 0 HTML sin cerrar y ninguna URL cambiada.

## Dudas abiertas
- Resuelta: el negocio no trabaja sábados ni domingos (RF-17).
- Resuelta en parte (RF-21): el propietario no tiene datos propios por ciudad ni conoce particularidades provinciales. Atiende en toda España, con interés prioritario en Madrid, y tiene más de 1000 contratos gestionados en total (no por ciudad). Los testimonios son solo las reseñas ya publicadas. RF-21 queda «pendiente de datos del propietario» en las ciudades distintas de Madrid.
- Resuelta: el propietario aprueba publicar «más de 1.000 contratos gestionados» como texto visible (RF-21b) y declarar `areaServed` de España en `LocalBusiness` (RF-16).
