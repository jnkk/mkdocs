---
title: Claude Code for Elixir Development
icon: material/code-braces-box
tags:
  - claude-code
  - elixir
  - phoenix
  - development-tools
  - ai
date_added: 2026-03-19
last_updated: 2026-03-19
---

# Claude Code for Elixir Development

> Researched via Playwright MCP web searches. Last updated: 2026-03-19
> **Note**: Sources ≤ 3 years old only.

## Overview

Claude Code has a growing ecosystem of Elixir-specific plugins, skills, and SDKs that enhance AI-assisted development for Elixir/Phoenix projects.

---

## Top Elixir Plugins for Claude Code

### 1. Elixir/Phoenix Plugin (Most Comprehensive)
- **Repo**: [oliver-kriska/claude-elixir-phoenix](https://github.com/oliver-kriska/claude-elixir-phoenix)
- **Stars**: 68 | **Forks**: 8
- **Last updated**: Mar 12, 2026 (1 week ago)

**What it does:**
- 20 specialist agents for Elixir/Phoenix development
- 38 skills, 92 references, 18 hooks
- 22 "Iron Laws" — non-negotiable rules that catch bugs tests won't

**Workflow commands:**
```
/phx:intro       # Interactive tutorial
/phx:plan        # Plan a feature with parallel research agents
/phx:work        # Execute plan task-by-task
/phx:verify      # Full verification (compile, format, credo, test)
/phx:review      # 4-agent parallel code review
/phx:full        # Full autonomous mode — plan, implement, review
/phx:quick       # Quick implementation
/phx:investigate # Bug investigation
/phx:audit       # Project health audit
```

**Iron Laws examples:**
- `assign_new` silently skips on reconnect
- `:float` will corrupt money fields
- Oban job idempotency checks

**Installation:**
```bash
/plugin marketplace add oliver-kriska/claude-elixir-phoenix
/plugin install elixir-phoenix
```

**Integration:**
- Tidewave MCP (semantic code search)
- Claude Agent SDK
- 4-agent parallel review (idioms, security, tests, compilation)

---

### 2. Elixir Skill (Pattern Reference)
- **Repo**: [BadBeta/Elixir_skill](https://github.com/BadBeta/Elixir_skill)
- **Stars**: 0 | **Last updated**: Mar 19, 2026 (today)

**What it does:**
Comprehensive skill with references covering:
- **Core**: SKILL.md (hub)
- **OTP**: GenServer, Supervisor, Agent patterns
- **Ecto**: Schemas, changesets, queries, migrations
- **Testing**: ExUnit, Mox, async tests
- **Production**: Deployment, monitoring, OpenApiSpex

**Reference files:**
| File | Coverage |
|------|----------|
| `architecture-reference.md` | Supervision trees, context boundaries |
| `otp-reference.md` | GenServer, Supervisor, Agent |
| `ecto-reference.md` | Schemas, changesets, queries |
| `testing-reference.md` | ExUnit, doctests, mocking |
| `production.md` | OpenApiSpex, deployment |
| `code-style.md` | Module organization, conventions |
| `type-system.md` | @spec, @type, Dialyzer |

**Installation:**
```bash
git clone git@github.com:BadBeta/Elixir_skill.git ~/.claude/skills/elixir
```

**Best practices prompt:**
```
Do not use agents for planning or implementation. Do it yourself.
ALWAYS load the elixir skill and read both SKILL.md and 
architecture-reference.md before starting to plan architecture 
or writing any code.
```

---

### 3. Ash/Phoenix Skill (Tidewave Integration)
- **Repo**: [jordangarrison/ash-kindle](https://github.com/jordangarrison/ash-kindle)
- **Last updated**: Mar 13, 2026 (5 days ago)

**What it does:**
- Claude Code skill for Ash/Phoenix projects
- Sets up AI-assisted dev tooling
- Integrates: `usage_rules`, Tidewave, Ash AI

---

### 4. Claude Agent SDK for Elixir
- **Repo**: [guess/claude_code](https://github.com/guess/claude_code)
- **Stars**: 72

**What it does:**
- Build AI agents with Claude Code in Elixir
- Streaming support
- Phoenix/LiveView integration

---

### 5. BEAM Agent SDK
- **Repo**: [beardedeagle/beam-agent](https://github.com/beardedeagle/beam-agent)
- **Stars**: 2

**What it does:**
- Canonical BEAM SDK for agentic coding
- Unified API over Claude Code, Codex, Gemini, OpenCode
- Erlang/Elixir native

---

## Quick Comparison

| Plugin/Skill | Type | Stars | Best For |
|-------------|------|-------|----------|
| claude-elixir-phoenix | Full plugin | 68 | Phoenix/LiveView full workflow |
| Elixir_skill | Skill | 0 | Elixir patterns & idioms |
| ash-kindle | Skill | 0 | Ash Framework projects |
| guess/claude_code | SDK | 72 | Building agents in Elixir |
| beam-agent | SDK | 2 | Multi-provider agent abstraction |

---

## Recommended Setup for This Project

For an Elixir/Phoenix project like this MkDocs site, recommended:

1. **Install the Elixir Skill** (patterns & idioms):
   ```bash
   git clone git@github.com:BadBeta/Elixir_skill.git ~/.claude/skills/elixir
   ```

2. **Add to CLAUDE.md**:
   ```markdown
   ## Elixir Development Guidelines
   ALWAYS load the elixir skill and read SKILL.md before planning 
   or writing Elixir code.
   
   Prefer:
   - ok/error tuples over exceptions
   - Multi-clause functions over if/else
   - Pipelines over nested calls
   - GenServers over Agents for stateful processes
   ```

---

## External Resources

- [ElixirForum: Best AI code assistants](https://www.reddit.com/r/elixir/comments/1ihsih0/best_ai_code_assistants_for_elixir/)
- [Thinking Elixir Podcast: AI Agents](https://www.youtube.com/watch?v=H1iyBresVkM)
- [Secure AI Coding in Elixir - Elixir Montreal](https://www.youtube.com/watch?v=biSJm-S08Qw)
