---
title: Elixir AI Tools for Development
icon: material/robot
tags:
  - elixir
  - ai
  - development-tools
  - debugging
  - agents
date_added: 2026-03-19
last_updated: 2026-03-19
---

# Elixir AI Tools for Development

> Researched via Playwright MCP web searches. Last updated: 2026-03-19
> **Note**: Only tools with recent activity (≤ 3 years old) are included.

## Overview

The Elixir ecosystem has a growing set of AI-powered tools for development assistance, debugging, code analysis, and agent orchestration. This document catalogs current, maintained tools.

## Categories

1. [AI Coding Agents & Assistants](#ai-coding-agents--assistants)
2. [AI Debugging Tools](#ai-debugging-tools)
3. [AI Code Analysis & Static Analysis](#ai-code-analysis--static-analysis)
4. [AI SDKs & Infrastructure](#ai-sdks--infrastructure)
5. [AI Agent Frameworks](#ai-agent-frameworks)
6. [MCP (Model Context Protocol) Servers](#mcp-model-context-protocol-servers)
7. [AI-Friendly Testing Tools](#ai-friendly-testing-tools)

---

## AI Coding Agents & Assistants

### Sagents
- **GitHub**: [sagents-ai/sagents](https://github.com/sagents-ai/sagents)
- **Stars**: 167 | **Forks**: 16
- **Description**: Build interactive AI agents in Elixir with OTP supervision, middleware composition, human-in-the-loop approvals, sub-agent delegation, and real-time Phoenix LiveView integration. Built on LangChain.
- **Status**: Active - v0.3.1 (Mar 2026)

### Alloy
- **GitHub**: [alloy-ex/alloy](https://github.com/alloy-ex/alloy)
- **Stars**: 41 | **Forks**: 2
- **Hex**: [hex.pm/alloy](https://hex.pm/packages/alloy)
- **Description**: Model-agnostic agent harness for Elixir. Minimal, OTP-native agent loop. Completion-tool-call loop and nothing else.
- **Status**: Active - v0.7.4 (Mar 2026)

### Shifts
- **GitHub**: [aaronrussell/shifts](https://github.com/lebrunel/shifts)
- **Stars**: 37
- **Description**: Elixir framework for composing autonomous AI agent workflows using LLM backends.
- **Status**: Last push Jun 2024

### Hive
- **GitHub**: [chrisgreg/hive](https://github.com/chrisgreg/hive)
- **Stars**: 8
- **Description**: Composable agent framework for Elixir: Build autonomous, schema-validated processing pipelines with retry mechanisms and LLM routing.
- **Status**: Active - last push Jul 2025

### ExCode (elixir-coding-agent)
- **GitHub**: [biraj21/ex-coding-agent](https://github.com/biraj21/ex-coding-agent)
- **Stars**: 5
- **Description**: A coding agent REPL that connects to OpenAI-compatible APIs. Reads files, edits code, runs commands.
- **Status**: Very new - created Mar 2026

### Rubber Duck
- **GitHub**: [pcharbon70/rubber_duck](https://github.com/pcharbon70/rubber_duck)
- **Stars**: 4
- **Description**: A concurrent, fault-tolerant and pluggable AI coding assistant in Elixir.
- **Status**: Active - Aug 2025

### AlexClaw
- **GitHub**: [thatsme/AlexClaw](https://github.com/thatsme/AlexClaw)
- **Stars**: 22 | **Forks**: 1
- **Description**: BEAM-native personal AI agent built on Elixir/OTP. Monitors RSS feeds, web sources, GitHub repos, APIs.
- **Status**: Very active - v0.2.2 (Mar 2026), pushed today

### NexAgent
- **GitHub**: [gofenix/nex-agent](https://github.com/gofenix/nex-agent)
- **Description**: Self-evolving AI agent system built on Elixir/OTP. Aimed at long-running, real-world use cases with persistent sessions and memory.
- **Status**: Active - discussed on ElixirForum Mar 2026

### AI Intent Driven Development
- **GitHub**: [Exadra37/ai-intent-driven-development](https://github.com/Exadra37/ai-intent-driven-development)
- **Stars**: 29
- **Description**: AI Intent Driven Development (IDD) guidelines and instructions for AI Coding Agents, AI Coding Assistants, and LLMs.
- **Status**: Active - Jan 2026

### Autoforge
- **GitHub**: [iotrak/autoforge](https://github.com/iotrak/autoforge)
- **Description**: AI coding assistant for Ash/Phoenix/Elixir.
- **Status**: Active - Mar 2026

### ElixirClaw
- **GitHub**: [developerfred/ElixirClaw](https://github.com/developerfred/ElixirClaw)
- **Stars**: 11
- **Description**: High-performance OpenClaw Node implemented in pure Elixir. Connect device to OpenClaw Gateway, expose AI capabilities.
- **Status**: Active - Feb 2026

### JidoCode
- **GitHub**: [agentjido/jido_code](https://github.com/agentjido/jido_code)
- **Stars**: 7
- **Description**: Elixir/Phoenix + LiveView application exploring a practical "AI coding orchestrator" built on the Jido agent runtime.
- **Status**: Active - v0.2.2 (Mar 2026)

### Agent Session Manager
- **Hex**: [hex.pm/packages/agent_session_manager](https://hex.pm/packages/agent_session_manager)
- **Downloads**: 72K all time, 17K last 30 days
- **Description**: Comprehensive Elixir library for managing AI agent sessions, state persistence, conversation context, and multi-agent orchestration workflows.
- **Status**: Very active - v0.8.0 (Feb 2026), 12 versions released

---

## AI Debugging Tools

### ElixirScope
- **GitHub**: [nshkrdotcom/ElixirScope](https://github.com/nshkrdotcom/ElixirScope)
- **Stars**: 5
- **Description**: AI-Powered Execution Cinema Debugger for Elixir/BEAM.
- **Status**: Active - May 2025

### Sagents Live Debugger
- **GitHub**: [sagents-ai/sagents_live_debugger](https://github.com/sagents-ai/sagents_live_debugger)
- **Stars**: 18
- **Description**: A Phoenix LiveView dashboard for debugging and monitoring Sagents agents in real-time.
- **Status**: Very active - updated Mar 2026 (5 days ago)

### StacktraceGpt
- **Hex**: [hex.pm/packages/stacktrace_gpt](https://hex.pm/packages/stacktrace_gpt)
- **Downloads**: 766 all time
- **Description**: Helps analyze Elixir/Phoenix stacktraces with help of ChatGPT.
- **Status**: Stale - last updated Nov 2023

### ExUnit JSON (AI-friendly test output)
- **Hex**: [hex.pm/packages/ex_unit_json](https://hex.pm/packages/ex_unit_json)
- **Description**: AI-friendly JSON test output for ExUnit. Structured output for AI editors like Claude Code, Cursor.
- **Features**: Deterministic test ordering, coverage gating, AI-optimized defaults

### Ksomnia
- **GitHub**: [ksomnia/ksomnia](https://github.com/ksomnia/ksomnia)
- **Stars**: 2
- **Description**: Bug catching web application with LLM integration for intelligent fix suggestions and troubleshooting advice.
- **Status**: Stale - last push Aug 2023

---

## AI Code Analysis & Static Analysis

### Assay
- **GitHub**: [Ch4s3/assay](https://github.com/Ch4s3/assay)
- **Stars**: 5
- **Description**: Elixir wrapper around Incremental Dialyzer with LLM-focused features. Reads Dialyzer settings from mix.exs, runs incremental engine, emits output for humans, CI, editors, and LLM-driven tools.
- **Features**: `mix assay`, watch mode, multiple output formats
- **Status**: Active - v0.3.0 (Jan 2026), pushed Mar 2026

### Codicil
- **GitHub**: [E-xyza/codicil](https://github.com/E-xyza/codicil)
- **Stars**: 44 | **Forks**: 4
- **Description**: Advanced LLM MCP addon for Elixir. Semantic code search and analysis for Elixir projects via MCP (Model Context Protocol).
- **Status**: Active - last push Nov 2025

### Fnord
- **Hex**: [hex.pm/packages/fnord](https://hex.pm/packages/fnord)
- **Downloads**: 5.4K all time
- **Description**: AI code archaeology - helps understand legacy code.

### Cicada MCP Server
- **Website**: [antigravity.codes/mcp/cicada](https://antigravity.codes/mcp/cicada)
- **Description**: AST-powered code intelligence server for Elixir. 9 tools including function search, call site tracking, PR attribution, git history, semantic search. Reduces AI query tokens by 82%.

---

## AI SDKs & Infrastructure

### Claude Agent SDK
- **Hex**: [hex.pm/packages/claude_agent_sdk](https://hex.pm/packages/claude_agent_sdk)
- **Downloads**: 3.3K all time, 317 last 7 days
- **Description**: Elixir SDK for Claude Code - Build AI-powered CLI tools with Claude.
- **Status**: Very active - v0.16.0 (Mar 2026), 37 versions released

### Amp SDK
- **Hex**: [hex.pm/packages/amp_sdk](https://hex.pm/packages/amp_sdk)
- **Downloads**: 24K all time
- **Description**: Elixir SDK for the Amp CLI - programmatic access to Amp's AI coding agent.
- **Status**: Active - v0.4.0

### Altar
- **Hex**: [hex.pm/packages/altar](https://hex.pm/packages/altar)
- **Downloads**: 62K all time
- **Description**: Robust, type-safe foundation for building AI agent tools in Elixir with clean contract for tool definitions.
- **Status**: Active - v0.2.0

### Altar AI
- **Hex**: [hex.pm/packages/altar_ai](https://hex.pm/packages/altar_ai)
- **Description**: Protocol-based AI adapter foundation. Provides unified abstractions for gemini_ex, claude_agent_sdk, codex_sdk with automatic fallback.
- **Status**: Active - v0.1.0

### AI SDK Ex
- **Hex**: [hex.pm/packages/ai_sdk_ex](https://hex.pm/packages/ai_sdk_ex)
- **Downloads**: 24K all time
- **Description**: Minimal Elixir AI SDK scaffolding for streaming text with tool calls.

### Agent Client Protocol (ACP)
- **Hex**: [hex.pm/packages/agent_client_protocol](https://hex.pm/packages/agent_client_protocol)
- **Description**: Elixir implementation of Agent Client Protocol for communication between code editors and AI coding agents.
- **Status**: v0.1.0

### Gemini CLI SDK
- **GitHub**: [nshkrdotcom/gemini_cli_sdk](https://github.com/nshkrdotcom/gemini_cli_sdk)
- **Description**: Elixir SDK for Gemini CLI - streaming, structured output, session management, OTP supervision tree integration.
- **Status**: Active - created Feb 2026

---

## AI Agent Frameworks

### Sagents (Sage Agents)
- **Description**: Combining wisdom of a Sage with LLM-based agents. OTP supervision, middleware, human-in-the-loop, sub-agent delegation, LiveView integration.
- **Built on**: LangChain

### Alloy
- **Description**: Minimal OTP-native agent loop. Completion-tool-call loop. Swap providers with one line. Run agents as supervised GenServers.
- **Philosophy**: No opinions on sessions, persistence, memory, scheduling, UI.

### Jido
- **Description**: Agent runtime with Sprites sandboxes for isolated execution.

---

## MCP (Model Context Protocol) Servers

### Tidewave
- **Hex**: [hex.pm/packages/tidewave](https://hex.pm/packages/tidewave)
- **Description**: Speeds up development with AI assistant that understands your web application. Current release connects editor's assistant to your Elixir project via MCP.
- **Features**: Ecto tools, Phoenix tools, logs, git, source code analysis

### Codicil
- **Description**: Semantic code search and analysis via MCP. Ask questions in natural language, find functions by behavior, trace dependencies.
- **Features**: Find similar functions, get function source, list callees, semantic search

### Agent Session Manager
- **Description**: MCP integration for managing AI agent sessions.

---

## AI-Friendly Testing Tools

### ExUnit JSON
- **Hex**: [hex.pm/packages/ex_unit_json](https://hex.pm/packages/ex_unit_json)
- **Description**: AI-friendly JSON test output for ExUnit.
- **Features**:
  - Structured JSON output for Claude Code, Cursor, etc.
  - Deterministic test ordering
  - Coverage with `--cover` and threshold gating
  - AI-optimized defaults (shows failures only)

### Hammox
- **Hex**: [hex.pm/packages/hammox](https://hex.pm/packages/hammox)
- **Downloads**: 4.1M all time
- **Description**: Automated contract testing for functions and mocks.
- **Status**: Active - v0.7.1 (Jul 2025)

---

## Awesome Lists

### Awesome ML & GenAI in Elixir
- **GitHub**: [georgeguimaraes/awesome-ml-gen-ai-elixir](https://github.com/georgeguimaraes/awesome-ml-gen-ai-elixir)
- **Stars**: 249 | **Forks**: 12
- **Description**: Curated list of Machine Learning and GenAI packages and resources for Elixir.
- **Status**: Very active - v7 (Feb 2026)

---

## Key Developers & Organizations

| Organization | Notable Projects |
|--------------|------------------|
| nshkrdotcom | Claude Agent SDK, Agent Session Manager, Gemini CLI SDK, ElixirScope, Tidewave |
| sagents-ai | Sagents, Sagents Live Debugger |
| E-xyza | Codicil |
| alloy-ex | Alloy |
| agentjido | JidoCode |
| thatsme | AlexClaw |

---

## Research Notes

### Community Insights from ElixirForum
- "Most agent projects are optimized for one-shot tasks: run a prompt, call a few tools, return a result, exit."
- "NexAgent is aimed at a different problem: keep an agent online, put it inside chat apps, give it persistent sessions and memory, let it run autonomously for weeks."
- Cursor with Sonnet 3.5 + agent feature in composer is popular for Elixir development
- GitHub Copilot has made great suggestions for Elixir syntax

### Security Considerations
- Secure AI Coding in Elixir talk at Elixir Montreal Sept 2025 (YouTube)

---

## External Links (All ≤ 3 Years Old)

| Link | Date | Notes |
|------|------|-------|
| [Reddit: Best AI code assistants for Elixir](https://www.reddit.com/r/elixir/comments/1ihsih0/best_ai_code_assistants_for_elixir/) | ~1 year ago | Community discussion |
| [ElixirForum: NexAgent self-evolving AI agent](https://www.reddit.com/r/elixir/comments/1rpz7s4/nexagent_a_selfevolving_ai_agent_built_on/) | ~1 week ago | Mar 2026 |
| [ElixirForum: Multi-Agent Orchestration](https://www.reddit.com/r/elixir/comments/1rv1scb/more_fun_with_multiagent_orchestration/) | ~3 days ago | Mar 2026 |
| [Thinking Elixir Podcast: Sage Advice for AI Agents](https://www.youtube.com/watch?v=H1iyBresVkM) | Feb 2026 | Very recent |
| [YouTube: We Vibe Coded with AI in 2 Hours](https://www.youtube.com/watch?v=ojaoe3h8hXA) | Jun 2025 | ~9 months old |
| [ElixirForum: X-Trace announcement](https://elixirforum.com/t/x-trace-a-new-tool-for-easy-to-use-recon-trace/55120) | Apr 2023 | ~3 years old - borderline |
| [Secure AI Coding in Elixir - Elixir Montréal](https://www.youtube.com/watch?v=biSJm-S08Qw) | Sep 2025 | ~6 months old |

---

*Researched via Playwright MCP web searches on GitHub, Hex.pm, and ElixirForum.*
