# Informe 001 — Chequeo inicial de SEO, arquitectura y diseño

Ámbito: 43 páginas HTML de `contratoempleadahogar/` (1 home, 24 en `p/`, 18 posts en `YYYY/MM/`) y sus 43 URLs publicadas en https://www.contratoempleadahogar.com/. Fuentes de datos: repositorio y sitio publicado (solo lectura). No se ha modificado ningún archivo del sitio.

## Resumen

| Área | Alta | Media | Baja |
|---|---|---|---|
| SEO | 2 | 6 | 2 |
| Arquitectura | 0 | 3 | 2 |
| Diseño | 0 | 1 | 2 |
| Contenido y señales locales | 0 | 4 | 0 |
| Producción | 0 | 1 | 0 |

Lo que ya está bien (verificado en las 43 páginas): `canonical` presente y igual a la URL de la página; `<title>` único; ninguna `meta description` repetida entre páginas; JSON-LD sintácticamente válido; `viewport` presente; todas las imágenes de contenido con atributo `alt`.

## SEO

**S1 — Sin `meta description` (alta).** 10 páginas: `2026/04/renta-2025-para-empleadas-de-hogar`, `2026/04/renta-2025-si-tienes-empleada-de-hogar`, `2026/05/formulario-prl`, `p/aviso-legal`, `p/ficha-de-alta`, `p/incidencias-del-mes`, `p/informacion-para-ia`, `p/iniciar-contratacion`, `p/politica-de-cookies`, `p/politica-de-privacidad`.
Comprobación: `grep -L "name=.description." <archivo>`.

**S2 — Sin `<h1>` (alta).** 5 páginas: `2026/05/el-sepe-activa-la-formacion-preventiva`, `p/aviso-legal`, `p/blog`, `p/como-funciona`, `p/politica-de-privacidad`. Las demás tienen exactamente uno. Todas las que carecen de él tienen `<h2>` (salvo `p/blog`, que no tiene ninguno).
Comprobación: `grep -Lic "<h1" <archivo>`.

**S3 — Enlace interno a una URL que redirige (media).** `p/blog.html` enlaza a `https://www.contratoempleadahogar.com/2026/07/blog-post.html`, que no existe en el repositorio. En producción responde con una redirección 301 a `/p/app-nominas-empleada-hogar.html`. No está roto para el usuario, pero la tarjeta enlaza a una URL antigua en lugar de a la definitiva, y la tarjeta podría ser un resto de plantilla.
Comprobación: `grep -n "2026/07/blog-post" contratoempleadahogar/p/blog.html` y `curl.exe -sI https://www.contratoempleadahogar.com/2026/07/blog-post.html`.

**S4 — Títulos demasiado largos (media).** 31 de 43 `<title>` superan 60 caracteres (con el sufijo ` | AsisteHogar`) y probablemente se truncarán en resultados. Los más largos: `el-sepe-activa-la-formacion-preventiva` (106), `faq` (102), `ayudas-para-contratar-empleada-de-hogar` (101), `como-funciona` (96), `despido-empleada-hogar` (91). Los 12 que no superan 60: `alta-empleada-hogar`, `bilbao`, `calculadora-indemnizacion`, `formulario-prl`, `guias`, `aviso-legal`, `ficha-de-alta`, `incidencias-del-mes`, `informacion-para-ia`, `iniciar-contratacion`, `politica-de-cookies`, `politica-de-privacidad`.
Nota: "60 caracteres" es una heurística y no puede confirmarse con el repositorio (RF-17).

**S5 — Open Graph incompleto y duplicado (media).**
- `og:image` solo existe en 1 de 43 páginas (`2026/03/contrato-empleada-hogar`). Sin imagen, las páginas compartidas en redes salen sin vista previa.
- `og:title` y `og:url` están duplicados en 43 páginas. En 42 el valor es idéntico, así que es redundancia. En `index.html` los dos `og:title` son distintos: "Contrato Empleada de Hogar – Modelos Oficiales y Guía Completa 2026" y "Gestoría de empleadas de hogar online…". Un rastreador puede elegir el equivocado.
- `meta description` aparece duplicada (mismo valor) en 33 páginas.
Comprobación: `grep -c "og:title" <archivo>` y `grep -l "og:image" -r contratoempleadahogar`.

**S6 — Marca inconsistente (media).** Los títulos y `og:site_name` usan "AsisteHogar", pero el dominio y el título original del blog son "Contrato Empleada de Hogar". Dos nombres para la misma entidad dificultan que los buscadores los asocien.
Comprobación: `grep -rl "AsisteHogar" contratoempleadahogar` y `grep -n "og:site_name" contratoempleadahogar/index.html`.

**S7 — Datos estructurados ausentes o incompletos (media).**
- Sin JSON-LD en 17 páginas: `2026/04/ayudas-para-contratar-empleada-de-hogar`, `renta-2025-para-empleadas-de-hogar`, `renta-2025-si-tienes-empleada-de-hogar`, `2026/05/el-sepe-activa-la-formacion-preventiva`, `formulario-prl`, y en `p/`: `app-nominas-empleada-hogar`, `aviso-legal`, `blog`, `como-funciona`, `contacto`, `ficha-de-alta`, `incidencias-del-mes`, `informacion-para-ia`, `iniciar-contratacion`, `opiniones-sobre-nuestro-servicio-de`, `politica-de-cookies`, `politica-de-privacidad`.
- `datePublished` solo aparece en 1 de los 18 posts (`precontrato-empleada-de-hogar`). El resto del `Article` lleva `dateModified` sin fecha de publicación.
- Coherencia entre JSON-LD y contenido visible: comprobada solo por muestreo (en `alta-empleada-hogar` el `headline` coincide con el título). No se verificó en las demás.
Comprobación: `grep -L "ld+json" <archivo>` y `grep -L "datePublished" 2026/*/*.html`.

**S8 — Posible canibalización (media, por validar).** Mapa página-intención inferido del contenido. Los pares con intención muy cercana:

| Intención inferida | Páginas |
|---|---|
| Renta / declaración con empleada de hogar | `renta-2025-para-empleadas-de-hogar`, `renta-2025-si-tienes-empleada-de-hogar` |
| Prevención de riesgos laborales (PRL) | `prevencion-riesgos-laborales-empleada-hogar-2026`, `el-sepe-activa-la-formacion-preventiva`, `formulario-prl` |
| Alta de empleada de hogar | `alta-empleada-hogar`, `p/dar-de-alta-empleada-de-hogar` |
| Calculadoras / coste | `p/calculadora-indemnizacion-empleada-hogar`, `p/calculadora_2`, `finiquito-empleada-hogar` |
| Ayudas para contratar | `ayudas-para-contratar-empleada-de-hogar`, `ayudas-empleadas-de-hogar-madrid-2026` |
| Contrato | `contrato-empleada-hogar`, `contrato-empleada-hogar-interna`, `precontrato-empleada-de-hogar` |

Las páginas de ciudad (`madrid`, `barcelona`, `bilbao`, `malaga`, `sevilla`, `valencia`) tienen intención distinta entre sí. Esta tabla es una inferencia a partir de títulos y contenido, y debe validarla el propietario.

**S9 — Títulos cortos y genéricos (baja).** `p/aviso-legal` (25), `p/ficha-de-alta` (27), `p/incidencias-del-mes` (33), `p/informacion-para-ia` (33), `p/iniciar-contratacion` (34), `p/politica-de-cookies` (33), `p/politica-de-privacidad` (36).

**S10 — Errata en título (baja).** `p/politica-de-privacidad` usa "Politica" sin tilde.
Comprobación: `grep -n "<title" contratoempleadahogar/p/politica-de-privacidad.html`.

## Arquitectura

**A1 — Páginas huérfanas (media).** 3 páginas sin ningún enlace entrante desde otras páginas del repositorio: `p/ficha-de-alta`, `p/incidencias-del-mes`, `p/iniciar-contratacion`. Las demás reciben al menos 4 enlaces. Tampoco tienen `meta description`. Las tres figuran en `sitemap-pages.xml`, así que Google puede indexarlas (ver P4).

**A2 — Páginas utilitarias mezcladas con contenido (media).** `2026/05/formulario-prl` es un formulario ("Rellenar datos") pero vive bajo la ruta de posts `YYYY/MM/`. Otras páginas utilitarias están en `p/` junto a las de contenido: `ficha-de-alta`, `iniciar-contratacion`, `incidencias-del-mes`, `informacion-para-ia`.

**A3 — Cabecera y estilos duplicados en las 43 páginas (media).** Cada archivo lleva su propio CSS embebido y de 14 a 22 reglas `@media`. Cualquier cambio de diseño exige editar los 43. Es inherente a la exportación de Blogger.
Comprobación: `grep -c "@media" <archivo>`.

**A4 — Slugs poco descriptivos (baja).** `p/calculadora_2` (guion bajo y sufijo numérico) y `p/opiniones-sobre-nuestro-servicio-de` (termina en "-de"). Cambiarlos rompería URLs publicadas (principio 3 de la constitución).

**A5 — Sin jerarquía temática en posts (baja, informativo).** Los posts viven en `YYYY/MM/` (estructura de Blogger) sin agrupación por tema. El agrupamiento por tema solo está en `p/guias` y `p/blog`.

## Diseño

**D1 — Falta `lang` en `<html>` (media).** Las 43 páginas declaran `<html class='v2' dir='ltr' …>` sin atributo `lang`. Afecta a lectores de pantalla y a la detección de idioma (accesibilidad).
Comprobación: `grep -L "<html[^>]*lang=" -r contratoempleadahogar`.

**D2 — Jerarquía de encabezados rota (baja).** Las 5 páginas sin `<h1>` (S2) empiezan directamente en `<h2>`. `p/blog` no tiene ningún encabezado.

**D3 — Pocas imágenes de contenido (baja).** Fuera de los iconos de edición de Blogger (con `alt=''`, correcto para decorativos), hay 46 imágenes propias en todo el sitio. Cinco son del post `contrato-empleada-hogar`, así que la mayoría de páginas no tienen imagen propia.

## Contenido, enlaces y señales locales

**S11 — H2 genérico "Preguntas frecuentes" (media).** 14 páginas usan exactamente ese H2 sin tema: 12 posts (`despido`, `empleada-hogar-por-horas`, `salario`, `vacaciones`, `ayudas-empleadas-de-hogar-madrid-2026`, `ayudas-para-contratar`, `finiquito`, `sin-contrato`, `prevencion-riesgos-laborales`, `contrato-interna`, `paro`, `precontrato`) y `p/app-nominas-empleada-hogar` y `p/dar-de-alta-empleada-de-hogar`. Un H2 con el tema ("Preguntas frecuentes sobre el finiquito de la empleada de hogar") suma búsquedas. Ningún H2 repite su H1.
Comprobación: `grep -n "<h2[^>]*>Preguntas frecuentes</h2>" -r contratoempleadahogar`.

**S12 — Posts normativos sin fuente oficial enlazada (media).** `finiquito-empleada-hogar`, `empleada-de-hogar-sin-contrato-en-2026`, `prevencion-riesgos-laborales-empleada-hogar-2026`, `ayudas-para-contratar-empleada-de-hogar` y `formulario-prl` no enlazan a ningún dominio oficial (`.gob.es`, `boe.es`, `seg-social.es`, `sepe.gob.es`…) según el filtro usado. Tratan cuantías, multas y ayudas. El resto de posts enlaza de 1 a 7 fuentes oficiales. Además `el-sepe-activa-la-formacion-preventiva` y `formulario-prl` no muestran fecha de actualización visible, y 5 posts no emiten `author` (los mismos 5 sin JSON-LD).
Nota: el filtro de dominios es una lista fija y puede dejar fuera fuentes válidas. La fecha visible se detectó por patrón de texto.
Comprobación: `grep -c "boe.es\|seg-social.es\|\.gob\.es" <archivo>`.

**S13 — Reputación no verificable (media).** El texto "5/5 en Google con 42 reseñas" aparece en 15 páginas, incluida la `meta description` de la home. Solo 2 páginas (`ficha-de-alta` e `incidencias-del-mes`) enlazan a un perfil de Google. No hay `aggregateRating` en el JSON-LD, que es lo correcto. La cifra no puede confirmarse con el repositorio: queda como pendiente de confirmación del propietario.
Comprobación: `grep -rl "42 rese" contratoempleadahogar`.

**S14 — Páginas de ciudad con pocas señales locales (media).** Las 6 páginas (`madrid`, `barcelona`, `bilbao`, `malaga`, `sevilla`, `valencia`) tienen entre 1.438 y 1.832 palabras, pero solo emiten `FAQPage`: ninguna lleva `LocalBusiness`, dirección, `areaServed` ni teléfono. El solapamiento de frases de 5 palabras entre ciudades va del 9 % al 40 % (las más parecidas: `barcelona`-`valencia` 40 %, `sevilla`-`valencia` 37 %, `malaga`-`sevilla` 33 %). Incluye texto común de menú y pie, así que es orientativo.
Comprobación: `grep -c "LocalBusiness\|areaServed" contratoempleadahogar/p/madrid.html`.

**Lo que está bien:** ningún enlace interno tiene texto genérico ni vacío; 13 posts (todos los que emiten `Article`) llevan `author`; ningún H2 repite el H1.

## Producción (sitio publicado)

Comprobado el 25/11/2026 con peticiones de solo lectura.

**P1 — Estado HTTP (sin hallazgos).** Las 43 URLs responden 200 sin redirecciones. Comprobación: `curl.exe -sI <url>`.

**P2 — Repositorio y producción coinciden (sin hallazgos).** En las 43 páginas coinciden `canonical`, `<title>` y el número de `<h1>`. El `canonical` publicado es igual a la URL de cada página. Las carencias de S1, S2 y S4 a S7 existen también en producción.

**P3 — `robots.txt` y sitemaps (sin hallazgos).**
- `robots.txt` bloquea solo `/search` y `/share-widget`, permite el resto y declara `https://www.contratoempleadahogar.com/sitemap.xml`.
- `sitemap.xml` lista los 18 posts y `sitemap-pages.xml` las 24 páginas de `p/`. Coinciden con el repositorio (la home no aparece, lo normal en Blogger).
- Comprobación: `curl.exe -s https://www.contratoempleadahogar.com/robots.txt` y `.../sitemap.xml`, `.../sitemap-pages.xml`.

**P4 — Páginas utilitarias en el sitemap (media).** `p/ficha-de-alta`, `p/incidencias-del-mes`, `p/iniciar-contratacion` y `2026/05/formulario-prl` están en los sitemaps. Son formularios o páginas internas, sin `meta description` y (las tres primeras) sin enlaces entrantes. Si no deben aparecer en buscadores, hace falta excluirlas. Si sí deben aparecer, necesitan contenido y metadatos propios.
Comprobación: `curl.exe -s https://www.contratoempleadahogar.com/sitemap-pages.xml | Select-String "ficha-de-alta|incidencias|iniciar"`.

**P5 — Enlaces internos fuera del conjunto de páginas.** Se revisaron 21 URLs del propio dominio enlazadas desde las páginas. 20 responden 200 (`favicon.ico` y feeds de Blogger). La restante es la de S3, con 301.

**P6 — Host, HTTPS y 404 (sin hallazgos).**
- `http://www…` y `https://contratoempleadahogar.com/` (sin `www`) responden 301 a `https://www.contratoempleadahogar.com/`.
- Una ruta inexistente devuelve 404.
- `/p/faq.html/` (con barra final) devuelve 404, así que no hay duplicado de URL.
Comprobación: `curl.exe -sI http://www.contratoempleadahogar.com/` y `curl.exe -s -o NUL -w "%{http_code}" https://www.contratoempleadahogar.com/ruta-inventada`.

**P7 — Bots de IA y `llms.txt` (informativo).** `robots.txt` no menciona ningún rastreador de IA (`GPTBot`, `OAI-SearchBot`, `PerplexityBot`, `ClaudeBot`, `Google-Extended`), y el comodín `*` permite todo salvo `/search` y `/share-widget`: los de búsqueda no están bloqueados. `llms.txt` no existe (404). Es opcional y no cuenta como optimización para IA por sí mismo.

**P8 — Medición y peso (informativo).** El repositorio contiene 44 llamadas `gtag('event', …)` y 254 enlaces `wa.me` en total, y 5 enlaces `tel:`. No puede verificarse que cada botón de contacto dispare su evento. El HTML publicado pesa entre 76 y 188 KB (media 106 KB). Core Web Vitals no se evalúan.

## No verificable

Indexación y posiciones, Core Web Vitals, tiempos de carga, contraste calculado, comportamiento táctil y navegación por teclado, disparo real de los eventos de GA4, veracidad de la reputación citada (S13), y coherencia completa del JSON-LD con el contenido visible.

## Prioridad sugerida para una spec de correcciones

1. S1 y S2 (rápidos y de impacto directo).
2. S5, S6 y S4 (coherencia de metadatos y marca).
3. P4, A1 y A2 (qué páginas deben indexarse).
4. S11, S12, S13 y S14 (contenido, fuentes y señales locales).
5. S7, S8, S3 y D1.
