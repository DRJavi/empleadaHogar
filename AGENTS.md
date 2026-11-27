# AGENTS.md

- No build, package manager, tests, or CI. The repo is a static HTML snapshot of a Blogger site (Spanish, "Contrato Empleada de Hogar") in `contratoempleadahogar/`.
- Pages are rendered Blogger output (`<meta name="generator" content="blogger">`), not templates. Edit the HTML directly; there is no source template or generator.
- Layout mirrors Blogger URLs: `index.html` (home), `p/*.html` (static pages, including city pages, calculators, legal pages), `YYYY/MM/*.html` (blog posts).
- Content is Spanish. Keep edits in Spanish. All HTML files are valid UTF-8 without BOM and use CRLF line endings: preserve both (read/write as UTF-8 no BOM, don't normalize line endings) and prefer targeted edits over whole-file rewrites.
- Header boilerplate (GA tag `G-74G3M8TE28`, Blogger CSS/widgets, nav) is duplicated in every page. A shared change must be applied to all files, e.g. with `grep` and `edit replaceAll`.
- Don't change internal links or file paths without updating references across all pages, because the paths map to live Blogger URLs.
- Never run `git commit`, `git push` or open a pull request without the user's explicit prior confirmation. When committing, stage only the specific files changed (no `git add -A`).
- Pull requests: `gh` is not installed and there is no token in the environment, and this session has no GitHub tool. Once the user confirms, do not ask them to install `gh` or to open the PR themselves; create it through the GitHub API with the credential Git already stores: read it with `git credential fill` (host `github.com`) without printing it, check `GET /repos/DRJavi/empleadaHogar/pulls?state=all&head=DRJavi:<branch>` for an existing PR, then `POST /repos/DRJavi/empleadaHogar/pulls` with `Invoke-RestMethod`. Base branch is `develop` unless told otherwise. Send title and body as UTF-8 (body from a file in `C:\Users\javier.dominguez\AppData\Local\Temp\opencode`, never inside the repo) and report the PR URL.
