# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

Luis Jiménez's personal website — a static Astro 7 site (About, Projects, Contact pages) deployed on Netlify at https://lucho-personal-test.netlify.app. No test suite or linter is configured.

## Commands

```sh
npm run dev          # dev server at localhost:4321
npm run astro dev -- --background   # background dev server; manage with `astro dev stop|status|logs`
npm run build        # production build to ./dist
npm run preview      # serve the production build locally
```

## Deployment

Pushing to `main` auto-deploys via Netlify (repo: github.com/luchogit23/personal-site). There is no manual deploy step. Build settings live in `netlify.toml`. Check deploy status with `netlify watch` or the Netlify dashboard.

## Architecture

- **Every page wraps itself in `src/layouts/BaseLayout.astro`**, which owns the `<head>` (Google Fonts, meta), Nav, and Footer. New pages go in `src/pages/` (file-based routing) and should use this layout.
- **All theming flows from CSS custom properties in `src/styles/global.css`** (colors, fonts, type scale, shadows, motion). Components use scoped `<style>` blocks that reference these tokens — to change the look, change the tokens, not the components. Current design: blue & white (`--accent: #2563eb`, navy `--ink`), Plus Jakarta Sans for headings, Inter for body, soft shadows (`--shadow-soft` / `--shadow-lift`).
- **Projects are a content collection** (`src/content.config.ts`, glob loader). Adding a project = adding a `.md` file to `src/content/projects/` with frontmatter (`title`, `description`, `tech[]`, optional `github`/`demo` URLs, `featured`, `order`). `featured: true` entries appear on the homepage; the Projects page shows all, sorted by `order`. Cards render via `src/components/ProjectCard.astro`.
- **The contact form uses Netlify Forms**: the `<form>` in `src/pages/contact.astro` needs its `data-netlify="true"`, hidden `form-name` input, and honeypot field to survive into the built static HTML — Netlify's build-time scan is what wires up submissions. It redirects to `/thanks` on submit.

## Content status

Placeholder copy is marked with `<!-- TODO: Luis -->` comments (bio in about.astro, hero one-liner in index.astro, social handles in contact.astro and Footer.astro, sample projects). Replace those with real content; don't invent biographical details.

## Astro docs

Consult before related tasks: [routing](https://docs.astro.build/en/guides/routing/) · [components](https://docs.astro.build/en/basics/astro-components/) · [content collections](https://docs.astro.build/en/guides/content-collections/) · [styling](https://docs.astro.build/en/guides/styling/)
