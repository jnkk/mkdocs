# Tech Stack — MkDocs digital garden

Pure static-docs stack; no application code, no tests, no type checker.

- **MkDocs 1.6.1** (`mkdocs==1.6.1` in `requirements.txt`)
- **mkdocs-material 9.6.14** theme
- **Python >= 3.11** (`.python-version` pin); environment managed with **uv** (`uv venv` + `uv pip install -r requirements.txt`), lockfile `uv.lock`
- **HARD RULE: uv-only tooling.** Never plain `pip`/`pip3`/`python -m pip`/system Python for local installs (CI's `pip install -r requirements.txt` is fine — it's the deploy pipeline). Prefer `uv add` over `uv pip install` so `uv.lock` stays in sync.
- Plugins (all pinned in `requirements.txt`):
  - `mkdocs-mermaid2-plugin==1.2.1` — Mermaid diagrams via fenced code blocks ` ```mermaid `, configured in `mkdocs.yml` `custom_fences` with `mermaid2.fence_mermaid_custom`
  - `mkdocs-redirects==1.2.3` — path redirect map in `mkdocs.yml`
  - `tags` and `search` (built-in mkdocs-material)
- Markdown extensions: pymdownx (critic, caret, keys, mark, tilde, details, superfences) + admonition
- Theme extras: light/dark palette toggle, `navigation.path` + `navigation.prune` features, Roboto/Roboto Mono fonts

No npm/node dependencies, no custom Python modules. `pyproject.toml` is a bare uv scaffold (no deps) — do not add code there.
