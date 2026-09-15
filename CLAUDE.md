# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Communication Style

- Use direct, concise language without unnecessary adjectives or adverbs
- Avoid flowery or marketing-style language ("tremendous", "dramatically", "revolutionary", etc.)
- Don't include flattery or excessive praise ("excellent!", "perfect!", "great job!")
- State facts and findings directly without embellishment
- Skip introductory phrases like "I'm excited to", "I'd be happy to", "Let me dive into"

## Project Overview

reveal.js presentation about using DDEV together with `shopware-cli` for Shopware 6 client
project development. Slide content lives in Markdown; reveal.js is loaded from unpkg CDN,
so there is no build step.

## Layout

- `index.html` — reveal.js bootstrap and `Reveal.initialize()` configuration
- `slides/ddev-shopware-cli-presentation.md` — all slide content
- `css/custom.css` — DDEV-branded theme overrides on top of the `white` theme
- `images/` — assets referenced from the slides
- `.github/workflows/deploy.yml` — copies `index.html`, `slides/`, `css/`, `images/` into
  `_site/` and deploys to GitHub Pages on push to `main`

## Slide Conventions

- `---` on its own line = new horizontal slide
- `--` on its own line = vertical (nested) slide
- `Note:` = speaker notes (separators are configured in `index.html`)
- New asset directories must also be added to the copy step in `deploy.yml`

## Local Preview

```bash
npm install
npm start   # http://localhost:8000
```

## Development Notes

### Branch Naming

Format: `YYYYMMDD_<username>_<short_description>`

## Important Instruction Reminders

Do what has been asked; nothing more, nothing less.
NEVER create files unless they're absolutely necessary for achieving your goal.
ALWAYS prefer editing an existing file to creating a new one.
NEVER proactively create documentation files (*.md) or README files. Only create documentation files if explicitly requested by the User.
