---
title: 'Linux Tools & CLI Commands'
icon: material/linux
tags:
  - linux
  - cli
  - tools
  - agent-harness
  - terminal
date_added: 2026-10-03
last_updated: 2026-10-03
---

# Linux Tools & CLI Commands

My personal toolbox — the CLI tools I actually have installed and reach for, and
why I picked them.

This page exists for two reasons:

1. **Future me** — six months from now I will have forgotten which tool solved
   which problem, and what it replaced. The "Replaced" line is the valuable part.
2. **AI agents** — an agent reading this knows what the machine can do *before*
   it falls back to slow generic commands or writes a shell script for something
   a tool already handles.

## How to use this page

Copy the entry template below for each tool. Keep it short — an entry that takes
more than a few minutes to fill in will not get filled in.

Drop tools into the category that matches what they *do*, not what they are.
`fzf` is a finder, so it goes under File & Navigation even though it is often
called a "fuzzy selector".

If you cannot remember whether you installed something, put it in
[Still Need to Check](#still-need-to-check) at the bottom. That section is a
scratchpad — promote an entry out of it once you have confirmed the tool is real.

### Entry template

````markdown
### `tool-name`

One line on what problem it solves for me.

- **Replaces:** what I used before, or `nothing`
- **Install:** `apt install tool-name`
- **Config:** `~/.config/tool-name/config.toml` (or: none)

```bash
# the 2-3 commands worth remembering
tool-name --the-flag-i-forget
```

**Gotcha:** the thing that bit me.
````

---

## File & Navigation

<!-- eza, fd, fzf, tree, broot, ranger, lf, bat, exa, dust, duf -->

_Empty — to be filled._

## Search & Text Processing

<!-- ripgrep, ag, ack, jq, yq, sd, sed basics, awk basics, csvkit, miller/mlr -->

_Empty — to be filled._

## Git

<!-- lazygit, delta, git aliases, pre-commit, gh CLI, git worktree -->

Alias definitions live in a separate repo — see [Bash Aliases](../Notes/bashalias.md).

_Empty — to be filled._

## Terminal & Multiplexing

<!-- tmux, zellij, screen, tmate, asciinema -->

_Empty — to be filled._

## Shell & Environment

<!-- bash/zsh, starship, atuin, zoxide, direnv, fzf keybindings, mise, nix -->

### `mise`

Single tool that owns every language runtime on the machine. One `mise install`,
one `mise use`, and the right version of everything is active — no apt, no
`asdf`, no juggling `~/.bashrc` exports.

- **Replaces:** apt/asdf/rbenv-style per-language version managers
- **Install:** `curl https://mise.run | sh`
- **Config:** `~/.config/mise/config.toml`
- **Version:** 2026.1.2 (linux-x64)
- **Installs into:** `~/.local/share/mise/installs/<tool>/<version>`, shims in
  `~/.local/share/mise/shims`

What it currently manages for me:

| Tool | Version | Note |
| --- | --- | --- |
| elixir | 1.20.2 (otp-29) | paired with erlang 29.0.3 |
| erlang | 29.0.3 | see the Elixir entry for why the pair matters |
| elixir-ls | 0.31.1 | the actual Elixir language server |
| go | 1.27.1 | |
| node | 24.13.0 | only one pinned exactly, rest are `latest` |
| bun | 1.3.6 | |
| deno | 2.9.7 | |
| pnpm | 10.28.0 | |
| rebar | 3.27.0 | **required** — Erlang deps will not build without it |
| gleam | 1.15.2 | |
| zig | 0.15.2 | |
| just | 1.58.0 | command runner |
| lefthook | 2.1.2 | git hooks |
| eza | 0.23.4 | |
| dexter | 0.6.0 | Elixir semantic search, see Agent-Facing Tools |

```bash
mise ls                 # everything installed, and which config pins it
mise use -g erlang@29.0.3 elixir@1.20.2   # pin a pair globally
mise where elixir       # which install dir a tool resolves to
mise current            # what is active right now
```

**Gotcha:** setting `elixir` without pinning `erlang` lets mise pick two
mismatched majors. Elixir builds against a specific OTP, so always pin the pair
together — `elixir@1.20.2-otp-29` is the `-otp-NN` suffix doing that work. The
machine currently holds three such pairs: `1.18.4-otp-27`, `1.19.5-otp-28`,
`1.20.2-otp-29`.

**Gotcha:** `latest` in `config.toml` means a future `mise upgrade` moves
everything forward at once. Several tools here are on `latest`; only `node` is
pinned to an exact version. Pin the ones you care about reproducing.

### `uv`

Python project and package manager. Replaces `pip`, `pipx`, `virtualenv`, and
`poetry` for everything I actually needed.

- **Replaces:** `pip`, `pipx`, `virtualenv`, `poetry`
- **Install:** `curl -LsSf https://astral.sh/uv/install.sh | sh`
- **Config:** `pyproject.toml`; per-project `.python-version`
- **Version:** 0.11.26

```bash
uv venv                 # create .venv
uv pip install -r requirements.txt
uv run mkdocs build     # run inside the venv — no manual activate step
uv add <pkg>            # add dep and keep the lockfile in sync
```

**Gotcha:** this repo's `AGENTS.md` mandates `uv` for all Python work — plain
`pip`, `pip3`, `python -m pip`, and system Python are off-limits locally. CI
still uses `pip install -r requirements.txt` internally, which is fine because
that is the deploy pipeline, not local development.

**Gotcha:** prefer `uv run <cmd>` over `source .venv/bin/activate`. The activate
step is a shell-state mutation that does not survive a new terminal, and `uv run`
always resolves against the project venv.

## System & Processes

<!-- htop, btop, procs, dust/dua, glances, lsof, strace, tldr -->

_Empty — to be filled._

## Networking

<!-- curl, wget, httpie, dig, nc, nmap, ss, bandwhich, tailscale, netcat recipes -->

_Empty — to be filled._

## Disk & Filesystem

<!-- ncdu, dust, duf, mount, lsblk, rsync, restic, borg, rclone -->

_Empty — to be filled._

## Language Runtimes & Toolchains

<!-- elixir/erlang, go, rust, python/uv, zig, gleam, node, bun, deno -->

### `elixir` + `erlang`

My main language. Erlang and Elixir are installed as a **pair** through mise and
are never versioned independently — Elixir compiles against a specific OTP
release, so the two must move together.

- **Install:** `mise install elixir@1.20.2-otp-29 erlang@29.0.3` — the `-otp-NN`
  suffix is what binds them
- **Config:** `~/.config/mise/config.toml`
- **Versions:** Elixir 1.20.2 (compiled with Erlang/OTP 29), OTP 29 / erts-17.0.3,
  JIT enabled
- **Binaries:** `elixir`, `elixirc`, `iex`, `erl`, `erlc`, `erl_call`, plus
  `mix` and `mix.lock` tooling from rebar for deps

```bash
elixir --version        # confirms BOTH elixir and OTP in one line
iex                     # interactive shell
mix deps.get            # fetch deps
mix test                # run tests
mix compile --warnings-as-errors
```

Three Elixir/OTP pairs are installed side by side, so switching between a
project's pinned pair and latest is instant:

| Elixir | OTP |
| --- | --- |
| 1.18.4 | 27 |
| 1.19.5 | 28 |
| 1.20.2 | 29 (default) |

**Gotcha:** `rebar` must be installed or Erlang dependency resolution silently
fails. It comes from mise (`rebar@3.27.0`), not from apt.

**Gotcha:** `elixir --version` prints Erlang's line first. The Elixir version is
the *second* line — reading only the first line makes it look like `elixir
--version` is broken.

**Gotcha:** the language server is a separate tool from the language.
`elixir-ls` (see Agent-Facing Tools) is what an editor or agent talks to for
definitions and references; plain `elixir` is the compiler and runtime.

### `go`

Secondary language, used for small CLI tools and services.

- **Replaces:** distro `golang` package (too old, no version switching)
- **Install:** `mise install go@1.27.1` — do **not** apt install it
- **Config:** toolchain is controlled by `GOTOOLCHAIN`, not by mise
- **Version:** go1.27.1 linux/amd64, `GOPATH=/home/jnkk/go`

```bash
go version
go env GOPATH GOTOOLCHAIN     # GOTOOLCHAIN=auto — go can fetch its own toolchain
go build ./...
go test ./...
```

**Gotcha:** Go has *two* version mechanisms that fight each other. mise
installs the binary, but `GOTOOLCHAIN=auto` (the default) means a `go.mod`
declaring a newer Go will make go download and use *that* version instead,
silently bypassing mise. Pin `GOTOOLCHAIN=local` if I want mise to be the only
authority.

### `rust`

Used for performance-sensitive work and native tooling.

- **Install:** `rustup` — the one major runtime on this machine that is **not**
  managed by mise
- **Config:** `~/.cargo/`, plus `rust-toolchain.toml` per project for pinning
- **Versions:** rustup 1.29.1, rustc 1.98.1, cargo 1.98.1
- **Also in `~/.cargo/bin`:** `clippy`, `rustfmt`, `miri`, `rls`, `cargo-tauri`

```bash
cargo build
cargo test
cargo clippy -- -D warnings    # CI-grade lint gate
cargo fmt
cargo miri test                # checks for undefined behaviour
```

**Gotcha:** rust is the odd one out — everything else goes through mise, but
rustup owns its own toolchain directory. Do not `mise install rust`; the two
will fight over `PATH` and produce confusing "wrong version" reports.

**Gotcha:** `cargo miri` only works on nightly. On stable it errors out rather
than silently skipping, which is easy to mistake for a broken install.

## Containers & Dev Environments

<!-- docker, podman, devcontainer, nix, devenv, direnv -->

### `docker`

Container runtime for isolated dev environments and throwaway services.

- **Install:** Docker's own apt repo (do not use Debian/Ubuntu's `docker.io`)
- **Config:** `/etc/docker/daemon.json`
- **Versions:** Docker 29.8.2, Docker Compose v5.6.0

```bash
docker ps -a
docker compose up -d
docker compose logs -f <service>
docker exec -it <container> sh
```

**Gotcha:** Compose is a separate binary with its own version line — `docker
--version` says nothing about whether `docker compose` works. Check
`docker compose version` separately.

**Gotcha:** images are cached across projects and never pruned by default.
When disk pressure hits, `docker system df` first to see what is actually
consuming space, then prune deliberately.

## Editor

<!-- nvim/micro/helix, IDE configs, editor CLI invocation from a terminal -->

### `zed`

My editor. Chosen over the usual suspects because it treats AI agents as a
first-class feature rather than a plugin — the agent panel, the context server,
and the language server MCP bridge are all built in and configured from one
`settings.json`.

- **Install:** tarball to `~/.local/zed.app`, launcher symlinked to
  `~/.local/bin/zed`
- **Config:** `~/.config/zed/settings.json` (and optional `keymap.json`)
- **State:** `~/.local/share/zed/` — extensions, bundled language servers,
  `conversations/`, `external_agents/`
- **Version:** 1.20.2

```bash
zed --version
zed .                  # open current directory
```

Layout and theme, as configured:

| Setting | Value |
| --- | --- |
| `project_panel`, `outline_panel`, `collaboration_panel`, `git_panel` | all docked **right** |
| `agent` panel | docked **left**, `default_profile: "ask"` |
| theme | `One Light` / `Catppuccin Frappé` |
| `ui_font_size` / `buffer_font_size` | 16 / 15 |
| agent model | `MiniMax-M2.7` via OpenAI-compatible `https://api.minimax.io/v1` |

**Zed brings its own language servers** under `~/.local/share/zed/languages/`:
`bash-language-server`, `eslint`, `json-language-server`, `vtsls`, `yaml-language-server`,
`tailwindcss-language-server`, `package-version-server`. Those need no separate
install. Elixir is the exception — see below.

**Gotcha:** `settings.json` is **JSONC**, not JSON. The real file has `//` comments
and a trailing comma after `"Catppuccin Frappé",`. Anything that parses it as
strict JSON — a script, a CI step, an agent's config reader — will fail on it.

**Gotcha:** the default agent profile is `"ask"`, which only reads. Anything that
should be allowed to change files needs the profile switched to something like
`"edit"` first — an agent that silently cannot write looks identical to one that
chose not to.

### `expert` (expert-lsp)

Elixir language server built on the BEAM's own debugging tracer, so it indexes
actual runtime data rather than parsing source. Pairs with Zed via the `elixir-ls`
MCP bridge below.

- **Install:** `brew install expert` (Linuxbrew)
- **Version:** 0.1.8
- **Binary:** `/home/linuxbrew/.linuxbrew/bin/expert` →
  `Cellar/expert/0.1.8/libexec/expert/bin/start_expert`
- **Install dir:** `~/.local/share/Expert/`
- **Per project:** `.expert/` containing `expert.log` and a `.gitignore`

Present in at least these projects:

```
Projects/Phoenix/garis_miring      Projects/elixir/garismiring
Projects/Phoenix/symphony          Projects/Phoenix/prasyarat
Projects/Phoenix/loomkin
```

**Gotcha:** there are **two copies of expert** on this machine and `PATH` order
decides which wins:

| Source | Version | Notes |
| --- | --- | --- |
| Homebrew (Linuxbrew) | 0.1.8 | the one currently on `PATH` |
| burrito | 0.1.0 (`_erts-15.2.7`) | `~/.local/share/.burrito/expert_erts-15.2.7_0.1.0-9e913f6` |

The burrito copy targets **erts-15.2.7 (OTP 25)** while the active runtime is
OTP 29. If expert suddenly reports a version nobody installed, or behaves like a
much older release, this is why. `command -v expert` is the check.

**Gotcha:** the install directory `~/.local/share/Expert/0.1.0-9e913f6` reports
itself as `0.1.8` at runtime. The directory name is not a reliable version
indicator here — ask the binary.

**Gotcha:** `.expert/expert.log` is **gitignored** in every project. Expert's
diagnostics are therefore invisible in review and lost on a fresh clone. When
expert misbehaves, that log is the only evidence, so copy it out before cleaning.

### `elixir-ls` — the Zed ↔ expert bridge

Expert does not talk to Zed directly. Zed's `elixir-ls` entry is configured to
speak **MCP over TCP on port 4328**, which is what actually exposes expert's
tools to the agent:

```json
"lsp": {
  "elixir-ls": {
    "settings": {
      "mcpEnabled": true,
      "mcpPort": 4328
    }
  }
}
```

**Gotcha:** **port 4328 is shared with opencode.** `~/.config/opencode/opencode.json`
bridges the *same* elixir-ls MCP server to stdio on 4328. Only one process can
own that port, so if Zed is open and holding it, the opencode elixir-ls MCP fails
to connect — and the error points at opencode, not at Zed. Conversely, if
nothing is listening on 4328 (verified: nothing is, right now), then neither
editor has expert available and no amount of restarting the agent will help.

**Gotcha:** Serena runs in Zed too, via `context_servers.serena-context-server`
with `"enabled": true`. So the machine has *two* Serena instances — one inside
Zed, one as an opencode MCP server — pointed at the same project. Their indices
can disagree; when an agent gets a stale answer, check which host it was running
under before suspecting the code.

### `hexdocs_mcp` (via burrito)

Hex documentation lookup as an MCP server, so an agent can check real API
signatures instead of guessing or relying on training data.

- **Install:** burrito build — `~/.local/share/.burrito/hexdocs_mcp_erts-15.2.7_0.6.0/bin/hexdocs_mcp`
- **Version:** 0.6.0, built for erts-15.2.7

**Gotcha:** not on `PATH` — reachable only through its burrito install path.
Invoking bare `hexdocs_mcp` fails. Like expert, it is pinned to an older erts than
the active OTP 29 runtime.

**Gotcha:** because it is pinned to erts-15.2.7, it can serve docs for a Hex
version that does not match the OTP/elixir in play. Check the doc version matches
the project's actual dependency before trusting a signature.

## Agent-Facing Tools

<!-- Things that are specifically good for an AI agent to have: structured output,
     non-interactive, exits nonzero on failure, fast. ast-grep, sg, jq, yq,
     git, curl, sd, fzf, delta, lsp, serena, codegraph -->

Everything in this section is safe for an agent to invoke unattended. That is
the selection criterion — a tool lands here only if it does **not** need a TTY,
does **not** page, and exits nonzero on real failure. Interactive-only tools
belong in [TUI Reference](#tui-reference).

### `serena` (MCP)

Semantic code retrieval over a whole repository, exposed to agents as MCP tools.
Lets an agent jump to a symbol definition or find references without reading the
tree file by file.

- **Install:** `pipx install git+https://github.com/oraios/serena` (or `uv tool install`)
- **Config:** per-project `.serena/project.yml` and `.serena/project.local.yml`,
  with agent memories in `.serena/memories/`
- **Version:** 1.7.0

Registered in `~/.config/opencode/opencode.json`:

```json
{
  "mcp": {
    "serena": {
      "type": "local",
      "command": ["serena", "start-mcp-server", "--context", "ide", "--project-from-cwd"]
    }
  }
}
```

```bash
serena --version
```

**Gotcha:** `--project-from-cwd` means the project is inferred from the working
directory, not from a path argument. Launch the agent from the repo root or the
wrong project gets indexed — and it fails quietly, not loudly.

**Gotcha:** `.serena/memories/` holds accumulated project knowledge. It is
gitignored. That means the most valuable accumulated context is the first thing
lost on a fresh clone, and it cannot be reviewed in a PR.

**Gotcha:** Serena is configured in **two** places — as an opencode MCP server
(`opencode.json`) *and* as a Zed context server (`settings.json` →
`context_servers.serena-context-server`). Two instances, one project, indices that
can drift apart. See the Editor section.

### `dexter`

Elixir-specific semantic search — the same problem serena solves, but backed by
a real Elixir compiler index rather than text search. Understands modules,
functions, and references the way the BEAM does.

- **Install:** `mise install dexter@0.6.0`
- **Config:** none; indexes into a project-local store on `init`
- **Version:** 0.6.0

```bash
dexter init                       # full index of an Elixir project — run once
dexter lookup <Module>.<fun>      # where is it defined
dexter references <Module>.<fun>  # who calls it
dexter reindex                    # check all files for changes
dexter lsp                        # expose as an LSP server over stdio
```

**Gotcha:** the index is stale until `dexter reindex` runs. After editing files,
lookups can return the previous answer — which reads as "the refactor did not
work" rather than "the index did not update".

**Gotcha:** `dexter init` is a whole-project operation and is not instant on a
large umbrella app. `reindex` afterwards is cheap; re-running `init` is not.

### `elixir-ls`

The Elixir language server, driven by editors and agents over LSP. This is what
provides "go to definition", "find references", and diagnostics for Elixir code.

- **Install:** `mise install elixir-ls@0.31.1`
- **Version:** 0.31.1

Registered in `~/.config/opencode/opencode.json` — note it runs as a **TCP** LSP
bridged to stdio, not as a direct stdio server:

```json
{
  "mcp": {
    "elixir-ls": {
      "type": "local",
      "command": [
        "elixir",
        "/home/jnkk/.local/share/mise/installs/elixir-ls/0.31.1/tcp_to_stdio_bridge.exs",
        "4328"
      ]
    }
  }
}
```

**Gotcha:** that config hardcodes the elixir-ls version in the path. When mise
upgrades elixir-ls, this path breaks and the failure looks like a dead MCP server
rather than a stale version number. Re-copy the path from `mise where elixir-ls`
after upgrading.

### Other MCP servers configured

Not Linux tools per se, but part of the same agent harness, so recorded here:

| Server | Transport | Launched by |
| --- | --- | --- |
| `tidewave-mcp` | local proxy → `http://localhost:4000/tidewave/mcp` | `/home/jnkk/mcp/mcp-proxy` |
| `playwright` | local | `npx @playwright/mcp@latest` |
| `backlog` | stdio | `backlog mcp start` (Claude Code only) |
| `stitch` | http | `https://stitch.googleapis.com/mcp` |

**Gotcha:** `@latest` on the playwright MCP means the version can change under
you between sessions. If browser behaviour suddenly shifts, that is the first
thing to pin.

**Gotcha:** `tidewave-mcp` proxies to port 4000, so it is dead whenever that
process is not running — and the MCP client just reports a connection failure
with no hint that a separate process needs starting first.

## TUI Reference

<!-- Interactive tools that need a real terminal. Worth listing separately because
     they cannot run non-interactively, so an agent must know to avoid them. -->

_Empty — to be filled._

---

## Removed / Replaced

Tools I dropped and what replaced them. This is the section I read most often,
because it stops me from re-installing something on a new machine for no reason.

| Dropped | Replaced by | Why |
| --- | --- | --- |
| _TBD_ | | |

## Still Need to Check

Scratchpad — things I might have installed but have not confirmed or written up yet.

- [ ] **hindsight ai-mcp** — *not installed.* Mentioned as something I want for
      agent memory/context, but nothing is on the machine yet: no MCP entry in
      `~/.config/opencode/opencode.json`, no binary on `PATH`. Need to confirm the
      actual project name before writing an entry — do not trust the spelling
      until a repo or package page is verified.
- [ ] `_TBD_`

---

## Notes

Scratch space. Anything that does not fit above.

**Version snapshot taken 2026-10-03**, verified by running each tool on this
machine rather than from memory. If a version here looks stale, that is the
signal to re-run the command, not to assume the doc drifted.

Related: [Learning Agent Harness](README.md) · [Bash Aliases](../Notes/bashalias.md)
