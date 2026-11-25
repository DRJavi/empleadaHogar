# AGENTS.md

- No build, package manager, tests, or CI. The repo is a static HTML snapshot of a Blogger site (Spanish, "Contrato Empleada de Hogar") in `contratoempleadahogar/`.
- Pages are rendered Blogger output (`<meta name="generator" content="blogger">`), not templates. Edit the HTML directly; there is no source template or generator.
- Layout mirrors Blogger URLs: `index.html` (home), `p/*.html` (static pages, including city pages, calculators, legal pages), `YYYY/MM/*.html` (blog posts).
- Content is Spanish. Keep edits in Spanish and keep the file encoding intact (some files contain non-UTF-8 bytes, so avoid blind whole-file rewrites).
- Header boilerplate (GA tag `G-74G3M8TE28`, Blogger CSS/widgets, nav) is duplicated in every page. A shared change must be applied to all files, e.g. with `grep` and `edit replaceAll`.
- Don't change internal links or file paths without updating references across all pages, because the paths map to live Blogger URLs.
