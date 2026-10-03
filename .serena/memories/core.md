# Core — Personal Digital Garden (MkDocs site)

Personal documentation site ("Digital Garden") — content, not code. No application source code lives here; the repo is Markdown + MkDocs config.

## Source map

- `mkdocs.yml` — site config: theme (material), plugins (tags, search, mermaid2, redirects), markdown_extensions, and the full `nav:` tree. **When adding a new page, it must be registered in `nav:`** (or be linked from an index) or it won't appear in the site.
- `docs/` — all Markdown content, organized in topic folders: Notes/, Wiki/, Selfhost/, Local-LLM/, Debian/, Docker/, Nix/, Code-CLI/, LearningSoftware/, AshFramework/, Projects/garis-miring/, AI-Research/, Blog/, Idea/, Builds/, Godot/, Tips/, Static-Site-Generator/, TODOs/, nota/ (legacy).
- `docs/AI-Research/` — **all AI-assisted content goes here** (see `mem:conventions` for its rules).
- `.github/workflows/ci.yml` — CI deploys to GitHub Pages via `mkdocs gh-deploy --force` on push to master/main.
- `site/` — built output, gitignored.
- Root-level `ai-research/` (lowercase) exists but is NOT the doc source — `docs/AI-Research/` is canonical. Do not create content there.

## Invariants

- Site URL: https://jnkk.github.io/mkdocs/ — deploy target is GitHub Pages.
- `mkdocs.yml` `redirects` plugin maps old paths → new paths (e.g. case-normalized `Nix/Nixos/` → `Nix/nixos/`). When moving/renaming a doc, add a redirect map entry instead of breaking old links.
- The `nav:` tree uses explicit per-file entries — a new file only shows up after being added there.
- Master rule for new content: follow the frontmatter + AI-Research requirements in `mem:conventions`.

## Related memories

- `mem:tech_stack` — versions, tooling, how the environment is provisioned.
- `mem:conventions` — frontmatter schema, markdown style, AI-Research content rules, redirects usage.
- `mem:suggested_commands` — serve/build/deploy commands.
- `mem:task_completion` — how to verify doc work is done.
