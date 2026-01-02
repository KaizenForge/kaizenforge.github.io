# Copilot instructions (kaizenforge.github.io)

## Project shape
- This repo is a **Hugo (extended) static site** using **Hugo Theme Stack v3** loaded via **Hugo Modules**.
- Theme/module wiring:
  - `config/_default/module.toml` imports `github.com/CaiJimmy/hugo-theme-stack/v3`.
  - `go.mod` pins the theme module version.
- Site configuration lives in `config/_default/` (not a single `config.toml`).
- Generated output:
  - `public/` and `resources/` are build artifacts and are **gitignored** (see `.gitignore`). Don’t hand-edit them.

## Content conventions
- Posts are **page bundles**: `content/post/<slug>/index.md` (e.g. `content/post/hello-world/index.md`).
  - Use front matter `slug` (permalinks are configured as `/p/:slug/` in `config/_default/permalinks.toml`).
  - Put post images alongside the post bundle when possible.
- Top-level pages live under `content/page/...` (e.g. archives/search/links).
- Taxonomies are represented under `content/categories/` and `content/tags/`.

## Markdown / rendering specifics
- Raw HTML in Markdown is allowed (`goldmark.renderer.unsafe = true` in `config/_default/markup.toml`).
- Math passthrough is enabled with delimiters `\( \)` / `\[ \]` and `$$ $$` (see `config/_default/markup.toml`).
- Code highlighting is configured to use line numbers and no CSS classes inlining (see `config/_default/markup.toml`).

## Styling and overrides
- Site-specific CSS overrides go in `assets/scss/custom.scss`.
- Static files should go in `static/` (served as-is).

## Comments / analytics integration points
- This repo currently includes **stub partials** to satisfy theme calls:
  - `layouts/partials/disqus.html`
  - `layouts/partials/google_analytics.html`
  Keep `comments.enabled = true` (as currently configured in `config/_default/params.toml`), but note that the Disqus partial is intentionally stubbed; to make comments actually work, switch to another provider supported by the theme (e.g. giscus/utterances/waline) and configure it in `config/_default/params.toml`, or implement the desired provider in these partials.

## Developer workflows
- Local dev server (from repo root): `hugo server`.
  - Use **Hugo extended v0.152.2** (matches the current dev environment) and **Go** (for Hugo Modules).
- Production build (matches CI intent): `hugo --minify --gc`.
- Update Theme Stack module:
  - `hugo mod get -u github.com/CaiJimmy/hugo-theme-stack/v3`
  - `hugo mod tidy`

## CI / deployment
- GitHub Pages deploy workflow: `.github/workflows/hugo.yml`.
  - Installs Hugo extended + Dart Sass, then runs `hugo --minify` with a Pages-provided `--baseURL`.
- Version note: CI currently pins Hugo via `HUGO_VERSION` in `.github/workflows/hugo.yml` (older than local). If you hit a build mismatch, either reproduce with the CI version locally or bump the workflow version intentionally.
- Theme auto-update workflow: `.github/workflows/update-theme.yml` (scheduled) runs `hugo mod get -u` and commits changes.
