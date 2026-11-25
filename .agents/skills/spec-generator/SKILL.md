---
name: spec-generator
description: Usa esta skill cuando el usuario pida crear, redactar o revisar una especificación (spec) de una funcionalidad del sitio Contrato Empleada de Hogar. Guía una entrevista de requisitos y produce un spec.md siguiendo la plantilla del proyecto.
---

# Generador de specs

Convierte una idea vaga en una especificación acordada. La spec es el
contrato: si algo no está aquí, no se implementa.

## Proceso

1. **Lee el contexto.** Obligatorio: `docs/CONSTITUTION.md`. Después, las specs
   previas de `specs/` (si existe) para respetar convenciones y no contradecir
   lo ya acordado.
2. **Entrevista al usuario** con la herramienta `question`. Preguntas de **UNA
   en UNA**, máximo 6, esperando respuesta antes de la siguiente. Pon primero la
   opción recomendada. Céntrate en casos límite, comportamiento ante errores y
   qué queda fuera. No propongas soluciones técnicas: si el usuario pregunta
   "¿cómo lo harías?", redirige al QUÉ.
   Prioriza preguntas cuya respuesta cambie lo que hay que construir; descarta
   las que tengan una respuesta obvia por defecto. Si falta una decisión, se
   pregunta: nunca se asume.
3. **Elige el número.** Mira `specs/` (créalo si no existe; el primero es
   `001`) y usa el siguiente libre con tres dígitos: `specs/NNN-<nombre-en-kebab-case>/spec.md`.
4. **Redacta** usando `spec-template.md`, que está en el mismo directorio que
   este `SKILL.md`, sin saltarte secciones. Criterios de aceptación **siempre
   en notación EARS**, numerados como RF-1, RF-2, … Cada requisito debe ser
   verificable: si no se te ocurre cómo comprobarlo, está mal escrito.
5. **Marca lo que no sepas** como `[NECESITA ACLARACIÓN: pregunta concreta]`.
   Nunca rellenes un hueco inventando: un hueco visible es información,
   una suposición silenciosa es deuda.
6. **Pide aprobación explícita** al terminar. No escribas código. Indica al
   usuario que, para implementar, debe cambiar al agente de construcción.

## Reglas

- Nunca modifiques specs existentes salvo petición explícita del usuario.
- La spec describe **QUÉ** y **POR QUÉ**. Prohibido incluir stack, arquitectura,
  nombres de archivos, algoritmos o firmas de funciones.
- Si la funcionalidad es visual, los requisitos de diseño se redactan con
  criterios verificables (breakpoints, contraste, navegación por teclado). La
  implementación usará las skills `frontend-design`, `impeccable` y
  `web-design-guidelines`.
- Si afecta a páginas públicas, incluye requisitos de SEO verificables
  (título, descripción, canonical, JSON-LD) según la constitución.
- Incluye **siempre** la sección "Fuera de alcance". Es la que evita que la
  funcionalidad crezca sola.
- Un requisito, una frase. Si necesitas un "y" para unir dos comportamientos,
  son dos requisitos.
- Sin adjetivos no medibles: "rápido", "intuitivo", "robusto" no son
  requisitos. Escribe el umbral o no lo escribas.

## Notación EARS

Cinco patrones. Elige el que corresponda, no mezcles:

| Patrón | Forma | Cuándo |
|---|---|---|
| Ubicuo | EL SISTEMA \<hará\> | siempre cierto |
| Dirigido por evento | CUANDO \<disparador\>, EL SISTEMA \<hará\> | responde a algo |
| Estado | MIENTRAS \<estado\>, EL SISTEMA \<hará\> | durante una condición |
| Opcional | DONDE \<característica\>, EL SISTEMA \<hará\> | solo si está presente |
| No deseado | SI \<condición\>, ENTONCES EL SISTEMA \<hará\> | errores y casos límite |

Ejemplo bien escrito:

> RF-4: SI el campo `email` del formulario de contacto no tiene formato válido,
> ENTONCES EL SISTEMA no enviará el formulario y mostrará el mensaje
> "Introduce un correo válido".

Mal escrito, para contrastar:

> ~~RF-4: El sistema debe manejar bien los errores del formulario y ser rápido.~~
> Sin patrón EARS, sin criterio verificable, dos ideas en una frase y un
> adjetivo no medible.

## Al revisar una spec existente

Si el usuario pide revisar en vez de crear, no reescribas: **detecta y lista**,
numerado, en cuatro bloques — (1) ambigüedades, (2) contradicciones entre
requisitos, (3) casos límite no cubiertos, (4) conflictos con la constitución.
No propongas soluciones hasta que te lo pidan.
