# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project context

This is a learner project following Astro's official "Build a Blog" tutorial. Expect the codebase to grow incrementally as tutorial steps are completed — current pages intentionally duplicate HTML scaffolding because `Layout` components and content collections are introduced later in the tutorial. **Do not refactor ahead of the tutorial** (e.g. extracting a layout, converting posts to a content collection, adding Markdown/MDX plugins) unless the user explicitly asks; the repetition is part of the learning progression.

Runtime: Node `>=22.12.0`, Astro 6.x, ESM. TypeScript uses `astro/tsconfigs/strict`.

## Commands

```sh
npm run dev       # dev server on http://localhost:4321
npm run build     # production build → ./dist/
npm run preview   # serve the built site locally
npm run astro -- check   # type-check .astro files (no test suite exists)
```

There is no configured test runner, linter, or formatter. Don't claim tests pass — there are none. Use `astro check` for type verification.

## Architecture

- `src/pages/` — file-based routing. Each `.astro` or `.md` file becomes a route at its filename (e.g. `src/pages/about.astro` → `/about/`, `src/pages/posts/post-1.md` → `/posts/post-1/`).
- `src/pages/posts/*.md` — blog posts with YAML frontmatter (`title`, `pubDate`, `description`, `author`, `image`, `tags`). Astro renders these directly as pages.
- `src/styles/global.css` — shared styles, imported explicitly at the top of each page's frontmatter (`import '../styles/global.css'`). There is no automatic global inclusion.
- `src/pages/about.astro` demonstrates scoped `<style>` with `define:vars={{ ... }}` to pass frontmatter variables into CSS custom properties — preserve this pattern when editing that page.
- `public/` — static assets served from the site root (favicons live here).
- `astro.config.mjs` is intentionally empty (`defineConfig({})`).

## Conventions for this repo

- Every page currently re-declares its own `<html>`, `<head>`, nav links, and `global.css` import. When adding a new page, copy this scaffolding rather than introducing a layout component.
- Nav links use trailing slashes (`/about/`, `/blog/`, `/posts/post-1/`) to match Astro's default output routing — keep this consistent.
- Blog post frontmatter schema is unenforced (no content collection yet); match the shape used by existing posts in `src/pages/posts/`.
