# Task Completion — how to verify doc work

This is a documentation repo with no linter, formatter, or test suite. "Done" means the site still builds and the change is structurally consistent.

## Verification commands

1. `mkdocs build` — must exit 0. Catches broken nav entries, missing files, bad frontmatter, broken redirect targets.
2. Optional local smoke check: `mkdocs serve` and open http://127.0.0.1:8000 for the touched pages.

## Checklist before calling a doc task complete

- [ ] `mkdocs build` exits 0 (no WARNINGs about missing files or nav mismatches).
- [ ] New page registered in `mkdocs.yml` `nav:` (if not linked from an index).
- [ ] Frontmatter present: `title`, `icon`, `tags`, `date_added`, `last_updated` (`last_updated` bumped on edits).
- [ ] AI-assisted content lives in `docs/AI-Research/` and ends with `Researched via:` sources.
- [ ] Renamed/moved pages have a `redirects.redirect_maps` entry.

## Deploy

- Automatic: push to `master`/`main` triggers CI → `mkdocs gh-deploy --force`.
- Manual: `mkdocs gh-deploy --force` (only when user asks).
