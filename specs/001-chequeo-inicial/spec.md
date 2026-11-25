# Spec 001 — Chequeo inicial de SEO, arquitectura y diseño

## Contexto y objetivo
El sitio es un conjunto de unas 45 páginas HTML estáticas exportadas de Blogger y publicadas en https://www.contratoempleadahogar.com/. Se necesita un diagnóstico inicial que detecte errores que perjudiquen el posicionamiento. El SEO es la prioridad; arquitectura y diseño se revisan también de forma completa. El chequeo cubre el repositorio y el sitio publicado. Esta spec produce solo un informe, sin correcciones.

## Usuarios / actores
- Propietario del sitio, que decide qué corregir.
- Agente de OpenCode, que ejecuta el chequeo.

## Historias de usuario
- H1: Como propietario quiero un informe priorizado de errores SEO para saber qué corregir primero.
- H2: Como propietario quiero saber qué páginas compiten por la misma intención de búsqueda, para evitar canibalización.
- H3: Como propietario quiero conocer los problemas de arquitectura y diseño del sitio.
- H4: Como propietario quiero saber si lo publicado coincide con el repositorio y si hay URLs con errores.

## Requisitos funcionales (criterios de aceptación en EARS)
- RF-1: EL SISTEMA analizará `index.html`, todos los `p/*.html` y todos los `YYYY/MM/*.html` de `contratoempleadahogar/`.
- RF-2: EL SISTEMA obtendrá los datos del contenido del repositorio y del sitio publicado, solo mediante lectura.
- RF-3: CUANDO una página carezca de `<title>`, `meta description`, `canonical` o `<h1>`, EL SISTEMA lo registrará como hallazgo.
- RF-4: CUANDO dos páginas compartan el mismo `<title>` o la misma `meta description`, EL SISTEMA las registrará como hallazgo.
- RF-5: CUANDO una página tenga más de un `<h1>`, EL SISTEMA lo registrará como hallazgo.
- RF-6: CUANDO una imagen carezca de `alt`, EL SISTEMA la registrará como hallazgo.
- RF-7: CUANDO un enlace interno apunte a un archivo inexistente, EL SISTEMA lo registrará como hallazgo.
- RF-8: CUANDO el JSON-LD de una página sea inválido o no coincida con su contenido visible, EL SISTEMA lo registrará como hallazgo.
- RF-9: EL SISTEMA comprobará si existen los datos de compartición (Open Graph) en cada página.
- RF-10: EL SISTEMA inferirá del contenido la intención de búsqueda de cada página y presentará un mapa página-intención.
- RF-11: CUANDO dos o más páginas tengan la misma intención inferida, EL SISTEMA las registrará como posible canibalización.
- RF-12: EL SISTEMA evaluará la jerarquía de URLs y el enlazado interno, y registrará las páginas sin enlaces entrantes.
- RF-13: EL SISTEMA revisará la arquitectura del sitio: jerarquía de URLs, navegación, enlazado interno, páginas huérfanas, duplicidad de plantillas y coherencia entre `p/` y `YYYY/MM/`.
- RF-14: EL SISTEMA revisará el diseño de cada página: responsive (viewport y breakpoints), contraste, jerarquía de títulos, navegación por teclado, textos alternativos y legibilidad.
- RF-15: EL SISTEMA asignará a cada hallazgo una severidad (alta, media o baja), las páginas afectadas y el área (SEO, arquitectura o diseño).
- RF-16: EL SISTEMA ordenará el informe con los hallazgos de SEO primero, después arquitectura y diseño, y dentro de cada área por severidad descendente.
- RF-17: SI un dato no puede comprobarse con el repositorio ni con lecturas del sitio publicado (indexación, posiciones, Core Web Vitals, contraste calculado, comportamiento táctil), ENTONCES EL SISTEMA lo marcará como "no verificable" en el informe y no lo estimará.
- RF-18: EL SISTEMA entregará un único informe en `specs/001-chequeo-inicial/informe.md`.
- RF-19: EL SISTEMA no modificará ningún archivo de `contratoempleadahogar/`.
- RF-20: EL SISTEMA comprobará el estado HTTP de las 43 URLs del sitio publicado y registrará las que no devuelvan 200, incluidas las redirecciones.
- RF-21: EL SISTEMA comprobará que el `canonical` y el `<title>` publicados de cada URL coinciden con los del repositorio, y registrará las diferencias.
- RF-22: EL SISTEMA comprobará el estado HTTP de los enlaces internos que apunten a URLs fuera del conjunto de 43 páginas, y registrará los que fallen o redirijan.
- RF-23: EL SISTEMA evaluará `robots.txt` y los sitemaps publicados: que no bloqueen contenido relevante, que declaren el sitemap y que listen las mismas URLs que el repositorio.
- RF-24: CUANDO una página utilitaria o sin contenido propio figure en el sitemap, EL SISTEMA lo registrará como hallazgo de indexabilidad.
- RF-25: EL SISTEMA evaluará los encabezados H2 de cada página y registrará los que sean un rótulo genérico sin tema (por ejemplo "Preguntas frecuentes" a secas) o repitan el H1.
- RF-26: EL SISTEMA evaluará los textos de los enlaces internos y registrará los genéricos ("aquí", "ver más") o vacíos.
- RF-27: CUANDO un post trate normativa, plazos, cuantías o ayudas y no enlace a una fuente oficial, EL SISTEMA lo registrará como hallazgo. EL SISTEMA comprobará también la fecha de actualización visible y la autoría en cada post.
- RF-28: CUANDO una página afirme un dato verificable de reputación (puntuación, número de reseñas), EL SISTEMA comprobará si enlaza a la fuente que lo respalda y, si no puede verificarlo, lo marcará como pendiente de confirmación del propietario.
- RF-29: EL SISTEMA evaluará las páginas de ciudad: señales locales (dirección, teléfono, `areaServed`, `LocalBusiness`, perfil de Google Business) y el grado de contenido compartido entre ellas.
- RF-30: EL SISTEMA comprobará en el sitio publicado la redirección de HTTP a HTTPS y de sin `www` a `www`, la respuesta 404 real para rutas inexistentes y el comportamiento con barra final.
- RF-31: EL SISTEMA comprobará que `robots.txt` no bloquea a los rastreadores de búsqueda de IA, y registrará la presencia o ausencia de `llms.txt`.
- RF-32: EL SISTEMA registrará la presencia de eventos de medición (GA4) y el peso del HTML de cada página, sin evaluar Core Web Vitals.

## Diseño, responsive y accesibilidad
Revisión completa del diseño (RF-14), comprobada con el HTML y el CSS del repositorio, sin medir en navegador. Lo que requiera un navegador real se marca como "no verificable" (RF-17).

## SEO
Es el núcleo de la spec. Criterios: RF-3 a RF-12 y RF-20 a RF-32.

## Requisitos no funcionales
- Cada hallazgo debe poder reproducirse con un comando indicado en el informe (`grep` para el repositorio, `curl` para el sitio publicado).
- El informe se redacta en español.
- Las lecturas del sitio publicado son solo peticiones GET o HEAD, sin enviar formularios ni cargar datos.

## Casos límite
- Páginas generadas por Blogger con varios `<h1>` o `canonical` automáticos.
- Páginas sin contenido propio (por ejemplo, calculadoras).
- Archivos con caracteres mal codificados.
- Enlaces a rutas externas o a URLs de Blogger que no están en el repositorio.
- Páginas con estilos incrustados de Blogger o elementos que dependen de scripts externos.
- URLs de Blogger que redirigen (301) a otra página.
- Sitio publicado distinto del repositorio por cambios posteriores a la exportación.
- La similitud entre páginas de ciudad incluye texto compartido de menú y pie, por lo que es un indicador orientativo.

## Fuera de alcance
- Corregir errores. Se hará en specs posteriores.
- Rediseñar o proponer nuevas interfaces.
- Medir posiciones, indexación, tráfico o Core Web Vitals.
- Herramientas externas de SEO (Search Console, Lighthouse, rastreadores de terceros).
- Comprobar enlaces a dominios externos.
- Investigar palabras clave con herramientas externas.
- Reescribir o ampliar contenido.

## Criterios de finalización
- Los RF-1 a RF-32 están cubiertos en el informe.
- Cada hallazgo incluye su comprobación y se informa del resultado.
- Ningún archivo de `contratoempleadahogar/` ha cambiado.

## Dudas abiertas
Ninguna.
