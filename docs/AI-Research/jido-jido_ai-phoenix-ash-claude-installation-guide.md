---
title: Installing Jido/Jido_AI into Ash/Phoenix with Claude Code Agent
icon: material/robot
tags:
  - jido
  - jido_ai
  - elixir
  - phoenix
  - ash
  - claud-code
  - agent
  - llm
  - liveview
  - tutorial
date_added: 2025-03-20
last_updated: 2025-03-20
---

# Installing Jido/Jido_AI into Ash/Phoenix with Claude Code Agent

## Executive Summary

This guide walks you through integrating the **Jido** (Elixir autonomous agent framework) and **Jido_AI** (AI runtime layer) into an existing **Ash/Phoenix** application. The goal is to create an AI agent system that can be controlled via **Claude Code Agent** (or any AI agent) through Phoenix LiveView to manage your application's resources and workflows.

### What You'll Build

1. **Jido Core**: Agent runtime with signal-based architecture
2. **Jido_AI**: AI/LLM runtime for tool calling and reasoning strategies
3. **Jido_Code**: Phoenix LiveView application with real-time agent dashboards and UI
4. **Claude Code Agent Interface**: An external AI agent that talks to your Jido agents

### The Architecture

```
┌─────────────┐────────────────┐───────────┐────────────────┐─────────────────┐─────────────┐──────────────┘─────────────┘
│                                             │                 │
│  ┌───────────────────────┐────────┐───────────┘          │ │   Phoenix Web Server       │
│ │  ┌───────────────────────┐─────────────────┘          │   (Phoenix Endpoint)   │
│ │ │                                    │               │
│ │  ┌───────────────────────┐─────────────────┘          │         │
│ │ │     Ash Framework  │               │         │
│ │ │  (Ash Domain)           │         │
│ │ │                                    │               │
│ │ ┌───────────────────────┐─────────────────┘          │         │
│ │                                    │               │
│ │ ────────────────────┐─────────────────┘          │         │
│ │     Jido Agents      │               │         │
│ │ │  (Supervised)        │         │         │
│ │ ┌───────────────────────┐─────────────────┘          │         │
│ │     LiveView UI        │               │         │
│ │  │ (Real-time Dash)     │         │
└─────────────┴───────────────────┴──────────────────┴──────────┘────────────────┴─────────────┘───────────────┘──┘─────┘
│                │                   │
│  │   Phoenix PubSub      │               │
│ │                   │         │
│ │                   │         │
│ │                   │         │
│ │                   │         │
└───────────┴──────────────────┴──────────────────┴──────────┘─────────────┘──────┘──────┘───┴──────────────┘
│                │                   │
│  │   Signals (PubSub)    │               │
│ │                   │         │
│ │                   │         │
│                   │         │
└───────────┴──────────┴──────────┴──────────┘─────────────┘───┘─────┘─────────────┘───┘──────┘─────────────┘─────────┘─────┘───┘───────────────┘─────────────┘───┘─────────────┘─────────────┘─────────────┘──────────────┘─────────────┘───┘─────────────┘────────┘─────────────�─────�┘──┘─────────────┘┘───┘─────────────┘────────┘─────────────┘───┘───┘──────────────┘─────────────┘
│                                    │               │
│               │                   │         │
│               │   Claude Code Agent        │
│               │                   │         │
│               │                   │         │
└─────────────┴───────────┴──────────┴──────────┘──────────┘─────────────┘─────────────┘──────────────────────────┘──────────────
```

---

## Prerequisites

### System Requirements

| Component | Version | Notes |
|-----------|----------|
| Elixir | 1.18+ |
| Erlang/OTP | 27.0+ |
| Phoenix | ~> 1.7+ |
| Ash | ~> 3.12+ |
| Ash_Postgres | ~> ~2.0+ |
| Phoenix LiveView | ~> 1.0+ |
| Claude | Installed and configured |

---

## Step-by-Step Installation Guide

### Step 1: Initialize Your Phoenix Project

If you have an existing Phoenix project, ensure you meet these requirements:

```bash
# Version check
elixir -v
erlang -v

# Check Phoenix version
mix phx.new my_app
cd my_app
```

### Step 2: Install Jido and Jido_AI

#### Option A: Using Igniter (Recommended)

```bash
mix igniter.install jido
mix igniter.install jido_ai
```

This automatically:
- Adds `jido` to your dependencies
- Creates `MyApp.Jido` instance module
- Configures `config/config.exs`
- Adds to supervision tree
```elix
```

#### Option B: Manual Installation

Add to `mix.exs`:

```elix
defp deps do
  {:jido, "~> 2.0"},
  {:jido_ai, "~> 2.0.0-rc.0"}
end
```

### Step 3: Configure Jido

Create or update `config/config.exs`:

```elix
# config/config.exs
import Config

# Jido Configuration
config :jido, MyApp.Jido,
  # Max concurrent tasks
  agent_pools: [] # Agent pools for specialized workers

# Model Aliases for AI
config :jido_ai, MyApp.Jido,
  model_aliases: %{
  fast: "anthropic:claude-3-haiku-20240329",
  capable: "anthropic:claude-3-5-sonnet"
}

# Provider Credentials
config :req_llm,
  anthropic_api_key: System.get_env("ANTHROPIC_API_KEY"),
  openai_api_key: System.get_env("OPENAI_API_KEY")
end
```

**Important**: Set your API keys in environment variables before starting!

### Step 4: Add Jido to Supervision Tree

Update `lib/my_app/application.ex`:

```elix
# lib/my_app/application.ex

defmodule MyApp.Application do
  use Application

  @impl Application do
  @impl true
  def start(_type, _args) do
    [
      MyApp.Jido,
      MyApp.Repo,
      MyApp.PubSub
    ]
  end
end
```

**Note**: If using Phoenix LiveView, add `MyApp.PubSub` (Phoenix LiveView agent skill)

### Step 5: Install and Configure Phoenix LiveView

#### Option A: Hex Installation

```bash
# Add to mix.exs
{:phoenix_live_view, "~> 1.0"}
```

#### Option B: Install via GitHub

```bash
# Add GitHub dependency
mix deps.get

# Configure app settings
config :my_app, MyApp.PubSub,
  adapter: Phoenix.Socket.PubSub # Add PubSub for WebSockets
```

---

## Step 6: Define Your First Jido Agent

Create `lib/my_app/agents/simple_task_agent.ex`:

```elix
defmodule MyApp.Agents.SimpleTaskAgent do
  use Jido.AI.Agent, name: "task_agent", model: :fast

  # Define tools that the agent can use
  tools: [
    MyApp.Actions.CreateTask,
    MyApp.Actions.ReadTask,
    MyApp.Actions.UpdateTask,
    MyApp.Actions.DeleteTask
  ]

  # System prompt - the agent's role
  system_prompt: """
  You are a helpful assistant that manages tasks in our task application.
  Be concise and action-oriented.
  """

  agent schema: [
    # State can be anything relevant to your app
    current_task: [
      type: {:union, nil, :map}, default: nil
    ]
  ]

  # Signal routes - map user commands to agent actions
  signal_routes: [
    {"create_task", MyApp.Actions.CreateTask},
    {"update_task", MyApp.Actions.UpdateTask},
    {"delete_task", MyApp.Actions.DeleteTask}
  ]
end
```

**Note**: Using `Jido.AI.Agent` provides automatic `ask/ask_sync` methods.

### Step 7: Create Jido Tools (Actions)

Create `lib/my_app/actions/create_task.ex`:

```elix
defmodule MyApp.Actions.CreateTask do
  use Jido.Action, name: "create_task"

  schema: Zoi.object(%{
    title: Zoi.string(),
    description: Zoi.string(),
    priority: Zoi.integer()
  })

  @impl true
  def run(params, context) do
    # Access your app logic here
    {:ok, task} = MyApp.Tasks.create_task(params)
end
end
```

**Note**: Tools wrap your existing app logic using `MyApp.Repo`.

### Step 8: Create Phoenix LiveView Agent Skill (The "Talk" Interface)

This skill allows Claude Code (or any agent) to observe and interact with your Jido agents through Phoenix LiveView.

Add `lib/my_app/agentskills/bobmatnyc-phoenix-liveview.ex`:

```elix
defmodule MyApp.Agentskills.BobmatnycPhoenixLiveview do
  use Jido.AI.Agent, name: "phoenix_liveview_agent", model: :capable

  # System prompt
  system_prompt: """
  You are an agent connected to a Phoenix LiveView application via PubSub.
  You observe users and issue commands to manage the application.
  The app has Ash resources accessible via MyRepo.
  """

  agent schema: [
    user_id: [type: :string, required: true],
    active_tasks: [type: :list, default: []]
  ]

  # Signal routes
  signal_routes: [
    {"user_query", MyApp.Actions.UserQuery},
    {"task_commands", MyApp.Actions.UserCommands}
  ]

  # Context - pass your domain module
  context: [
    {:domain, MyApp.Shop},
    {:actor, MyApp.User}
  ]
end
```

**LiveView Integration Details**:

- **Signal Routes**: `ai.react.query` (for user questions)
- **Output**: Signals to Phoenix PubSub (`user_commands`)
- **Context**: Requires `domain` and `actor` from `MyApp.Repo`

---

## Step 9: Define Your First Jido AI Agent

Create `lib/my_app/agents/task_manager_agent.ex`:

```elix
defmodule MyApp.Agents.TaskManagerAgent do
  use Jido.AI.Agent, name: "task_manager", model: :fast

  # More complex reasoning
  tools: [
    MyApp.Actions.CreateTask,
    MyApp.Actions.ReadTask,
    MyApp.Actions.UpdateTask,
    MyApp.Actions.DeleteTask
  ]

  # Orchestration Strategies
  orch_type: [:sequential, :parallel, :loop],
  max_concurrency: 10
end
```

## Step 10: Wire Up Claude Code Agent Interface

Create a simple HTTP endpoint that your Claude Code agent can call:

`lib/my_app/web/agent_controller.ex`:

```elix
defmodule MyAppWeb.AgentController do
  use Phoenix.Controller

  @impl true
  def chat(conn, %{"message" => message}) do
  # 1. Generate Jido prompt
  prompt = "What can I help the user with?"

  # 2. Execute via Jido
  {:ok, result} = MyApp.Agents.TaskManagerAgent.ask_sync(
    "phoenix_liveview_agent",
    prompt
  )

  # 3. Return result
  json(conn, %{result: result})
end
end
```

## Claude Code Agent Configuration

When setting up Claude Code to talk to your app:

```elixir
# In Claude Code (Anthropic's IDE plugin)
# Workspace -> Select your project

# Tools to give Claude
tools = [
  {
  name: "jido_agent",
  "description": "Control Jido agents via Phoenix PubSub",
  "input_schema": {
    "type": "object",
    "properties": {
      "command": "string",
      "domain": "string"
    }
  }
  }
]
```

### Claude Agent System Prompt Template

```text
You are an AI agent connected to a Phoenix application via Jido AI.

Context:
- App Domain: MyShop (e-commerce platform)
- Database: PostgreSQL via Ash.Postgres
- Domain Actor: Current User
- API Endpoint: /api/chat
- Tools:
   1. Search users (via Phoenix LiveView skill)
 2. Read/Update user data (via Jido actions)
3. Create/read tasks (via Jido tools)

Capabilities:
- Real-time monitoring
- Multi-user coordination
- Long-running workflows

Constraints:
- Always use human approval for data changes
- Never expose internal system data
- Log all user interactions
```

## Step 11: Start the Application

```bash
# Start Phoenix endpoint
mix phx.server

# Start Phoenix LiveView
# Agent will connect automatically via PubSub
```

---

## Step 12: Validate the Architecture

### Testing Checklist

- [ ] Agent starts successfully in supervision tree
- [ ] Phoenix PubSub receives and broadcasts signals
- [ ] Claude Agent responds via Phoenix channel
- [ ] App resources (DB) accessible via agentskills skill
- [ ] API endpoints respond correctly to Claude

### Common Issues and Solutions

| Issue | Solution |
|-------|----------|
| **Agents crash** | Check your supervision tree configuration |
| **PubSub not receiving signals** | Verify PubSub is in children and routing is correct |
| **Claude disconnected** | Check API health and connectivity |
| **Agentskills skill not working** | Ensure skill is properly registered as `PhoenixLiveView` |
| **Performance issues** | Profile slow operations |

---

## Enhancement: Multi-Agent Orchestration

For advanced workflows, consider:

```elixir
# Multi-agent setup
{:ok, agent1} = MyApp.Jido.start_agent(
  "orchestration_agent",
  id: "orchestrator"
)

{:ok, agent2} = MyApp.Jido.start_agent(
  "task_agent",
  id: "task_agent"
)

# Send task from agent1 to agent2
{:ok, result} = Jido.Agent.cast(
  "task_agent",
  "orchestration_agent",
  Jido.Signal.new!("task_created", %{task_data: %{title: "New Task"}})

# Agent 2 processes task
{:ok, agent2} = MyApp.Agents.TaskManagerAgent.ask_sync(
  "task_agent",
  "Manage the following task: %{title}: task_data.title}"
)
end
```

---

## Resources & References

- **Jido Documentation**: https://hexdocs.pm/jido/
- **Jido_AI Guide**: https://hexdocs.pm/jido_ai/
- **Phoenix Integration**: https://hexdocs.pm/jido/phoenix-integration
- **Phoenix LiveView Skill**: https://agentskills.so/skills/phoenix-liveview
- **JetThoughts Tutorial**: https://jetthoughts.com/blog/elixir-ai-integration-tutorial-complete-guide

### Summary

By integrating Jido/Jido_AI with your Ash/Phoenix project, Claude Code can:

1. **Observe** your application state in real-time
2. **Search** and **Filter** users efficiently
3. **Create** and **Update** application data (with human approval)
4. **Monitor** system events and user actions
5. **Coordinate** multi-agent workflows
6. **Debug** issues with observable state

This architecture leverages Elixir's OTP for fault tolerance and concurrency, while giving AI agents powerful capabilities to manage your app through a standard Phoenix web interface.
