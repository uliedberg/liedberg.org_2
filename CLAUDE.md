# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Git workflow

Never push directly to `main`. Create a feature branch and open a PR instead. Anything that lands on `main` is deployed to liedberg.org automatically on push (the hosting service is connected to the GitHub repo; there is no deploy config in this repo).

## Commands

Uses pnpm. If `pnpm` is not on PATH, `npx pnpm <cmd>` works.

- `pnpm install` — install dependencies (`pnpm-workspace.yaml` pre-approves the esbuild/sharp build scripts)
- `pnpm dev` — dev server at localhost:4321
- `pnpm build` — runs `astro check` (type-checks .astro files) then builds to `dist/`
- `pnpm preview` — serve the built `dist/` locally

There are no tests and no linter beyond `astro check`.

## Architecture

Personal/family static site (liedberg.org) built with Astro 4 + Tailwind (with `@tailwindcss/forms`). Content is in Swedish; the main content is a wedding site.

Two ways pages get published:

1. **Astro pages** — file-based routing in `src/pages/` (e.g. `src/pages/brollop/index.astro` → `/brollop/`). Shared markup lives in `src/components/` as plain `.astro` components; pages compose them. There is no shared layout component — each page carries its own `<html>`/`<head>`.
2. **Self-contained HTML drops** — one-off standalone pages go in `public/` as `public/<path>/index.html` and are served verbatim at `/<path>/` (e.g. `public/viktor/student/index.html` → `/viktor/student/`). Use this for pages authored outside the Astro toolchain. These are not linked from the rest of the site; they are reached by direct URL.

Other things to know:

- Pages under `src/pages/` exist in variants kept side by side (e.g. `brollop-inbjudan/index.astro` and `index-rejected.astro`); only files named `index.astro` (or other valid route names) become routes.
- The RSVP form (`src/components/Form.astro`) has no backend — it builds a `mailto:` link from the form fields.
- The seating chart is a static `public/seating.svg` plus hand-maintained name lists in `src/components/Seating.astro`.
