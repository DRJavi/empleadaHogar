# Tareas — Spec 003 (Revisión SEO integral)

Convención: `[ ]` pendiente. Cada tarea indica el requisito (RF), los archivos y la verificación. Rutas relativas a `contratoempleadahogar/`.

## Fase 0 — Preparación y línea base
- [x] **T0.1** Crear la rama `feature/spec-003-revision-seo-integral`.
- [x] **T0.2** Recontar S1 (sin `meta description`), S2 (sin `<h1>`), S4 (título > 60), S6 (marca), S9 (título < 40) y S10 («Politica») y guardar la línea base en `specs/003-revision-seo-integral/cierre.md`.
  - Verificación: `grep -L "name=.description."`, `grep -Lic "<h1"`, `grep -rl "AsisteHogar"`.
- [x] **T0.3** (RF-24, RF-25) Medir con PageSpeed Insights, en móvil y escritorio, la home, un post, una página de ciudad y una calculadora. Anotar LCP, CLS e INP.
  - Verificación: tabla con las 8 mediciones y su fecha.

## Fase 1 — Metadatos básicos
- [x] **T1.1** (RF-1) (producción ya tenía las descripciones: se copiaron al repositorio) Añadir `meta description` propia a las páginas indexables que no la tengan.
  - Verificación: `grep -L "name=.description."` sin resultados salvo las utilitarias con `noindex`; ninguna descripción repetida.
- [x] **T1.2** (RF-2) (producción ya tenía los H1: se copiaron al repositorio) Añadir un único `<h1>` a las páginas que no lo tengan.
  - Verificación: `grep -c "<h1"` = 1 en las 43 páginas.
- [x] **T1.3** (RF-3, RF-4) Acortar a 60 caracteres o menos el `<title>` (sufijo incluido) de las páginas que lo superan. Admitir hasta 70 si acortar lo empeora y anotar la excepción.
  - Verificación: script con la longitud de cada `<title>`; excepciones listadas en el cierre.
- [x] **T1.4** (RF-5) Dar título descriptivo a las páginas con título genérico de menos de 40 caracteres.
  - Verificación: longitud y contenido de cada `<title>` revisados.
- [x] **T1.5** (RF-6) Corregir «Politica» por «Política» en `p/politica-de-privacidad.html`.
  - Verificación: `grep -n "<title" p/politica-de-privacidad.html`.
- [x] **T1.6** (RF-7, RF-8) Comprobar títulos únicos, `canonical` igual a la URL y ninguna URL cambiada.
  - Verificación: script de duplicados de `<title>` y `canonical`; `git diff --name-status` sin renombrados ni borrados.

## Fase 2 — Marca
- [x] **T2.1** (RF-9) Hacer que «AsisteHogar» sea el nombre en títulos, `og:site_name` y datos estructurados.
  - Verificación: `grep -n "og:site_name"` coherente en las 43 páginas.
- [x] **T2.2** (RF-10) Declarar «Contrato Empleada de Hogar» como `alternateName` del negocio y de la organización.
  - Verificación: `grep -c "alternateName"`; el JSON-LD se parsea sin error.
- [x] **T2.3** (RF-11) Buscar nombres de marca distintos de los dos anteriores y corregirlos o anotarlos.
  - Verificación: informe de coincidencias.

## Fase 3 — Señales locales
- [x] **T3.1** (RF-16, RF-17) Añadir `LocalBusiness` al JSON-LD de `index.html` con nombre, dirección (Ronda de Santo Domingo 2, 13270 Almagro, Ciudad Real), horario de lunes a viernes (10:00–14:00 y 17:00–20:00), los dos teléfonos, `areaServed` de España y el enlace a Google Business sin parámetros de seguimiento. Sin horario de fin de semana.
  - Verificación: se parsea sin error; `grep -c "LocalBusiness" index.html` = 1.
- [x] **T3.1b** (RF-16b) Declarar en `areaServed` del `LocalBusiness` de la home España y las seis ciudades.
  - Verificación: el JSON-LD se parsea sin error; las 7 zonas figuran en `areaServed`.
- [x] **T3.1c** (RF-16c, RF-16d) En el bloque de zonas de la home, mantener las seis ciudades con enlace, mostrar Madrid primera y con indicación visible de zona principal. Usar `frontend-design` y `web-design-guidelines`; **aprobación del propietario del texto y del aspecto.**
  - Verificación: `grep -n "zona-nombre" index.html` con Madrid primera y 6 enlaces; sin enlaces rotos.
- [x] **T3.2** (RF-18, RF-19) Mostrar dirección y horario como texto visible en la home o en `p/contacto.html` y comprobar que coinciden con el JSON-LD.
  - Verificación: comparación de valores con `grep`.
- [x] **T3.3** (RF-20) Comprobar que las 6 páginas de ciudad no llevan `LocalBusiness` ni dirección.
  - Verificación: `grep -c "LocalBusiness\|streetAddress" p/<ciudad>.html` = 0.
- [x] **T3.3b** (RF-21b, RF-36) Mostrar en la home la cifra «más de 1.000 contratos gestionados» como texto visible, sin `aggregateRating` ni `Review`.
  - Verificación: `grep -n "1.000 contratos" index.html`; `grep -rl "aggregateRating"` sin resultados.
- [ ] **T3.4** (pendiente de datos del propietario) (RF-21, RF-22, RF-23) Incorporar datos propios de ciudad solo si el propietario los aporta; con fuente oficial y año las cifras nuevas.
  - Verificación: sin datos, se anota «pendiente de datos del propietario».

## Fase 4 — Core Web Vitals
- [x] **T4.1** (RF-25) Comparar LCP, CLS e INP de T0.3 con los umbrales «bueno» y listar las causas.
  - Verificación: tabla de valores, umbral y causa.
- [x] **T4.2** (RF-27) Separar las causas que están en el tema de Blogger y documentarlas como limitación.
  - Verificación: lista en el cierre.
- [x] **T4.3** (RF-26) Corregir en el HTML las causas corregibles (imágenes, carga de scripts) en todas las páginas afectadas, manteniendo el `alt`. **Aprobación del propietario antes de aplicar.**
  - Verificación: `git diff` de una muestra; ninguna imagen sin `alt`.
- [x] **T4.4** (RF-28) Repetir la medición de T0.3 y comparar.
  - Verificación: tabla antes/después.

## Fase 5 — Visibilidad en IA
- [x] **T5.1** (RF-29) Auditar con la skill `ai-seo` el `robots.txt`, los sitemaps y los rastreadores de IA.
  - Verificación: `curl.exe -s https://www.contratoempleadahogar.com/robots.txt` y resultado anotado.
- [x] **T5.2** (RF-31, RF-32) Revisar que las páginas clave tengan respuestas directas y citables con fuente y fecha, sin afirmaciones nuevas sin fuente.
  - Verificación: lista de páginas revisadas y cambios propuestos.
- [x] **T5.3** (RF-30) Listar las mejoras que dependan del panel de Blogger o de la raíz del dominio y proponerlas sin aplicarlas. **Aprobación del propietario.**
  - Verificación: lista en el cierre.

## Fase 6 — Plantilla, utilitarias y medición
- [x] **T6.1** (RF-12, RF-13) Documentar que la imagen Open Graph se mantiene y la limitación de ancho, alto y `alt` por la plantilla.
  - Verificación: estado «no publicado por la plantilla» anotado.
- [x] **T6.2** (RF-14, RF-15) Documentar que las 4 utilitarias siguen en `sitemap-pages.xml` y `sitemap.xml` y su causa, sin cambiarlas.
  - Verificación: `curl.exe -s https://www.contratoempleadahogar.com/sitemap-pages.xml | Select-String "ficha-de-alta|incidencias|iniciar"`.
- [x] **T6.3** (RF-33, RF-34) Anotar como tareas del propietario la verificación de `phone_click`, `email_click` y `whatsapp_click` en GA4 DebugView y la confirmación en Search Console.
  - Verificación: sección en el cierre.

## Fase 7 — Cierre
- [x] **T7.1** (RF-35, RF-36) Comprobar que la cabecera compartida está en las 43 páginas y que no hay `aggregateRating` ni `Review` ni cambios en el texto de reseñas.
  - Verificación: `git diff --stat`; `grep -rl "aggregateRating"` sin resultados; «42 rese» en las mismas páginas.
- [x] **T7.2** Redactar `specs/003-revision-seo-integral/cierre.md` con línea base frente a estado final, limitaciones y resumen de archivos. **Aprobación del propietario antes de publicar.**
- [x] **T7.3** Verificación de enlaces: ningún enlace interno roto, JSON-LD válido y ningún HTML sin cerrar en los archivos modificados.
- [ ] **T7.4** (tras la publicación por el propietario) Verificar en producción con `curl` cada requisito y anotar «publicado», «no publicado por la plantilla», «aplazado» o «pendiente de datos del propietario».
- [ ] **T7.5** Repetir S1, S2, S4, S6, S9 y S10 y actualizar su estado.

## Dependencias
- T0.2 y T0.3 van antes de cualquier cambio.
- T1.x, T2.x y T3.x son independientes entre sí, salvo T2.1 y T1.3, que comparten el sufijo de los títulos y se hacen juntas.
- T4.3 espera la aprobación del propietario; T4.4 depende de T4.3.
- T5.3 espera la aprobación del propietario.
- T7.2 depende de todas las fases anteriores; T7.4 y T7.5 dependen de la publicación.
