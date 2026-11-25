# Spec 001 — Chequeo inicial de SEO, arquitectura y diseño

## Contexto y objetivo
El sitio es un conjunto de unas 45 páginas HTML estáticas exportadas de Blogger. Se necesita un diagnóstico inicial que detecte errores que perjudiquen el posicionamiento. El SEO es la prioridad; arquitectura y diseño se revisan también de forma completa. Esta spec produce solo un informe, sin correcciones.

## Usuarios / actores
- Propietario del sitio, que decide qué corregir.
- Agente de OpenCode, que ejecuta el chequeo.

## Historias de usuario
- H1: Como propietario quiero un informe priorizado de errores SEO para saber qué corregir primero.
- H2: Como propietario quiero saber qué páginas compiten por la misma intención de búsqueda, para evitar canibalización.
- H3: Como propietario quiero conocer los problemas de arquitectura y diseño del sitio.

## Requisitos funcionales (criterios de aceptación en EARS)
- RF-1: EL SISTEMA analizará `index.html`, todos los `p/*.html` y todos los `YYYY/MM/*.html` de `contratoempleadahogar/`.
- RF-2: EL SISTEMA obtendrá los datos solo del contenido del repositorio.
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
- RF-17: SI un dato no puede comprobarse con el repositorio (indexación, posiciones, Core Web Vitals, estado HTTP real, contraste calculado, comportamiento táctil, `robots.txt` y sitemap), ENTONCES EL SISTEMA lo marcará como "no verificable" en el informe y no lo estimará.
- RF-18: EL SISTEMA entregará un único informe en `specs/001-chequeo-inicial/informe.md`.
- RF-19: EL SISTEMA no modificará ningún archivo de `contratoempleadahogar/`.

## Diseño, responsive y accesibilidad
Revisión completa del diseño (RF-14), comprobada con el HTML y el CSS del repositorio, sin medir en navegador. Lo que requiera un navegador real se marca como "no verificable" (RF-17).

## SEO
Es el núcleo de la spec. Criterios: RF-3 a RF-12.

## Requisitos no funcionales
- Cada hallazgo debe poder reproducirse con un `grep` indicado en el informe.
- El informe se redacta en español.

## Casos límite
- Páginas generadas por Blogger con varios `<h1>` o `canonical` automáticos.
- Páginas sin contenido propio (por ejemplo, calculadoras).
- Archivos con caracteres mal codificados.
- Enlaces a rutas externas o a URLs de Blogger que no están en el repositorio.
- Páginas con estilos incrustados de Blogger o elementos que dependen de scripts externos.

## Fuera de alcance
- Corregir errores. Se hará en specs posteriores.
- Rediseñar o proponer nuevas interfaces.
- Medir posiciones, indexación, tráfico o Core Web Vitals.
- Comprobar el sitio publicado.
- `robots.txt` y sitemap: no se evalúan, porque requieren el sitio publicado.
- Investigar palabras clave con herramientas externas.
- Reescribir o ampliar contenido.

## Criterios de finalización
- Los RF-1 a RF-19 están cubiertos en el informe.
- Cada hallazgo incluye su comprobación con `grep` y se informa del resultado.
- Ningún archivo de `contratoempleadahogar/` ha cambiado.

## Dudas abiertas
Ninguna.
