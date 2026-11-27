# Plan de implementación — Spec 003 (Revisión SEO integral)

Fuente: `specs/003-revision-seo-integral/spec.md` (aprobada). Parte del estado publicado de la spec 002 (`specs/002-mejora-seo/cierre.md`).

## Reglas de ejecución (constitución)
- Ediciones puntuales, nunca reescribir un archivo entero (principio 6, codificación). UTF-8 sin BOM y saltos CRLF.
- Cambios de cabecera compartida en las 43 páginas o en ninguna (principio 4).
- No se renombran ni mueven archivos ni URLs (principio 3).
- Cifras legales con fuente oficial y año; sin fuente, no se afirma (principio 7).
- Verificación con `grep`/`curl`/PageSpeed (solo lectura) e informe del resultado (principios 11 y 12).
- Sin commit, push ni publicación en Blogger salvo petición expresa (principio 13).
- Rama: `feature/spec-003-revision-seo-integral` (creada desde `feature/spec-002-mejora-seo`, que contiene el trabajo ya publicado).

## Fases

### Fase 0 — Preparación y línea base
Recuento actual de S1, S2, S4, S6, S9 y S10 (la spec 002 ya tocó alguna página) y guardado de las cifras de partida en el cierre. Medición inicial de Core Web Vitals.

### Fase 1 — Metadatos básicos (RF-1 a RF-8)
`meta description` y `<h1>` donde faltan, títulos a 60 caracteres (flexible hasta 70 con anotación), títulos genéricos y la errata «Politica». Se decide página a página, no por reemplazo masivo.

### Fase 2 — Marca (RF-9 a RF-11)
AsisteHogar como nombre en títulos, `og:site_name` y datos estructurados; «Contrato Empleada de Hogar» como `alternateName`. Barrido de otros nombres.

### Fase 3 — Señales locales (RF-16 a RF-23)
`LocalBusiness` solo en la home con los datos aprobados, texto visible de dirección y horario, coherencia JSON-LD/texto y comprobación de que las ciudades no lo llevan. RF-21 queda pendiente de datos.

### Fase 4 — Core Web Vitals (RF-24 a RF-28)
Diagnóstico de 4 páginas representativas, corrección de las causas que estén en el HTML de las páginas y nueva medición comparada.

### Fase 5 — Visibilidad en IA (RF-29 a RF-32)
Auditoría con `ai-seo` de `robots.txt`, sitemaps y contenido citable. Propuestas, sin aplicar, para lo que dependa del panel de Blogger.

### Fase 6 — Plantilla, utilitarias y medición (RF-12 a RF-15, RF-33, RF-34)
Documentación de las limitaciones de la plantilla y del sitemap, y de las tareas manuales del propietario (GA4 y Search Console).

### Fase 7 — Cierre
Verificación global, informe de cierre y entrega al propietario para publicar. Tras la publicación, verificación en producción.

## Puntos de aprobación del propietario
1. Títulos acortados con excepciones sobre 60 caracteres (T1.3, T1.4).
2. Textos de `meta description` y `<h1>` nuevos (T1.1, T1.2).
3. Cambios de Core Web Vitals que afecten a imágenes o scripts (T4.3).
4. Propuestas de IA que dependan de Blogger (T5.3).
5. Antes de publicar, revisión del resumen de cambios (T7.2).

## Riesgos
- **Plantilla de Blogger:** parte de la cabecera puede no publicarse; se detecta con `curl` tras publicar y se anota «no publicada por la plantilla».
- **Títulos acortados:** pueden perder palabras clave; se revisa cada uno y se admite la excepción anotada.
- **Datos locales:** dirección y horario deben coincidir con la ficha de Google; se comparan con la fuente que aportó el propietario.
- **Core Web Vitals:** los valores varían entre mediciones; se repite la medida y se documenta el rango.
- **Codificación:** algunos archivos tienen bytes no UTF-8; se editan sin cambiar la codificación y se revisa el `git diff` de una muestra.
- **Repositorio frente a producción:** si el propietario edita en Blogger mientras tanto, se compara antes de publicar.
