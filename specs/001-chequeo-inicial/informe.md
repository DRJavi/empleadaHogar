# Informe 001 — Chequeo inicial de SEO, arquitectura y diseño

Ámbito: 43 páginas HTML de `contratoempleadahogar/` (1 home, 24 en `p/`, 18 posts en `YYYY/MM/`). Fuente de datos: solo el repositorio. No se ha modificado ningún archivo del sitio.

## Resumen

| Área | Alta | Media | Baja |
|---|---|---|---|
| SEO | 3 | 5 | 2 |
| Arquitectura | 0 | 3 | 2 |
| Diseño | 0 | 1 | 2 |

Lo que ya está bien (verificado en las 43 páginas): `canonical` presente y igual a la URL de la página; `<title>` único; ninguna `meta description` repetida entre páginas; JSON-LD sintácticamente válido; `viewport` presente; todas las imágenes de contenido con atributo `alt`.

## SEO

**S1 — Sin `meta description` (alta).** 10 páginas: `2026/04/renta-2025-para-empleadas-de-hogar`, `2026/04/renta-2025-si-tienes-empleada-de-hogar`, `2026/05/formulario-prl`, `p/aviso-legal`, `p/ficha-de-alta`, `p/incidencias-del-mes`, `p/informacion-para-ia`, `p/iniciar-contratacion`, `p/politica-de-cookies`, `p/politica-de-privacidad`.
Comprobación: `grep -L "name=.description." <archivo>`.

**S2 — Sin `<h1>` (alta).** 5 páginas: `2026/05/el-sepe-activa-la-formacion-preventiva`, `p/aviso-legal`, `p/blog`, `p/como-funciona`, `p/politica-de-privacidad`. Las demás tienen exactamente uno. Todas las que carecen de él tienen `<h2>` (salvo `p/blog`, que no tiene ninguno).
Comprobación: `grep -Lic "<h1" <archivo>`.

**S3 — Enlace interno roto (alta).** `p/blog.html` enlaza a `https://www.contratoempleadahogar.com/2026/07/blog-post.html`, que no existe en el repositorio. Parece una tarjeta de plantilla sin sustituir.
Comprobación: `grep -n "2026/07/blog-post" contratoempleadahogar/p/blog.html`.

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
- `datePublished` solo aparece en 1 de los 18 posts (`contrato-empleada-hogar`). El resto del `Article` lleva `dateModified` sin fecha de publicación.
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

**A1 — Páginas huérfanas (media).** 3 páginas sin ningún enlace entrante desde otras páginas del repositorio: `p/ficha-de-alta`, `p/incidencias-del-mes`, `p/iniciar-contratacion`. Las demás reciben al menos 4 enlaces. Tampoco tienen `meta description`, así que pueden ser páginas internas de formularios que no deberían indexarse. Eso no es verificable con el repositorio.

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

## No verificable con el repositorio

Indexación y posiciones, Core Web Vitals, peso real y tiempos de carga, estado HTTP y redirecciones, contraste calculado, comportamiento táctil y navegación por teclado, y coherencia completa del JSON-LD con el contenido visible. Tampoco se evalúan `robots.txt` ni el sitemap, porque requieren el sitio publicado (fuera de alcance).

## Prioridad sugerida para una spec de correcciones

1. S1, S2 y S3 (rápidos y de impacto directo).
2. S5, S6 y S4 (coherencia de metadatos y marca).
3. S7 y S8 (datos estructurados y canibalización).
4. A1, A2 y D1.
