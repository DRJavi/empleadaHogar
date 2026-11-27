# Cierre — Spec 003 (Revisión SEO integral)

Rama: `feature/spec-003-revision-seo-integral`. Cambios solo en el repositorio. Sin commit ni push. Estado a 27/11/2026, pendiente de que el propietario publique en Blogger y de la verificación en producción (T7.4 y T7.5).

## Hallazgo importante: el repositorio estaba desactualizado respecto a producción

Antes de aplicar el bloque A se comparó el repositorio (HEAD) con las 43 páginas publicadas. En 12 páginas el propietario ya había corregido cosas en Blogger después de la última exportación:

- Las 5 páginas sin `<h1>` (S2) ya tienen `<h1>` en producción: `blog`, `como-funciona` («Cómo funciona: tres pasos y listo»), `aviso-legal`, `politica-de-privacidad` y el post `el-sepe-activa-la-formacion-preventiva`.
- Las páginas sin `meta description` (S1) ya la tienen en producción: `renta-2025-para-empleadas-de-hogar`, `renta-2025-si-tienes-empleada-de-hogar`, `aviso-legal`, `politica-de-cookies`, `politica-de-privacidad` y las 4 utilitarias.
- «Politica de privacidad» (S10) ya está corregida en el título de producción.

Qué se hizo: el repositorio se alinea con producción (se copian los H1 y las descripciones publicados) en lugar de aplicar las propuestas propias para esas páginas. El resto de títulos publicados coincide con el repositorio, así que las propuestas de título sí se aplican.

Límite de esta comprobación: solo se compararon título, `<h1>` y `meta description`. No se ha comparado el resto del contenido, que puede haber cambiado sin que el repositorio lo refleje.

## Línea base (T0.2)

| Hallazgo | Informe 001 | Repositorio al empezar | Producción al empezar |
|---|---|---|---|
| S1 sin `meta description` | 10 | 9 | 0 |
| S2 sin `<h1>` | 5 | 5 | 0 |
| S4 `<title>` > 60 caracteres | 31 | 30 | 30 |
| S6 `og:site_name` | solo en la home, con descriptor | igual | igual |
| S9 título < 40 caracteres | 7 | 6 | 6 |
| S10 «Politica» | 1 | 1 | 0 |

## Resultado final (repositorio)

| Hallazgo | Antes | Después |
|---|---|---|
| S1 sin `meta description` | 9 | 0 (las 4 utilitarias y 5 más, copiadas de producción) |
| S2 sin `<h1>` | 5 | 0: las 43 páginas tienen exactamente un `<h1>` |
| S4 título > 60 | 30 | 0 (máximo 59) |
| S6 `og:site_name` | 1 de 43 | 43 de 43 = «AsisteHogar» |
| S9 título < 40 | 6 | 3 (las utilitarias con `noindex`, que no se tocan) |
| S10 «Politica» | 1 | 0 |

## Estado por requisito

| RF | Estado | Verificación |
|---|---|---|
| RF-1 | Hecho | 0 páginas indexables sin `meta description`; las descripciones son las que ya estaban publicadas |
| RF-2 | Hecho | 43 de 43 páginas con exactamente un `<h1>`; los 5 H1 son los de producción |
| RF-3, RF-4 | Hecho | 33 títulos cambiados; 0 de más de 60 caracteres; sin excepciones; el `og:title` de la home sigue igual al `<title>` |
| RF-5 | Hecho | `aviso-legal`, `politica-de-cookies` y `politica-de-privacidad` ya superan los 40 caracteres; las 3 utilitarias (`noindex`) no se tocan |
| RF-6 | Hecho | «Política» en título, `og:title`, JSON-LD y texto de la página de privacidad |
| RF-7, RF-8 | Hecho | 0 títulos duplicados; 43 `canonical`; `git diff --name-status` sin borrados ni renombrados |
| RF-9 | Hecho | `og:site_name` = «AsisteHogar» en las 43 páginas |
| RF-10 | Hecho (home) | `alternateName` de la organización = «Contrato Empleada de Hogar» y «AsisteHogar Servicio Doméstico»; el `WebSite` ya lo tenía |
| RF-11 | Hecho | sin otras variantes de marca; `formulario-prl` (bilingüe, `noindex`) usa «AsisteHogar» |
| RF-12, RF-13 | Hecho (documentado) | ver «Limitaciones» |
| RF-14, RF-15 | Hecho (documentado) | las 4 utilitarias devuelven `X-Robots-Tag: noindex` y siguen en `sitemap-pages.xml` (3) y `sitemap.xml` (1) |
| RF-16, RF-16b | Hecho | `LocalBusiness` en la home (la organización pasa a `["Organization","LocalBusiness"]`) con dirección, `geo`, `hasMap`, horario de lunes a viernes en dos tramos, `areaServed` de España y las 6 ciudades y los dos teléfonos |
| RF-16c, RF-16d | Hecho | Madrid va la primera con la etiqueta «Zona principal»; las 6 ciudades siguen con enlace; comprobado con captura |
| RF-17 | Hecho | sin horario de sábado ni domingo |
| RF-18, RF-19 | Hecho | `p/contacto` muestra dirección y horario, con enlace a Google Maps; coincide con el JSON-LD; el aviso legal y la política de privacidad ya llevaban la misma dirección |
| RF-20 | Hecho | 0 `LocalBusiness` y 0 `streetAddress` en las 6 ciudades |
| RF-21 | Pendiente de datos del propietario | sin datos propios por ciudad |
| RF-21b | Hecho | la home dice «con más de 1.000 contratos gestionados» (sección «Solo trabajamos con empleo doméstico») |
| RF-22, RF-23 | Hecho | no se añadió ninguna cifra o plazo nuevo salvo los 1.000 contratos, aportados por el propietario |
| RF-24, RF-25 | Hecho | ver «Core Web Vitals» |
| RF-26 | Hecho (efecto limitado) | `preconnect` y fuentes sin bloqueo en las 43 páginas |
| RF-27 | Hecho | causas del tema de Blogger documentadas |
| RF-28 | Hecho | medición repetida; ver tabla |
| RF-29 a RF-32 | Hecho | ver «Visibilidad en IA» |
| RF-33, RF-34 | Hecho (documentado) | tareas del propietario, abajo |
| RF-35 | Hecho | `og:site_name` y fuentes en las 43 páginas |
| RF-36 | Hecho | 0 `aggregateRating` y 0 `Review`; texto «42 reseñas» sin cambios |

Cambio de texto aprobado por el propietario fuera de los RF: en la home, «Servicio 100% online. Sin desplazamientos, sin oficina física.» pasa a «Servicio 100% online, sin desplazamientos. Sede en Almagro (Ciudad Real).», para no contradecir la dirección declarada. `p/bilbao` conserva «sin pedir cita en ninguna oficina» (no se ha tocado).

## Core Web Vitals

Medición de laboratorio con Lighthouse 12 en local (Chrome, sin credenciales), porque la API pública de PageSpeed Insights devolvió 429 (límite de uso sin clave). Es el mismo motor que usa PageSpeed. El INP no se mide en laboratorio: se usa el Total Blocking Time (TBT) como indicador. Umbrales «bueno» de Google: LCP ≤ 2,5 s, CLS ≤ 0,1, INP ≤ 200 ms.

### Estado inicial en producción (25 y 27/11/2026)

| Página | Dispositivo | Puntuación | LCP | CLS | TBT |
|---|---|---|---|---|---|
| Home | móvil | 69 | 4,5 s | 0 | 350 ms |
| Home | escritorio | 95 | 1,2 s | 0,004 | 30 ms |
| Post `despido-empleada-hogar` | móvil | 69 | 4,4 s | 0 | 370 ms |
| Post `despido-empleada-hogar` | escritorio | 88 | 1,3 s | **0,135** | 10 ms |
| `p/madrid` | móvil | 69 | 4,5 s | 0 | 360 ms |
| `p/madrid` | escritorio | 94 | 1,3 s | 0,04 | 40 ms |
| `p/calculadora_2` | móvil | 75 | 3,7 s | 0 | 470 ms |
| `p/calculadora_2` | escritorio | 83 | 1,3 s | **0,184** | 30 ms |

### Antes y después del cambio de fuentes (servidor local, misma máquina)

Se sirvieron la versión del repositorio sin cambios (HEAD) y la modificada.

| Página | Dispositivo | LCP antes → después | CLS antes → después | Puntuación antes → después |
|---|---|---|---|---|
| Home | móvil | 4,2 → 4,2 s | 0 → 0 | 77 → 73 |
| Home | escritorio | 1,1 → 1,1 s | 0,004 → 0,004 | 97 → 98 |
| Post | móvil | 4,4 → 4,5 s | 0 → 0 | 74 → 73 |
| Post | escritorio | 1,1 → 1,1 s | 0,135 → 0,135 | 92 → 92 |
| Madrid | móvil | 4,5 → 4,3 s | 0 → 0 | 62 → 73 |
| Madrid | escritorio | 1,1 → 1,0 s | 0,04 → 0,04 | 97 → 98 |
| Calculadora | móvil | 3,8 → 2,8 s | 0 → 0 | 77 → 79 |
| Calculadora | escritorio | 1,3 → 1,1 s | 0,269 → 0,207 | 83 → 88 |

Lectura: el CLS no empeora en ninguna página, la calculadora mejora de forma clara y en el resto el efecto es pequeño o está dentro del ruido (los TBT varían entre mediciones). El LCP en móvil sigue fuera del umbral (2,8 a 4,5 s) y el CLS de escritorio sigue fuera en el post (0,135) y la calculadora (0,207).

### Causas
- **LCP móvil:** el elemento es texto y el retraso de renderizado es de 3,0 a 3,8 s. Lo producen recursos que bloquean la pintura y que inyecta Blogger: `css_bundle_v2.css` (800 ms) y `widgets.js`. Eso no se puede quitar desde el HTML de las páginas (limitación del tema de Blogger, RF-27).
- **CLS escritorio:** el desplazamiento viene del cambio de fuente (texto en cursiva de la calculadora y primer párrafo del post) cuando se cargan las fuentes. El cambio aplicado no lo elimina.
- Las etiquetas de Google Analytics (179 KB) se cargan con `async` y no bloquean.

### Opciones que quedan fuera de esta spec
- Cambiar `display=swap` por `display=optional` en las fuentes eliminaría el CLS por cambio de fuente, pero en la primera visita el texto saldría con la fuente de respaldo. Es una decisión de diseño del propietario.
- Reducir el LCP móvil por debajo de 2,5 s exige cambiar la plantilla de Blogger.

## Visibilidad en IA

- `robots.txt`: permite todo salvo `/search` y `/share-widget`. No menciona rastreadores de IA y el comodín `*` los deja pasar. Con el agente de usuario de `GPTBot`, `OAI-SearchBot`, `PerplexityBot`, `ClaudeBot`, `Google-Extended` y `Googlebot`, la página de Madrid responde 200 en todos.
- `llms.txt`: 404. Es opcional y Google indica que no hace falta para sus resultados con IA. No se puede publicar en la raíz del dominio con Blogger.
- Sitemaps: coinciden con el repositorio (18 posts y 24 páginas).
- Contenido: `FAQPage` en 23 de 43 páginas; 17 de los 18 posts llevan fuentes oficiales (solo `formulario-prl`, un formulario, no); fecha de actualización y autoría visibles.
- Páginas sin `FAQPage`: la home, 4 posts (renta ×2, ayudas y SEPE) y varias páginas informativas. En renta, ayudas y SEPE las preguntas ya son H2, así que serían candidatas a un bloque de preguntas frecuentes.
- **Propuestas para el propietario (no aplicadas, dependen de fuera del repositorio):**
  1. Comprobar la indexación del sitio en Bing Webmaster Tools y en Brave (ChatGPT, Copilot y Claude dependen de esos índices).
  2. Revisar en Search Console el informe de rendimiento de funciones de IA.

## Limitaciones
- **Plantilla de Blogger:** `og:image:width`, `og:image:height` y el `alt` completo de la imagen Open Graph no se publican (verificado en la spec 002; en producción el `alt` sigue saliendo cortado). Se mantiene la imagen de 993×630.
- **Sitemap:** las 4 utilitarias siguen en el sitemap, con `noindex`. No se pueden excluir desde el HTML.
- **Descripción de `p/blog` en producción:** termina cortada en «correcta» (la `meta description` publicada se corta a mitad de palabra). Conviene que el propietario la revise en Blogger.
- **Repositorio frente a producción:** se alinearon título, H1 y descripción de las páginas afectadas, pero no se comparó el resto del contenido.

## Tareas del propietario
- Publicar los cambios de los archivos modificados.
- Verificar en GA4 DebugView el disparo de `phone_click`, `email_click` y `whatsapp_click`.
- Confirmar en Search Console la indexación y los datos estructurados, incluido el nuevo `LocalBusiness` de la home.
- Comprobar que la ficha de Google Business tiene la misma dirección y el mismo horario que la web.
- Revisar la descripción cortada de `p/blog`.

## Comprobaciones finales
- 60 bloques JSON-LD, 0 inválidos.
- 43 de 43 páginas con un único `<h1>`.
- Recuento de `<div>` abiertos y cerrados igual en las 43 páginas.
- 0 enlaces internos sin archivo correspondiente.
- UTF-8 sin BOM y saltos CRLF conservados (0 saltos LF sueltos).
- 43 archivos HTML modificados, 0 borrados, 0 renombrados.
