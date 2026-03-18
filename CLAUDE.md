# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a personal **MkDocs documentation site** (Digital Garden) built with:
- MkDocs 1.6.x with Material theme
- Python 3.11+ managed via uv
- Mermaid diagrams support
- GitHub Pages deployment via CI

## Commands

```bash
# Install dependencies
uv venv && source .venv/bin/activate && uv pip install -r requirements.txt

# Serve locally (live reload)
mkdocs serve

# Build static site
mkdocs build

# Deploy to GitHub Pages
mkdocs gh-deploy --force
```

## Structure

- `mkdocs.yml` - Site configuration (theme, plugins, navigation)
- `docs/` - Markdown content directory
- `.github/workflows/ci.yml` - CI pipeline that deploys to GitHub Pages on push to main/master

## AI Research Notes

**ALL new content generated with AI assistance should go in `docs/AI-Research/`**
- This folder is for topics researched with AI help
- Categories: llms, audio, robotics, code, homelab, etc.
- Create subfolders as needed

### Research Requirements

When creating content in AI-Research:
- **ALWAYS use Google search first** via Playwright MCP - use "site:example.com <topic>" or "best <topic> setup 2025"
- **NEVER rely on training data** - my knowledge cutoff means docs may be outdated
- **Check multiple sources** - compare official docs vs community guides
- **Always cite sources** - add "Researched via:" at the bottom of docs

Example workflow:
1. Search Google: "cloudflared tunnel setup 2025" or "openwrt vlan config guide"
2. Use Playwright to visit official documentation and top guides
3. Write content based on current, accurate information
4. Add source attribution at end of file

**Why Google first:** My training data has a cutoff date - things change! Google search finds the latest docs, tutorials, and best practices.

### Frontmatter Requirements

All new docs **MUST include frontmatter** at the top:

```yaml
---
title: Page Title
icon: material/icon-name
tags:
  - tag1
  - tag2
date_added: YYYY-MM-DD
last_updated: YYYY-MM-DD
---
```

**Required fields:**
- `title` - Page title (used in navigation)
- `icon` - Material icon (e.g., `material/brain`, `material/lan`)
- `tags` - List of tags for categorization
- `date_added` - When the file was first created (YYYY-MM-DD)
- `last_updated` - Last time the file was modified (YYYY-MM-DD)

**Where to find icons:** https://squidfunk.github.io/mkdocs-material/reference/icons-emojis/

## Documentation Topics

The docs cover diverse topics organized in subdirectories:
- `Code-CLI/` - CLI tools and code projects
- `Local-LLM/` - Local LLM running guides
- `Debian/` - Debian/Ubuntu server setup
- `LearningSoftware/` - Software learning notes
- `Selfhost/` - Self-hosted services and homelab
- `Nix/` - NixOS configuration
- `Notes/` - General notes and how-tos
