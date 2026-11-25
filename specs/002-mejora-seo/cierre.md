# Cierre — Spec 002 (Mejora del SEO)

Rama: `feature/spec-002-mejora-seo`. Cambios aplicados solo en el repositorio (43 archivos de `contratoempleadahogar/`). No hay commit ni push. El propietario publica los ficheros en Blogger.

## Estado por requisito

| RF | Estado | Verificación en el repositorio |
|---|---|---|
| RF-1, RF-2 | Hecho | `og:image`, `twitter:image`, ancho 993, alto 630 y `alt` en las 43 páginas (una sola vez por página) |
| RF-3 | Hecho | un único `og:title`, `og:url` y `og:description` por página; sin `meta description` duplicada |
| RF-4 | Hecho | `og:title` de la home = `<title>` de la home |
| RF-5 | Hecho | JSON-LD en 13 páginas indexables que no lo tenían. Quedan sin JSON-LD solo las 4 utilitarias (`ficha-de-alta`, `incidencias-del-mes`, `iniciar-contratacion`, `formulario-prl`) |
| RF-6 | Hecho | `dateModified` en todos los `Article`. Los 4 posts nuevos usan el `lastmod` del sitemap publicado |
| RF-7 | Hecho | `@id` de organización único en todos los `publisher`, `provider` y `worksFor`; la home lista los dos teléfonos |
| RF-8 | Hecho | 5 respuestas de `FAQPage` alineadas con el texto visible; entidades HTML decodificadas en 25 bloques de JSON-LD. 0 preguntas o respuestas sin coincidencia |
| RF-9, RF-10 | Hecho | 0 `aggregateRating`/`Review`; el texto "42 reseñas" sigue en las mismas 15 páginas |
| RF-11, RF-12 | Aplazado | requieren ajustes del panel de Blogger (spec 003) |
| RF-13, RF-14 | Hecho | enlaces cruzados descriptivos en todos los pares; se mantienen los títulos y H1 actuales, que ya distinguen la intención (aprobado por el propietario) |
| RF-15 | Hecho | `git diff --name-status`: solo modificados, sin renombrados ni borrados |
| RF-16 | Hecho | 0 referencias a `/2026/07/blog-post.html`; `p/blog` enlaza a `/p/app-nominas-empleada-hogar.html` |
| RF-17 | Hecho (17 de 18) | cada post enlaza a ≥1 página de servicio y a ≥2 contenidos relacionados. Excepción: `formulario-prl` (formulario, pendiente de `noindex`) |
| RF-18 a RF-21 | Hecho | `informacion-para-ia`: título, descripción, JSON-LD, 9 cifras con fuente BOE enlazada y 13 posts resumidos con enlace |
| RF-22 | Hecho | 0 H2 "Preguntas frecuentes" sin tema (14 corregidos; los `id="faq"` se mantienen) |
| RF-23 | Hecho | bloque "Fuentes oficiales" con enlaces con respuesta 200 en `finiquito`, `sin-contrato`, `prevencion` y `ayudas-para-contratar` (aprobado por el propietario) |
| RF-24 | Hecho (17 de 18) | fecha y autor visibles en todos los posts salvo `formulario-prl` |
| RF-25, RF-26 | Hecho | `Service` con `areaServed` y `Organization` con móvil (principal) y fijo en las 6 ciudades |
| RF-27 | Hecho | sin dirección, horario ni perfil de Google Business |
| RF-28 | Hecho | `tel:34926112851` → `tel:+34926112851` (1 enlace, en `opiniones-sobre-nuestro-servicio-de`) |
| RF-29 | Hecho | `lang='es'` en las 43 páginas |
| RF-30 | Hecho | `phone_click` y `email_click` por delegación de eventos en las 43 páginas |
| RF-31 | Hecho | los 254 enlaces `wa.me` quedan cubiertos por el listener delegado existente de `whatsapp_click` (ninguno sin cubrir) |
| RF-32, RF-33 | Hecho | 0 cambios en `whatsapp_click` y `generador_contrato` |
| RF-34 | Hecho | cambios de `<title>` solo en `informacion-para-ia`; ningún H1 modificado |
| RF-35 | Hecho | cambios de cabecera en las 43 páginas |

## Comprobaciones finales
- 53 bloques JSON-LD, 0 inválidos, 0 con entidades HTML.
- 0 enlaces internos rotos; el recuento de `<div>` abiertos y cerrados es igual que en el original en las 43 páginas.
- Codificación UTF-8 sin BOM y saltos CRLF conservados.

## Hallazgos del informe 001

| Hallazgo | Antes | Después |
|---|---|---|
| S3 enlace con 301 | 1 | 0 |
| S5 Open Graph | `og:image` en 1/43; `og:title` duplicado en 43 | `og:image` en 43/43; 0 duplicados |
| S7 JSON-LD | 17 páginas sin JSON-LD | 4 (utilitarias) |
| S8 canibalización | sin enlaces cruzados | enlaces cruzados descriptivos; títulos se mantienen |
| S11 H2 genérico | 14 | 0 |
| S12 fuentes | 5 posts sin fuente oficial | 0 (`formulario-prl` queda fuera por ser un formulario) |
| D1 `lang` | 0/43 | 43/43 |
| P4 utilitarias en sitemap | 4 | 4 (aplazado) |
| P8 medición | sin eventos de teléfono ni correo | `phone_click` y `email_click` |
| datePublished | 1/18 | 1/18 (aplazado a spec 003) |

## Aprobado por el propietario

- Fuentes oficiales de los 4 posts (RF-23) y mantener los títulos y H1 de los pares de canibalización (RF-13).
- Pendiente de confirmar: la autoría de Petri Borondo Martín en los 4 posts nuevos.

## Cambios fuera de lo literal de la spec
- Los 4 `Article` nuevos (`renta` ×2, `ayudas-para-contratar`, `formacion-preventiva`) llevan como autora a Petri Borondo Martín, como el resto de posts. Conviene confirmarlo.
- `p/informacion-para-ia` pasa a titularse "Datos clave del empleo de hogar 2026".
- La home publica dos `contactPoint` (móvil y fijo).
- Se añadió `twitter:card` y `twitter:image` en las páginas que no los tenían.

## Limitaciones y publicación
- **Plantilla de Blogger:** parte de la cabecera (Open Graph, `lang`) puede proceder del tema. Si tras publicar no aparece en producción, se anotará "no publicada por la plantilla" y pasa a la spec 003.
- **Aplazado a la spec 003:** `noindex` de las 4 utilitarias y su exclusión del sitemap, imagen Open Graph propia 1200×630, dirección, horario y perfil de Google Business, contenido propio por ciudad y `datePublished`.
- **T7.4 y T7.5** se hacen cuando el propietario publique: verificar cada requisito en producción con `curl` y repetir S3, S5, S7, S8, S11, S12, D1 y P8.
