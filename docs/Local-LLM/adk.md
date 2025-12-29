# Google Agent Dev Kit (ADK)

```
/speckit.constitution <Command>
/speckit.specify <Command>
/speckit.clarify
/speckit.plan <Command>
/speckit.tasks
/speckit.analyze
/speckit.checklist
/speckit.implement
```

## Constitution

```markdown
/speckit.constitution Update the constitution. Create principles focused on code quality, testing standards, user experience consistency, and performance requirements. Include governance for how these principles should guide technical decisions and implementation choices.
```

## Specify

```markdown
/speckit.specify Develop an Agent Dev Kit. A tool for building AI agents using local LLM. By leveraging Ollama for local LLMs and `ollama_chat` in `google-adk` python library. Using `gemma3:270m`, for chat and orchestration and `functiongemma`, for function calling capabilities.


Develop Garis Miring, website for helping humans in the world of AI. Users can post their services (example cleaning, repairs), make an admin page to add, remove or update existing services. Make a category for availabe services, for home, apartements or offices. This is dual language website for now, Indonesian as the default and has an English site. The code implementation is in English
```

## Plan

```markdown
/speckit.plan Using best practices of the python language. Whenever possible, use function programming practices for python language and not the default classes or OOP. Which one is the fastest causing less bugs, so we don't have to rewrite many things. Use the `uv` terminal tool if pip packages are needed.

We are already inside an Ash and Phoenix Framework root project folder so we are using the Elixir programming language. Use the default Postgres from Phoenix Framework as the database. The frontend should use liveview with tailwindcss, use liveview whenever possible for real-time updates. Use the tidewave MCP for further information about the server logs (whenever the compile success. Cannot run the server if the project cannot compile). Search for usage rules `deps/<library>/usage-rules.md` files for better coding and debugging.
```
