---
title: Elixir Debugging Tools - Session Notes
icon: material/bug
tags:
  - elixir
  - debugging
  - tooling
  - session-notes
date_added: 2026-03-19
last_updated: 2026-03-19
---

# Elixir Debugging Tools - Session Notes

> Session state and progress tracking. Last updated: 2026-03-19

## Work in Progress

Currently expanding and updating the Elixir debugging tools documentation in `docs/AI-Research/elixir-debugging-tools.md`.

### Current Tasks (Remaining)

- **Fix Modern Tracing Tools section** in the main doc:
  - Remove `tracer` (verified stale - Sep 8, 2017)
  - Verify `X-Trace` current status on GitHub
  - Keep only verified-current tools (recon, built-in tracing, Observer)
- **Commit updated document**

### Completed Work

1. **Created `playwright-search` skill** at `~/.config/opencode/superpowers/skills/playwright-search/SKILL.md` and committed to git

2. **Initial doc created:** `docs/AI-Research/elixir-debugging-tools.md` (281 lines) with:
   - Quick reference table
   - dbg/2, IO.inspect usage
   - Pry and breakpoint setup
   - Observer GUI with all tabs
   - Remote debugging walkthrough
   - Profiling commands
   - Erlang tools (etop, ttb, crashdump_viewer)
   - Phoenix LiveDashboard
   - Docker container debugging section

3. **First expansion** added community insights:
   - Debugging concurrent systems (Robert Virding quote)
   - Erlang Debugger (:debugger)
   - erlyberly (subsequently found to be stale)
   - Og logging library
   - LiveBook debugging
   - Docker/Container debugging
   - Debugging anti-patterns

4. **User correction on erlyberly** - discovered project is abandoned

5. **Second expansion** replaced erlyberly with Modern Tracing Tools section:
   - Recon_TRACE (recon library)
   - Tracer (subsequently found to be stale)
   - OpenTelemetry/Telemetry
   - X-Trace
   - Observer Trace Overview

6. **Current verification in progress** - user questioning if tracer is current:
   - recon verified: v2.5.6, Aug 31, 2024 ✓
   - tracer verified: Sep 8, 2017 - STALE ✗
   - X-Trace verified: Apr 2023 announcement (unclear if maintained)

## Verified Tool Status

### Current/Maintained ✓
| Tool | Version | Last Updated | Status |
|------|---------|--------------|--------|
| recon | 2.5.6 | Aug 2024 | ACTIVE - Maintained by ferd, 97M+ downloads |
| dbg/2 | Built-in | Elixir v1.14+ | ACTIVE |
| Observer Trace | Built-in | BEAM built-in | ACTIVE |
| OpenTelemetry | Various | Active | ACTIVE |

### Stale/Abandoned ✗
| Tool | Last Updated | Status |
|------|-------------|--------|
| erlyberly | ~2019 | DEAD - GitHub 404 |
| tracer | Sep 2017 | ABANDONED - v0.1.1, removed from doc |

### Verified Current ✓
| Tool | Version | Last Updated | Status |
|------|---------|--------------|--------|
| recon | 2.5.6 | Aug 2024 | ACTIVE |
| X-Trace | v0.2.3 | Nov 2025 | ACTIVE - feng19/x_trace |
| Extrace | v0.3.0 | Feb 2025 | ACTIVE - redink/extrace |
| OpenTelemetry | Various | Active | ACTIVE |

## External References

- `https://hex.pm/packages/tracer` - STALE (Sep 2017)
- `https://hex.pm/packages/recon` - CURRENT (Aug 2024)
- `https://github.com/feng19/x_trace` - X-Trace (Apr 2023, unverified)
- `https://elixirforum.com/t/what-are-your-debugging-techniques/693` - Community discussion
- `https://elixirforum.com/t/remote-debugging-elixir-on-docker-container/39382` - Docker debugging

## Relevant Files

```
docs/AI-Research/elixir-debugging-tools.md   ← main doc (needs fixing)
~/.config/opencode/superpowers/skills/playwright-search/SKILL.md  ← created
```

## Completed Actions

- [x] Removed stale `tracer` (Sep 2017 - abandoned)
- [x] Verified X-Trace is ACTIVE (v0.2.3, Nov 2025)
- [x] Added Extrace as verified alternative (v0.3.0, Feb 2025)
- [x] Updated Modern Tracing Tools section with current info
- [x] Renumbered tooling list (1-5)

## Next Steps

- [ ] Commit changes to git
