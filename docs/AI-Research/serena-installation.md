---
title: Serena - AI Coding Agent Toolkit
icon: material/toolbox
tags:
  - serena
  - ai
  - coding-agent
  - mcp
  - toolkit
date_added: 2026-03-19
last_updated: 2026-03-19
---

# Serena - AI Coding Agent Toolkit

> **Source**: [github.com/oraios/serena](https://github.com/oraios/serena)
> **Stars**: 21.7k | **Forks**: 1.5k
> **Last updated**: Mar 17, 2026 (2 days ago)

## What is Serena?

Serena is a **coding agent toolkit** that turns an LLM into a fully-featured agent that works **directly on your codebase**. It provides:

- **Semantic code retrieval and editing tools** — IDE-like capabilities at the symbol level
- **NOT tied to an LLM, framework, or interface** — works with any coding agent
- **Free & open-source** — enhances LLMs you already have access to

Think of Serena as providing **IDE-like tools** to your LLM/coding agent. Instead of reading entire files or using grep-like searches, agents can use code-centric tools like:
- `find_symbol` — find functions/classes by name
- `find_referencing_symbols` — find all usages
- `insert_after_symbol` — edit at the symbol level

## Installation

### Prerequisites

Serena is managed by **`uv`**. Install it first:

```bash
# Install uv (if you don't have it)
curl -LsSf https://astral.sh/uv/install.sh | sh
```

### Quick Start: Start the MCP Server

The easiest way to start Serena:

```bash
uvx --from git+https://github.com/oraios/serena serena start-mcp-server --help
```

This downloads and runs the latest version directly from GitHub.

### Configure Your Client

To connect Serena to your preferred MCP client, configure a launch command.

**Supported clients:**
- Claude Code and Claude Desktop
- Terminal-based: Codex, Gemini-CLI, Qwen3-Coder, rovodev, OpenHands CLI
- IDEs: VSCode, Cursor, IntelliJ
- Extensions: Cline, Roo Code
- Local clients: OpenWebUI, Jan, Agno

See [Client Configuration Guide](https://oraios.github.io/serena/02-usage/030_clients.html) for specific instructions.

## Language Support

Serena supports **30+ programming languages** via Language Server Protocol (LSP), including:

- **Full support**: Python, JavaScript, TypeScript, Rust, Go, Java, C/C++, Ruby, PHP, and more
- **Notable for this project**: **Elixir**, Erlang, Phoenix frameworks
- Others: AL, Ansible, Bash, C#, Clojure, Dart, Elm, Fortran, GLSL, Haskell, Julia, Kotlin, Lua, Nix, OCaml, Perl, PowerShell, R, Scala, Solidity, Swift, TOML, YAML, Zig

> ⚠️ **Note**: Some language servers require additional dependencies. See [Language Support docs](https://oraios.github.io/serena/01-about/020_programming-languages.html).

## Alternative: JetBrains Plugin

For the most robust experience, use the **[Serena JetBrains Plugin](https://plugins.jetbrains.com/plugin/28946-serena/)**:

- Leverages your JetBrains IDE's code analysis
- Supports all JetBrains-supported languages (IntelliJ IDEA, PyCharm, WebStorm, PhpStorm, etc.)
- See [plugin documentation](https://oraios.github.io/serena/02-usage/025_jetbrains_plugin.html)

## Installation Scope: Global + Per-Project

Serena uses a **multi-layered configuration**:

| Level | Location | What it controls |
|-------|---------|-----------------|
| **Global** | `~/.serena/serena_config.yml` | Default tools, modes, language backend, logging |
| **Project** | `./.serena/project.yml` | Project-specific overrides |
| **CLI args** | `--mode`, `--context` | Runtime overrides |

**Global config** applies to all projects. **Project config** can override for specific repos.

### Global Config Location

- **Linux/macOS**: `~/.serena/serena_config.yml`
- **Windows**: `%USERPROFILE%\.serena\serena_config.yml`

The config is auto-created on first run.

### Project Config

Each project can have its own `.serena/` folder with:
- `project.yml` — project-specific settings
- `memories/` — project memories (learns about your codebase)
- Cache files

## Other Installation Methods

| Method | Command/Link |
|--------|--------------|
| **uvx (recommended)** | `uvx --from git+https://github.com/oraios/serena serena start-mcp-server` |
| **Docker** | See [DOCKER.md](https://github.com/oraios/serena/blob/main/DOCKER.md) |
| **Nix** | `flake.nix` available in repo |
| **Build from source** | Clone repo, `uv sync`, `uv run serena start-mcp-server` |

## Key Features

| Feature | Description |
|---------|-------------|
| Symbol-level search | Find functions/classes by name across the codebase |
| Reference tracking | Find all usages of a symbol |
| Safe editing | Edit at symbol level, not string replacement |
| Multi-language | 30+ languages via LSP |
| Project-based workflow | Understands your project structure |
| Token efficiency | Reduces tokens by ~82% compared to file-based approaches |

## Documentation

- [Official Docs](https://oraios.github.io/serena/)
- [User Guide](https://oraios.github.io/serena/02-usage/000_intro.html)
- [Tools Reference](https://oraios.github.io/serena/01-about/035_tools.html)
- [Configuration](https://oraios.github.io/serena/02-usage/050_configuration.html)
- [Changelog](https://github.com/oraios/serena/blob/main/CHANGELOG.md)

## Community Resources

- [Reddit: "Try out Serena MCP - thank me later"](https://www.reddit.com/r/ClaudeAI/comments/1lfsdll/try_out_serena_mcp_thank_me_later/)
- [Blog: "Turning Claude Code into a Development Powerhouse"](https://robertmarshall.dev/blog/turning-claude-code-into-a-development-powerhouse/)
- [Medium: "Serena's Design Principles"](https://medium.com/@souradip1000/deconstructing-serenas-mcp-powered-semantic-code-understanding-architecture-75802515d116)
