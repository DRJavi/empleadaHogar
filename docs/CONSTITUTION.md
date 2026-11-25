# Constitución del proyecto

1. **Stack:** HTML estático exportado de Blogger. No se añaden frameworks, bundlers ni dependencias sin aprobación.
2. **Edición directa:** se edita el HTML de `contratoempleadahogar/`. Solo se crean archivos nuevos en `p/` o `YYYY/MM/`.
3. **URLs intocables:** no se renombran ni mueven archivos sin actualizar todos los enlaces internos (verificable con `grep`).
4. **Cambios compartidos:** cabecera, analítica y navegación se modifican en todas las páginas o en ninguna.
5. **Idioma:** contenido en español de España, sin mezclar idiomas.
6. **Codificación:** no se reescriben archivos enteros. Se usan ediciones puntuales para no corromper acentos.
7. **Contenido legal y fiscal:** cifras, plazos y normativa se citan con fuente oficial y año. Sin fuente, no se afirma.
8. **SEO por página:** cada página tiene un `<title>` único, `meta description`, un solo `<h1>` y `canonical`.
9. **Datos estructurados:** el JSON-LD debe ser válido y coincidir con el contenido visible.
10. **Accesibilidad:** las imágenes llevan `alt` y los enlaces tienen texto descriptivo.
11. **Calidad:** tras un cambio no debe haber enlaces internos rotos ni HTML sin cerrar. Se comprueba con `grep` y se informa del resultado. Más adelante se puede pasar a un script en el repo.
12. **Tests:** no existen tests. Todo cambio se verifica con un `grep` o script reproducible y se reporta el resultado.
13. **Límites:** no se toca `.git/`. No se hace commit, push ni publicación en Blogger sin petición expresa.
14. **Skills:** `.agents/skills/` se trata como código de terceros. No se edita. `ai-seo` está aprobada para su uso.
15. **Secretos:** no se añaden claves ni datos personales. El ID de Analytics existente se deja como está.
