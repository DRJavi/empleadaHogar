# Plan de implementación — Spec 002 (Mejora del SEO)

Fuente: `specs/002-mejora-seo/spec.md` (aprobada). Estado acordado que aún no está reflejado en el archivo de la spec:
- RF-11 y RF-12 (`noindex`) quedan **aplazados a la spec 003** (requieren ajustes del panel de Blogger).
- El propietario publica los ficheros. Si una etiqueta de `<head>` (Open Graph, `lang`) no aparece en producción tras publicar, se anota "no publicada por la plantilla" y pasa a la spec 003.

## Reglas de ejecución (constitución)
- Ediciones puntuales, nunca reescribir un archivo entero (principio 6, codificación).
- Cambios de cabecera compartida en las 43 páginas o en ninguna (principio 4).
- No se renombran ni mueven archivos ni URLs (principio 3).
- Verificación con `grep`/`curl` e informe del resultado (principios 11 y 12).
- Sin commit ni push salvo petición expresa (principio 13).
- Rama sugerida: `feature/spec-002-mejora-seo`.

## Fases

### Fase 0 — Preparación
Rama de trabajo, lista de las 43 páginas y línea base: repetir las comprobaciones del informe 001 para S3, S5, S7, S8, S11, S12, D1, P4 y P8 y guardar las cifras de partida.

### Fase 1 — Cabecera común (43 páginas)
Open Graph y idioma. Se aplica con una edición idéntica por archivo y se comprueba que no cambia nada más.

### Fase 2 — Datos estructurados
JSON-LD en las páginas indexables que no lo tienen, `dateModified`, `@id` único de organización y revisión de `FAQPage`.

### Fase 3 — Página `informacion-para-ia`
Título, descripción, JSON-LD, fuentes por cifra y resúmenes enlazados a los posts.

### Fase 4 — Contenido y arquitectura
H2 de FAQ con tema, fuentes oficiales (con aprobación), fecha y autor visibles, enlaces entre contenidos, intención propia en los pares de canibalización y enlace directo en `p/blog`.

### Fase 5 — Páginas de ciudad
Teléfonos y `areaServed` en el JSON-LD y corrección del enlace `tel:`.

### Fase 6 — Medición
Eventos `phone_click` y `email_click` y comprobación de `whatsapp_click`.

### Fase 7 — Cierre
Verificación global, informe de cierre y entrega al propietario para publicar. Tras la publicación, verificación en producción.

## Puntos de aprobación del propietario
1. Fuentes oficiales propuestas para los posts sin fuente (T4.2).
2. Intención, título, H1 e introducción propuestos para cada par de canibalización (T4.5).
3. Antes de publicar, revisión del resumen de cambios (T7.2).

## Riesgos
- **Plantilla de Blogger:** la cabecera puede venir del tema y no publicarse con los ficheros. Se detecta con `curl` tras publicar.
- **Repositorio frente a producción:** si el propietario edita en Blogger mientras tanto, se comparará antes de publicar.
- **Codificación:** algunos archivos tienen bytes no UTF-8. Se leen y escriben sin cambiar la codificación y se revisa el `git diff` de una muestra.
- **Enlaces de fuentes oficiales:** se comprueba que cada URL responde 200 antes de añadirla.
