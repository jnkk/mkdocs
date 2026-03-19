---
title: Elixir Internal Debugging Tools
icon: material/bug
tags:
  - elixir
  - debugging
  - tooling
  - iex
  - observer
date_added: 2026-03-19
last_updated: 2026-03-19
---

# Elixir Internal Debugging Tools

Elixir ships with comprehensive debugging and introspection tooling built into the BEAM VM. No external AI needed — the platform itself provides everything for observing, profiling, and debugging running systems.

## Quick Reference

| Tool | Purpose | When to Use |
|------|---------|-------------|
| `dbg/2` | Quick inline debugging | Fast value inspection during development |
| `IO.inspect/2` | Pipeline debugging | See values at any point in a chain |
| `IEx.pry` | Interactive debugging | Need to inspect variables mid-execution |
| `IEx.break!/2` | Line breakpoints | Step through code without modifying it |
| `:observer` | System-wide GUI | Understand VM state, processes, memory |
| `mix profile.*` | Performance profiling | Find bottlenecks |

## Basic Debugging

### dbg/2 (Elixir v1.14+)

Modern replacement for `IO.inspect` with context:

```elixir
# Basic usage
dbg(some_value)

# In pipelines - shows EVERY step
__ENV__.file
|> String.split("/", trim: true)
|> List.last()
|> File.exists?()
|> dbg()
# Prints:
# [file.exs:5: (file)]
# __ENV__.file #=> "/path/file.exs"
# |> String.split("/", trim: true) #=> ["path", "file.exs"]
# |> List.last() #=> "file.exs"
# |> File.exists?() #=> true
```

### IO.inspect with binding

```elixir
def my_function(a, b, c) do
  IO.inspect(binding(), label: "variables")
  # ...
end
# Prints: variables: [a: 1, b: 2, c: 3]
```

## Pry Debugging

Interactive REPL inside running code:

```bash
# Start IEx with pry debugger
iex --dbg pry -S mix
```

```elixir
def my_function(x) do
  result = x |> process_data() |> validate()
  dbg(result)  # Opens pry session here
  result
end
```

**Pry commands:**
- `continue` or `c` — Resume execution
- `next` or `n` — Step to next line
- `restart` — Restart the current function

## Breakpoints

Set breakpoints without modifying code:

```elixir
# Set breakpoint on a function
IEx.break!(MyModule, :my_function, 2)

# List all breakpoints
IEx.breaks()

# Remove breakpoint
IEx.remove_break!(MyModule, :my_function, 2)
```

**Debug tests with breakpoints:**
```bash
# Break at start of every failed test
mix test --breakpoints --failed

# Break at specific line
mix test -b test/my_module_test.exs:42
```

## Observer (GUI)

The GUI tool for understanding the entire VM:

```elixir
# Basic start
iex> :observer.start()

# For mix projects (requires runtime_tools)
iex> Mix.ensure_application!(:wx)           # Not needed on OTP 27+
iex> Mix.ensure_application!(:runtime_tools) # Not needed on OTP 27+
iex> Mix.ensure_application!(:observer)
iex> :observer.start()
```

### Observer Tabs

| Tab | Shows |
|-----|-------|
| **System** | CPU, memory, runtime statistics |
| **Load Charts** | Live memory, scheduler utilization, I/O |
| **Memory Allocators** | VM memory carriers |
| **Applications** | Supervision trees, app hierarchies |
| **Processes** | All processes, reductions, mailbox sizes |
| **Ports** | Open file descriptors |
| **Sockets** | Network connections |
| **Table Viewer** | ETS tables contents |
| **Trace Overview** | Function call tracing |

### Remote Debugging with Observer

```bash
# Terminal 1: Start app with named node
iex --name myapp@127.0.0.1 --cookie secret -S mix

# Terminal 2: Start hidden observer node
iex --hidden --name observer@127.0.0.1 --cookie secret

# In observer node:
iex> Node.connect(:"myapp@127.0.0.1")
iex> :observer.start(:"myapp@127.0.0.1")
```

This isolates Observer from the target application — it won't affect measurements.

### Tracing in Observer

1. Go to **Processes** tab → right-click process → "Trace selected processes"
2. Select options:
   - Trace function calls
   - Trace arity instead of arguments
   - Trace garbage collections
3. Go to **Trace Overview** → click process → "Add Trace Pattern"
4. Select module and function to trace
5. Click **Start Trace**

**Garbage collection info available:**
- `bin_vheap_size` — Binary memory in process
- `heap_size` — Process heap usage
- `recent_size` — Memory after last GC
- `mbuf_size` — Message buffer size

## Profiling

### mix profile.cprof — Function Call Counting

```bash
mix profile.cprof -n MyModule.my_function 1
```

Counts how many times each function is called.

### mix profile.fprof — Time Profiling

```bash
mix profile.fprof -n MyModule.my_function 1
```

Detailed call graph with time spent in each function.

## Advanced Erlang Tools

### Terminal Observer (etop)

```bash
erl -name etop -hidden -s etop -s erlang halt \
    -output text \
    -node myapp@127.0.0.1 \
    -setcookie secret
```

Text-based process monitoring without GUI.

### Trace Tool Builder (ttb)

Comprehensive tracing with file output for later analysis.

### crashdump_viewer

View crash dumps:
```bash
crashdump_viewer my_crash.dump
```

### OS-Level Tracing

- **Linux Trace Toolkit (LTTng)**
- **DTRACE** (macOS/Solaris)
- **SystemTap** (Linux)

### Microstate Accounting

```elixir
:erlang.statistics(:microstate_accounting)
```

Low-overhead metrics on VM time distribution.

## Phoenix LiveDashboard

For web applications, provides Observer-like features in the browser:

```elixir
# In mix.exs
def deps do
  [
    {:phoenix_live_dashboard, "~> 0.8"}
  ]
end
```

Features:
- Request metrics
- ETS tables viewer
- Processes viewer
- Custom metrics

## IEx Runtime Info

Quick system overview without GUI:

```elixir
runtime_info()  # Mini Observer overview
```

## Key Commands Cheatsheet

```bash
# Start with debugging
iex --dbg pry -S mix

# Debug failed tests
mix test --breakpoints --failed

# Profile functions
mix profile.fprof -n Module.function 1

# Remote Observer
iex --name app@host --cookie x -S mix
iex --hidden --name obs@host --cookie x
Node.connect(:"app@host")
:observer.start(:"app@host")
```

## Further Reading

- [Elixir Debugging Guide](https://hexdocs.pm/elixir/debugging.html) — Official docs
- [Erlang in Anger](https://www.erlang-in-anger.com/) — Free ebook on VM debugging
- [AppSignal: Debugging with Observer](https://blog.appsignal.com/2025/11/04/debugging-in-elixir-with-observer.html) — Practical examples
- [ElixirForum: What are your debugging techniques?](https://elixirforum.com/t/what-are-your-debugging-techniques/693) — Community discussion
- [ElixirForum: Remote debugging on Docker](https://elixirforum.com/t/remote-debugging-elixir-on-docker-container/39382) — Container debugging

## Community Insights

Wisdom from experienced Elixir/Erlang developers on the [Elixir Forum](https://elixirforum.com):

### Debugging Concurrent Systems

> *"You have to be very careful when debugging things in a concurrent environment... Anything that is waiting for the debugged process will hang and in many cases may time out and crash."* — Robert Virding (Creator of Erlang)

**Key takeaways:**
- Stopping processes with breakpoints can cause cascading timeouts
- `IEx.pry` in functions called by many processes creates multiple sessions
- Sometimes `IO.inspect` is all that's reasonable
- Use tracing instead of traditional debugging for concurrent systems

### Why Experienced Devs Prefer Tracing

From experienced Erlang developers in the community:

1. **Hot code reloading** makes print debugging trivial — changes don't require restarts
2. **Tracing** lets you observe without disrupting the system
3. **The BEAM is different** — introducing breakpoints changes program behavior

### Erlang Debugger (:debugger)

For traditional step-through debugging:

```elixir
# Start the GUI debugger
:debugger.start()

# Interpret a module for debugging
:debugger.start()
:int.ni(MyModule)
```

Note: Requires setup to find Elixir source files. See [Plataformatec's debugging guide](http://blog.plataformatec.com.br/2016/04/debugging-techniques-in-elixir-lang/).

### Modern Tracing Tools

The BEAM provides several tracing options. Here are the current tools:

**1. Recon_TRACE (built-in)**
The most practical option — part of the `recon` library:

```elixir
# Add to mix.exs
{:recon, "~> 2.5"}

# Usage
:recon_trace.calls({Module, :function, :_}, 100)
```

**2. Extrace** (Hex)
Elixir wrapper for Recon Trace — clean API for setting trace points:

```elixir
# Add to mix.exs
{:extrace, "~> 0.3"}

# Trace specific functions
Extrace.calls([{Enum, :take_random, fn _ -> :return end}], 100)
```

Active project — last push Feb 2025, v0.3.0.

**3. X-Trace** (GitHub: feng19/x_trace)
Web UI for recon_trace — visual interface for tracing without CLI:
- Latest release v0.2.3 (Nov 2025)
- Download desktop app or run as web server
- Visit localhost:4000 for the GUI

**4. OpenTelemetry / Telemetry**
Standardized observability:

```elixir
# Telemetry is built into Elixir
:telemetry.execute([:my_app, :request], %{duration: 100}, %{path: "/api"})
```

**5. Observer Trace Overview**
Already covered above — use the GUI for tracing without code changes.

### Logging Libraries

**Og** — Tiny logging wrapper that mimics `IO.inspect` style:

```elixir
# Log with source location automatically
(1..10) |> Enum.map(&(&1 * &1)) |> Og.log_return()

# Explicit environment logging
result |> Og.log_return(__ENV__, :info)
```

From the community: *"I end up using `__ENV__` a lot"* for tracking where logs come from.

### LiveBook Debugging

For processes running in LiveBook:

```elixir
# Start Observer to see LiveBook processes
:observer.start()
```

LiveBook runs code in named processes — you can connect Observer to inspect them.

## Docker/Container Debugging

Remote debugging Elixir in Docker containers:

### Setup Requirements

```dockerfile
# Expose EPMD port (4369) and a range for distributed nodes
EXPOSE 4369 4369..4370
```

### Container to Host Debugging

```bash
# Container: Start your app with a named node
iex --name myapp@host.docker.internal --cookie mycookie -S mix

# Host: Start hidden observer node
iex --hidden --name observer@host.docker.internal --cookie mycookie

# Connect
iex> Node.connect(:"myapp@host.docker.internal")
iex> :observer.start(:"myapp@host.docker.internal")
```

### Common Issues

1. **EPMD port mapping** — Ensure port 4369 is properly exposed/mapped
2. **Hostname resolution** — Use `--name host.docker.internal` to reach host
3. **Cookie mismatch** — Both nodes must use the same cookie
4. **epmd conflicts** — Kill local epmd if running: `epmd -kill`

See: [ElixirForum: Remote debugging on Docker](https://elixirforum.com/t/remote-debugging-elixir-on-docker-container/39382)

## Debugging Anti-Patterns

| Anti-Pattern | Better Approach |
|-------------|----------------|
| Using breakpoints in GenServers | Use `dbg/2` or add temporary logging |
| Stopping many processes at once | Trace one process, understand, repeat |
| Debugging production directly | Use LiveDashboard or remote Observer |
| Modifying code to add debug prints | Use `dbg/2` or Observer tracing |
| Fighting the BEAM concurrency model | Work with processes, not against them |

---

*Researched via: Google search, hexdocs.pm official documentation, AppSignal blog, ElixirForum community*
