# Tareas — Spec 002 (Mejora del SEO)

Convención: `[ ]` pendiente. Cada tarea indica el requisito (RF), los archivos y la verificación. Rutas relativas a `contratoempleadahogar/`.

## Fase 0 — Preparación
- [x] **T0.1** Crear la rama `feature/spec-002-mejora-seo`.
- [x] **T0.2** Guardar la línea base de S3, S5, S7, S8, S11, S12, D1, P4 y P8 (cifras del informe 001) en el informe de cierre.
  - Verificación: las cifras coinciden con las del informe 001.

## Fase 1 — Cabecera común (43 páginas)
- [x] **T1.1** (RF-1, RF-2) Declarar en las 43 páginas la imagen de `2026/03/contrato-empleada-hogar` como `og:image` y `twitter:image`, con `og:image:width` 993, `og:image:height` 630 y `og:image:alt`.
  - Verificación: `grep -c "og:image" <archivo>` ≥ 1 en las 43. Medir la imagen publicada y confirmar 993×630.
- [x] **T1.2** (RF-3) Eliminar el `og:title`, `og:url` y `og:description` duplicados, dejando uno por página.
  - Verificación: `grep -c "property='og:title'" <archivo>` = 1 en las 43.
- [x] **T1.3** (RF-4) Hacer que el `og:title` de `index.html` coincida con su `<title>`.
  - Verificación: comparar ambos valores con `grep`.
- [x] **T1.4** (RF-29) Añadir `lang="es"` a `<html>` en las 43 páginas.
  - Verificación: `grep -L "<html[^>]*lang=" -r .` sin resultados.

## Fase 2 — Datos estructurados
- [x] **T2.1** (RF-5) Añadir JSON-LD con `BreadcrumbList` y el tipo adecuado a las páginas indexables que no lo tienen:
  - Posts: `ayudas-para-contratar-empleada-de-hogar`, `renta-2025-para-empleadas-de-hogar`, `renta-2025-si-tienes-empleada-de-hogar`, `el-sepe-activa-la-formacion-preventiva` (`Article`).
  - `p/`: `app-nominas-empleada-hogar`, `blog` (`CollectionPage`), `como-funciona`, `opiniones-sobre-nuestro-servicio-de`, `contacto` (`ContactPage`), `aviso-legal`, `politica-de-cookies`, `politica-de-privacidad` (`WebPage`).
  - Se excluyen `ficha-de-alta`, `incidencias-del-mes`, `iniciar-contratacion` y `formulario-prl` por ser utilitarias; `informacion-para-ia` va en T3.3.
  - Verificación: `grep -L "ld+json"` sobre esas páginas sin resultados; el JSON-LD se parsea sin error.
- [x] **T2.2** (RF-6) Añadir `dateModified` a los posts que no lo tengan, con el `lastmod` de `sitemap.xml`.
  - Verificación: `grep -L "dateModified" 2026/*/*.html` sin resultados en los posts con `Article`.
- [x] **T2.3** (RF-7) Unificar el `@id` de la organización y añadir el nodo mínimo en cada página que lo referencie.
  - Verificación: `grep -o "#organization"` coherente en todas las páginas.
- [x] **T2.4** (RF-8) Comprobar que cada `FAQPage` coincide con las preguntas visibles.
  - Verificación: informe con las páginas revisadas y los desajustes corregidos.
- [x] **T2.5** (RF-9, RF-10) Comprobar que no hay `aggregateRating` ni `Review` y que no se modificó el texto de reseñas.
  - Verificación: `grep -rl "aggregateRating"` sin resultados; `git diff` sin cambios en el texto de reseñas.

## Fase 3 — `informacion-para-ia`
- [x] **T3.1** (RF-18) Título y `meta description` propios en `p/informacion-para-ia.html`.
  - Verificación: `grep -n "<title\|name='description'"`.
- [x] **T3.2** (RF-20) Enlazar cada cifra normativa (SMI, cotización, desempleo, FOGASA, MEI) a su fuente oficial del BOE (`Orden PJC/297/2026`, `RD 126/2026`).
  - Verificación: cada cifra tiene un enlace cercano que responde 200.
- [x] **T3.3** (RF-19) Añadir JSON-LD `WebPage` válido.
  - Verificación: se parsea sin error.
- [x] **T3.4** (RF-21) Resumir el contenido de los posts con enlace a cada uno, sin repetir el texto completo.
  - Verificación: revisión del texto y `grep` de los enlaces a posts.

## Fase 4 — Contenido y arquitectura
- [x] **T4.1** (RF-22) Cambiar el H2 "Preguntas frecuentes" por uno con tema en las 14 páginas afectadas.
  - Verificación: `grep -n "<h2[^>]*>Preguntas frecuentes</h2>" -r .` sin resultados.
- [x] **T4.2** (RF-23) **Aprobada y aplicada.** Proponer una fuente oficial para `finiquito-empleada-hogar`, `empleada-de-hogar-sin-contrato-en-2026`, `prevencion-riesgos-laborales-empleada-hogar-2026` y `ayudas-para-contratar-empleada-de-hogar`. Propuesta inicial: RD 1620/2011 y RDL 16/2022 para finiquito y sin contrato; RD 893/2024 para prevención; la fuente de ayudas está por localizar. Añadir solo las aprobadas.
  - Verificación: cada enlace responde 200 con `curl`.
- [x] **T4.3** (RF-24) Mostrar fecha de actualización y autor visibles en todos los posts. Incluir `el-sepe-activa-la-formacion-preventiva` y `formulario-prl`, que no la muestran.
  - Verificación: `grep` del texto de fecha y autor en los 18 posts.
- [x] **T4.4** (RF-17) Cada post enlaza al menos a una página de servicio y a dos contenidos relacionados.
  - Verificación: recuento de enlaces internos por post.
- [x] **T4.5** (RF-13, RF-14) **Enlaces cruzados hechos; títulos y H1 se mantienen (aprobado).** Definir la intención de cada página de los pares del informe 001 (S8) y ajustar título, H1, introducción y enlaces cruzados descriptivos:
  - Renta: `renta-2025-para-empleadas-de-hogar` y `renta-2025-si-tienes-empleada-de-hogar`.
  - PRL: `prevencion-riesgos-laborales-empleada-hogar-2026`, `el-sepe-activa-la-formacion-preventiva` y `formulario-prl`.
  - Alta: `alta-empleada-hogar` y `p/dar-de-alta-empleada-de-hogar`.
  - Calculadoras: `p/calculadora-indemnizacion-empleada-hogar`, `p/calculadora_2` y `finiquito-empleada-hogar`.
  - Ayudas: `ayudas-para-contratar-empleada-de-hogar` y `ayudas-empleadas-de-hogar-madrid-2026`.
  - Contrato: `contrato-empleada-hogar`, `contrato-empleada-hogar-interna` y `precontrato-empleada-de-hogar`.
  - Verificación: títulos distintos por par y enlaces cruzados presentes.
- [x] **T4.6** (RF-16) En `p/blog.html`, cambiar el enlace a `/2026/07/blog-post.html` por `/p/app-nominas-empleada-hogar.html`.
  - Verificación: `grep -n "2026/07/blog-post" p/blog.html` sin resultados.
- [x] **T4.7** (RF-15) Comprobar que no cambió ninguna URL ni se eliminó ninguna página.
  - Verificación: `git diff --name-status` sin renombrados ni borrados.

## Fase 5 — Páginas de ciudad
- [x] **T5.1** (RF-25) En `madrid`, `barcelona`, `bilbao`, `malaga`, `sevilla` y `valencia`, añadir al JSON-LD `telephone` y `contactPoint` con el móvil (`+34685153360`) como principal y el fijo (`+34926112851`) como segundo.
  - Verificación: `grep -c "+34685153360" p/<ciudad>.html`; el JSON-LD se parsea sin error.
- [x] **T5.2** (RF-26) Declarar `areaServed` con la ciudad o región de cada página.
  - Verificación: `grep -c "areaServed" p/<ciudad>.html` = 1.
- [x] **T5.3** (RF-28) Localizar y corregir el enlace `tel:34926112851` sin prefijo `+`.
  - Verificación: `grep -rn "tel:3" .` sin resultados.
- [x] **T5.4** (RF-27) Comprobar que no se añadió dirección, horario ni otro dato no confirmado.
  - Verificación: `grep -rn "streetAddress\|openingHours"` sin resultados nuevos.

## Fase 6 — Medición
- [x] **T6.1** (RF-30, RF-32, RF-33) Disparar `phone_click` en los enlaces `tel:` y `email_click` en los `mailto:`, con `event_category`, `event_label` y `page_path`, sin duplicar ni renombrar eventos existentes.
  - Verificación: `grep -c "phone_click\|email_click"`; `git diff` sin cambios en `whatsapp_click` ni `generador_contrato`.
- [x] **T6.2** (RF-31) Revisar los enlaces `wa.me` y registrar cuáles no llaman a `whatsapp_click`.
  - Verificación: informe con el recuento de enlaces con y sin evento.

## Fase 7 — Cierre
- [x] **T7.1** (RF-34, RF-35) Comprobar que solo se tocó título, meta description, H1 y marca en los pares de canibalización y en `informacion-para-ia`, y que los cambios de cabecera están en las 43 páginas.
  - Verificación: `git diff --stat` y recuento por archivo.
- [x] **T7.2** Redactar `specs/002-mejora-seo/cierre.md` con la línea base frente al estado final, las limitaciones y el resumen de archivos para el propietario. **Aprobación del propietario antes de publicar.**
- [x] **T7.3** Verificación de enlaces: ningún enlace interno roto ni HTML sin cerrar en los archivos modificados.
- [x] **T7.4** (tras la publicación por el propietario) Verificar en producción con `curl` cada requisito y anotar "publicado", "no publicado por la plantilla" o "aplazado".
- [x] **T7.5** Repetir S3, S5, S7, S8, S11, S12, D1, P4 y P8 y actualizar su estado.

## Dependencias
- T1.x, T2.x, T3.x, T5.x y T6.x son independientes entre sí.
- T4.2 y T4.5 esperan la aprobación del propietario.
- T7.2 depende de todas las fases anteriores; T7.4 y T7.5 dependen de la publicación.
