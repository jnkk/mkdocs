# Suggested Commands — this repo

All commands run from repo root `/home/jnkk/Projects/mkdocs`. No special Linux-specific shell quirks.

```bash
# First-time setup
uv venv && source .venv/bin/activate && uv pip install -r requirements.txt

# Local dev with live reload (defaults to http://127.0.0.1:8000)
mkdocs serve

# Build static site into site/ (gitignored) — the sanity check for doc edits
mkdocs build

# Deploy to GitHub Pages (also done by CI on push to master/main)
mkdocs gh-deploy --force
```

- There is no lint/test/format step for this repo — verification = `mkdocs build` succeeding.
- `uv` is the package manager; do not use pip directly outside the venv.
- Prefer `uv run mkdocs ...` over activating the venv manually.
