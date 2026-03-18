---
title: MkDocs Guide
icon: material/book-open-page-variant
tags:
  - mkdocs
  - documentation
  - guide
date_added: 2026-03-18
last_updated: 2026-03-18
---

# MkDocs Guide

A comprehensive guide to writing and organizing documentation with MkDocs and the Material theme.

## Quick Reference

```bash
# Serve locally (live reload)
mkdocs serve

# Build static site
mkdocs build

# Deploy to GitHub Pages
mkdocs gh-deploy --force
```

## File Structure

Your documentation source should be in the `docs/` folder:

```
project/
├── mkdocs.yml          # Configuration
└── docs/              # Documentation source
    ├── index.md       # Homepage (becomes /)
    ├── about.md      # About page (becomes /about/)
    └── guide/
        └── getting-started.md  # (becomes /guide/getting-started/)
```

## Frontmatter (YAML Metadata)

Add metadata at the top of each Markdown file:

```yaml
---
title: Page Title
icon: material/icon-name
tags:
  - tag1
  - tag2
date_added: 2026-03-18
last_updated: 2026-03-18
---
```

### Required Frontmatter Fields

| Field | Description | Example |
|-------|-------------|---------|
| `title` | Page title in navigation | `title: Getting Started` |
| `icon` | Material icon for nav | `icon: material Rocket` |
| `tags` | List of categorization tags | `tags: [linux, guide]` |
| `date_added` | Creation date (YYYY-MM-DD) | `date_added: 2026-03-18` |
| `last_updated` | Last modified (YYYY-MM-DD) | `last_updated: 2026-03-18` |

### Material Icons

!!! tip "Icon Search"
    **[Click here to browse all available icons](https://squidfunk.github.io/mkdocs-material/reference/icons-emojis/)**

    Use the search to find icons by keyword (e.g., "network", "security", "code")

Common icons:
- `material/brain` - AI/ML topics
- `material/rocket` - Getting started
- `material/cog` - Settings/config
- `material/lan` - Networking
- `material/server` - Servers
- `material/shield-check` - Security

## Markdown Syntax

### Headers

```markdown
# H1 - Page title
## H2 - Section
### H3 - Subsection
#### H4 - Minor heading
```

### Links

```markdown
# Internal links (within docs/)
[Link text](other-page.md)
[Link text](folder/page.md)
[Link to section](page.md#section-name)

# External links
[Link text](https://example.com)
```

### Images

```markdown
![Alt text](../images/screenshot.png)
```

### Code Blocks

````markdown
```python
def hello():
    print("Hello, World!")
```
````

### Tables

```markdown
| Header 1 | Header 2 |
|----------|----------|
| Cell 1   | Cell 2   |
| Cell 3   | Cell 4   |
```

### Lists

```markdown
- Unordered item
- Another item
  - Nested item

1. Ordered item
2. Second item
```

## Material Theme Features

### Admonitions (Callouts)

```markdown
!!! note
    This is a note.

!!! warning
    This is a warning.

!!! tip
    This is a tip.
```

Available types: `note`, `tip`, `warning`, `danger`, `info`, `abstract`, `success`, `question`, `failure`, `example`, `quote`

### Collapsible Sections

```markdown
??? summary "Click to expand"
    Hidden content here.
```

### Tabs

```markdown
=== "Tab 1"
    Content of tab 1.

=== "Tab 2"
    Content of tab 2.
```

### Task Lists

```markdown
- [x] Completed task
- [ ] Incomplete task
```

### Footnotes

```markdown
Here is a footnote[^1].

[^1]: This is the footnote content.
```

## Navigation

### Auto-Generated Navigation

MkDocs automatically discovers all Markdown files and creates navigation based on file structure.

### Manual Navigation (mkdocs.yml)

```yaml
nav:
  - Home: index.md
  - Guide:
    - getting-started.md
    - configuration.md
  - Reference: reference.md
```

## Useful Links

- [MkDocs User Guide](https://www.mkdocs.org/user-guide/writing-your-docs/)
- [Material Theme Docs](https://squidfunk.github.io/mkdocs-material/)
- [Icon Search](https://squidfunk.github.io/mkdocs-material/reference/icons-emojis/)

---

*Researched via: MkDocs.org and Material for MkDocs official documentation*