---
title: OpenAI Symphony - AI Agent Orchestration for Phoenix Projects
icon: material/robot
tags:
  - ai
  - agents
  - orchestration
  - elixir
  - phoenix
  - openai
date_added: 2026-03-21
last_updated: 2026-03-21
---

# OpenAI Symphony: AI Agent Orchestration for Phoenix Projects

## Executive Summary

**OpenAI Symphony** is an open-source orchestration service that transforms your Linear issue tracker into an autonomous coding agent factory. It polls your project board, spawns isolated AI agents (OpenAI Codex) for each issue, and delivers verified pull requests—all without manual developer prompting.

For Phoenix/Elixir projects, Symphony offers a compelling architecture built on the BEAM runtime, featuring Phoenix LiveView for its observability dashboard. This research explores what Symphony is, how it works, and how Phoenix developers can leverage its patterns.

## What is Symphony?

### Core Concept

Symphony is a **long-running automation service** that implements what OpenAI calls **"Harness Engineering"**—a discipline of designing infrastructure, constraints, and feedback loops that make AI agents reliably productive.

Unlike code assistants that wait for prompts, Symphony represents a paradigm shift:

```
Code completion (2021-2023): AI suggests next line → GitHub Copilot
Code conversation (2023-2025): AI discusses and modifies code → Claude Code, Cursor  
Code orchestration (2025+): AI autonomously processes project work → Symphony
```

### Key Statistics
- **Stars**: 13.7k+
- **Forks**: 1.1k+
- **Primary Language**: Elixir (95.4%)
- **License**: Apache 2.0
- **Release Date**: March 5, 2026

## Architecture Deep Dive

### Why Elixir/BEAM?

The choice of Elixir over Python is the most architecturally significant decision:

| Requirement | BEAM Feature |
|-------------|--------------|
| Concurrent agent management | Lightweight processes (millions per node) |
| Fault tolerance | Supervisor trees auto-restart crashed processes |
| Message passing | Actor model for agent communication |
| Self-healing | OTP supervision principles |

**The BEAM runtime** was designed for telecom infrastructure handling thousands of simultaneous processes with isolated failure domains. In a telecom switch, one call failing shouldn't cascade—but it also shouldn't restart the entire switch. This mirrors exactly what AI agents need: isolation without total system failure.

### System Components

```
┌─────────────────────────────────────────────────────────────┐
│                    Symphony Service (Elixir/OTP)            │
├─────────────────────────────────────────────────────────────┤
│  Workflow Loader  │  Config Layer  │  Orchestrator (GenServer) │
├─────────────────────────────────────────────────────────────┤
│  Issue Tracker Client  │  Workspace Manager  │  Agent Runner │
├─────────────────────────────────────────────────────────────┤
│  Status Surface (Phoenix LiveView)  │  Structured Logging   │
└─────────────────────────────────────────────────────────────┘
```

### Six-Layer Architecture (Language-Agnostic)

1. **Policy Layer** (`WORKFLOW.md`) - Team's rules encoded in markdown
2. **Configuration Layer** - Typed runtime settings from YAML front matter
3. **Coordination Layer** - Orchestrator managing polling, concurrency, retries
4. **Execution Layer** - Workspace and agent subprocess management
5. **Integration Layer** - Linear API via GraphQL
6. **Observability Layer** - Phoenix LiveView dashboard + structured logs

## Phoenix Integration Details

### Phoenix LiveView Dashboard

Symphony's observability UI runs on a **minimal Phoenix stack**:

- **LiveView** for real-time dashboard at `/`
- **JSON API** for operational debugging at `/api/v1/*`
- **Bandit** as the HTTP server
- **Phoenix** static assets for LiveView client bootstrap

```bash
# Start with Phoenix dashboard on port 4000
./bin/symphony --port 4000
```

The dashboard provides:
- Real-time agent status monitoring
- Issue progress tracking
- Token usage metrics
- Error logging and recovery status

### Project Structure

```
symphony/elixir/
├── lib/
│   ├── symphony_elixir/          # Core orchestrator, GenServer polling loop
│   │   ├── orchestrator.ex       # State machine tracking issue lifecycle
│   │   ├── workflow_store.ex     # Config hot-reload
│   │   ├── workspace_manager.ex  # Per-issue workspace isolation
│   │   └── codex_runner.ex       # Agent subprocess management
│   └── symphony_elixir_web/     # Phoenix LiveView dashboard
│       ├── live/
│       │   └── dashboard_live.ex
│       └── router.ex
├── WORKFLOW.md                   # In-repo workflow contract
└── mix.exs                       # Dependencies including Phoenix
```

### Phoenix-Specific Patterns Used

1. **Phoenix.PubSub** for internal event distribution
2. **Phoenix.LiveView** for real-time dashboard updates
3. **Phoenix.HTML** for dashboard templates
4. **Bandit** (Phoenix's HTTP server) for the observability endpoint

## WORKFLOW.md: The In-Repo Contract

The heart of Symphony is the `WORKFLOW.md` file—your team's version-controlled agent contract:

```yaml
---
tracker:
  kind: linear
  api_key: $LINEAR_API_KEY
  project_slug: "your-project-slug"
  
workspace:
  root: ~/code/workspaces
  hooks:
    after_create: |
      git clone git@github.com:your-org/your-repo.git .
      mix deps.get
      mix compile

agent:
  max_concurrent_agents: 10
  max_turns: 20

codex:
  command: "codex app-server --model gpt-5.3-codex"
  approval_policy: "on-failure"
  turn_sandbox_policy: "workspaceWrite"

polling:
  interval_ms: 60000
---

You are working on Linear issue {{ issue.identifier }}.

Title: {{ issue.title }}
Body: {{ issue.description }}

Requirements:
- Follow project conventions (see CONTRIBUTING.md)
- Write tests for all new functionality
- Ensure CI passes before marking complete
```

## How It Works for Phoenix Projects

### Setup for Phoenix Projects

1. **Copy WORKFLOW.md** to your Phoenix project root
2. **Configure** the tracker, workspace, and agent settings
3. **Add skills** (commit, push, pull, land, linear) to your `.codex/` directory
4. **Set environment variables**: `LINEAR_API_KEY`, `OPENAI_API_KEY`
5. **Start Symphony**: `./bin/symphony ./WORKFLOW.md --port 4000`

### What Happens

1. **Poll** Linear every N seconds for new issues with specific labels
2. **Create** isolated workspace: `~/code/workspaces/ABC-123/`
3. **Clone** your Phoenix project into the workspace
4. **Bootstrap** with `mix deps.get && mix compile`
5. **Launch** Codex in App Server mode with your workflow prompt
6. **Agent reads** issue, writes code, runs tests
7. **Agent commits** with proper conventions
8. **Agent opens PR** with description and test results
9. **If issue moves to terminal state** (Done/Closed), cleanup workspace

### Phoenix-Specific Benefits

| Phoenix Pattern | Symphony Benefit |
|-----------------|------------------|
| Mix tasks | Agents can run `mix phx.server`, `mix test` |
| Ecto schemas | Agents understand your data model |
| LiveView components | Agents build reactive UIs |
| Contexts | Agents work within bounded contexts |
| Testing (ExUnit) | Agents verify with `mix test` |

## Competitive Landscape

Symphony enters a consolidating market:

| Platform | Approach | Open Source |
|----------|----------|-------------|
| **Symphony** | Issue-to-PR automation | ✅ Apache 2.0 |
| **GitHub Copilot for Jira** | IDE-integrated agent | ❌ SaaS |
| **Cursor Automations** | Event-driven AI agents | ❌ SaaS |
| **Blitzy** | Full autonomous generation | ❌ SaaS |
| **Anthropic Code Review** | Parallel review agents | ❌ SaaS |

**Symphony's advantage**: Self-hostable, enterprise-friendly, language-agnostic spec.

## Limitations & Considerations

### Current Limitations (as of March 2026)

1. **Linear only**: GitHub Issues and Jira adapters not yet built
2. **Codex only**: Other agents would need custom runners
3. **Elixir dependency**: Reference implementation requires BEAM
4. **Early stage**: Expect rough edges, breaking changes
5. **Trusted environments only**: Not hardened for adversarial use

### Security Considerations

- Agents run with workspace permissions only
- Approval policies configurable (`on-failure`, `on-request`, `never`)
- Thread sandboxing (`read-only`, `workspace-write`, `danger-full-access`)
- SSH worker support for remote execution

## For Phoenix Developers: Practical Takeaways

### Patterns to Adopt

1. **Version-controlled workflows**: Store `WORKFLOW.md` in your repo
2. **Supervision trees**: Symphony's GenServer patterns mirror Phoenix's approach
3. **Hot configuration reload**: Workflow changes without restart
4. **Isolated workspaces**: One issue = one directory = one agent
5. **Structured observability**: Phoenix LiveView for real-time dashboards

### Integration Opportunities

```elixir
# Your Phoenix app could expose Symphony metrics
defmodule MyPhoenixAppWeb.Endpoint
  socket "/live", Phoenix.LiveView.Socket
  socket "/symphony/metrics", SymphonyMetricsSocket
end

# Subscribe to Symphony events
Phoenix.PubSub.subscribe(MyPhoenixApp.PubSub, "symphony:events")
```

### Code Examples

#### Phoenix LiveView Dashboard Component

```elixir
defmodule SymphonyElixirWeb.DashboardLive do
  use Phoenix.LiveView
  
  def mount(_params, _session, socket) do
    if connected?(socket) do
      Phoenix.PubSub.subscribe(Symphony.PubSub, "orchestrator:state")
    end
    
    {:ok, assign(socket, :state, Orchestrator.get_state())}
  end
  
  def render(assigns) do
    ~H"""
    <div class="symphony-dashboard">
      <h1>Symphony Orchestrator</h1>
      <.live_component module={AgentStatusComponent} agents={@state.running} />
      <.live_component module={RetryQueueComponent} retries={@state.retry_attempts} />
    </div>
    """
  end
end
```

## Getting Started

### Prerequisites

- Elixir 1.15+ and Erlang/OTP 26+
- Linear account with API access
- OpenAI API key (for Codex)
- `mise` for Elixir/Erlang version management

### Quick Start

```bash
# Clone Symphony
git clone https://github.com/openai/symphony.git
cd symphony/elixir

# Setup
mise trust
mise install
mix setup
mix build

# Configure
export LINEAR_API_KEY="lin_api_xxxxx"
export OPENAI_API_KEY="sk-xxxxx"

# Run
./bin/symphony ./WORKFLOW.md --port 4000
```

## Conclusion

OpenAI Symphony represents a significant validation of Elixir/BEAM for AI agent orchestration. For Phoenix developers, it offers:

1. **Production-grade patterns** for concurrent, fault-tolerant services
2. **Phoenix LiveView** integration example for real-time dashboards
3. **Supervision tree** patterns for agent lifecycle management
4. **In-repo workflow contracts** for team collaboration

While still experimental, Symphony's architecture provides a blueprint for building agentic systems in Elixir. The language-agnostic `SPEC.md` also means you can implement the orchestration layer in any language while using Phoenix for the observability dashboard.

## Further Resources

- [Symphony GitHub Repository](https://github.com/openai/symphony)
- [SPEC.md - Full Specification](https://github.com/openai/symphony/blob/main/SPEC.md)
- [Harness Engineering - OpenAI](https://openai.com/index/harness-engineering/)
- [Codex App Server Mode](https://developers.openai.com/codex/app-server/)

---

*Researched via: GitHub repository analysis, SPEC.md documentation, official README, community discussions, and third-party analysis articles.*
