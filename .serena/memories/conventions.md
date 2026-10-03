# Conventions — markdown docs & frontmatter

## Frontmatter (REQUIRED on every doc)

```yaml
---
title: Page Title
icon: material/icon-name
tags:
  - tag1
date_added: YYYY-MM-DD
last_updated: YYYY-MM-DD
---
```

- `title` — used in navigation.
- `icon` — Material icon, e.g. `material/brain`, `material/lan`. Browse at https://squidfunk.github.io/mkdocs-material/reference/icons-emojis/ .
- `tags` — list for the tags plugin.
- `date_added` / `last_updated` — creation/modification dates. Update `last_updated` when editing an existing page.
- NOTE: some older pages (e.g. AI-Research indexes) predate this schema and lack dates — new/edited pages must include them.

## AI-Research content rules (docs/AI-Research/)

Content generated with AI assistance belongs in `docs/AI-Research/` only. Rules:
1. **Google search FIRST** (Playwright/`site:` queries, "best <topic> setup <year>") — never rely on training-data knowledge; docs can be outdated.
2. **Check multiple sources** — official docs vs community guides.
3. **Always cite sources** — end the doc with a `Researched via:` section listing consulted URLs.
4. Organize into subfolders by category (llms, audio, robotics, code, homelab, ...).

## Content style

- One Markdown file per page; topic folders mirror nav sections.
- Relative links between pages; `index.md` per folder for section overviews.
- Mermaid diagrams in ` ```mermaid ` fenced blocks (custom_fence configured — no plugin HTML needed).
- Admonitions (`!!! note`, `!!! warning`) and pymdownx details for callouts.

## Navigation & redirects

- Every new page must be registered in `mkdocs.yml` `nav:` (or linked from a folder index) or it is unreachable.
- When renaming/moving a page, add a `redirects.redirect_maps` entry: `'Old/Path.md': 'New/Path.md'` — do not leave broken links.
- Paths are case-sensitive in nav/redirects; newer sections normalized to lowercase dirs (e.g. `Nix/nixos/`, `Projects/garis-miring/`, `AI-Research/code-cli/`).
