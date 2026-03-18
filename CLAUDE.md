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

## Documentation Topics

The docs cover diverse topics organized in subdirectories:
- `Code-CLI/` - CLI tools and code projects
- `Local-LLM/` - Local LLM running guides
- `Debian/` - Debian/Ubuntu server setup
- `LearningSoftware/` - Software learning notes
- `Selfhost/` - Self-hosted services and homelab
- `Nix/` - NixOS configuration
- `Notes/` - General notes and how-tos
