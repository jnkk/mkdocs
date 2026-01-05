# Garis Miring Speckit:

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

## /speckit.constitution Example:

```markdown
/speckit.constitution Create principles focused on code quality, testing standards, user experience consistency, and performance requirements. Include governance for how these principles should guide technical decisions and implementation choices. The AGENTS.md file is the source of truth. Always use Tidewave's tools for evaluating code, querying the database, etc.

Use `get_docs` to access documentation and the `get_source_location` tool to
find module/function definitions.
```

/speckit.constitution Update the constitution. Create principles focused on code quality, testing standards, user experience consistency, and performance requirements. Include governance for how these principles should guide technical decisions and implementation choices. The AGENTS.md file is the source of truth.


## /speckit.specify Example:
```markdown
/speckit.specify Develop Garis Miring, website for helping humans in the world of AI. Users can post their services (example cleaning, repairs), make an admin page to add, remove or update existing services. Make a category for availabe services, for home, apartements or offices. This is dual language website for now, Indonesian as the default and has an English site. The code implementation is in English.  

We are developing an AI agent tool using the Google-ADK. We are able to create tools for an AI Agents to help make using AI easier and simpler by using tools. If the tools are not created, we are to create it. And make sure agents are able to talk to each other agents, to help and for better debugging. 

We are developing an AI Agents. The AI Agents are able to create tools for the AI Agents to help make using AI easier and simpler by using tools. If there are tools that are not availabe, we are able to create it. Make sure that the agents are able to communicate with each other.

It should allow users to create custom PC builds, allow the admin (me, the project owner. Use the `usage rules` for further information) page (it does not have to be in the `/admin` route. It can be in `/web_admin`) to add and remove computer components and their compatibilities with ease with a yaml file (like a seed.ex), also allow the admin make a build and build templetes so other users can later buy (using affiliate links. Provide later in the project), allow users to comment on the builds that other users or the admin has built, allow users to orovide feedback and able to take surveys from the admin. 
```

## /speckit.plan Example:
```markdown
We are already inside an Ash and Phoenix Framework so we are using the Elixir programming language. Use the default Postgres from Phoenix Framework as the database. Always use Tidewave's tools for evaluating code, querying the database, etc.

Use `get_docs` to access documentation and the `get_source_location` tool to
find module/function definitions.


We are already inside a python folder, so use the python programming language. Inside this folder, `uv` and `google-adk `is already installed. There's a sample on how the code inside the `knowledge` folder. We are not using the default GOOGLE services to make this happen, we are using ollama.
```


You are developing an AI coding agent designed to assist with a wide range of development tasks, and tools from routine maintenance to complex, transformative work.
You excels at bug fixes, testing, refactoring, and implementing new features that require deep understanding of codebases. 


You are using the python programming language. Libraries used right now are as following: google-adk, fastmcp, r2r and qdrant (already install via `uv`), later are added if needed. LMStudio is also used a tool for the LLM server with small local models (models provided later). You are able to make and use agents to help with the tasks. These agents are able to orchestrate, do function calling using tools (like search documents, parsing data, collecting data, write code, fix bugs etc), delegate tasks, talk to each other to better them self, communicate with the maker, be able to use an MCP, make use of a small language models.

Update to the plan. Create an MCP server to manage and delegate those AI Agents. The MCP is able to create, use, modify, update tools to and from the AI Agents. To help better themselfs. The MCP and AI Agents are able to communicate with each other. Make the MCP server be able to talk to LLMs, like in LMStudio and vscode github copilot. We are focusing on local LLMs, using LMStudio.
Use qdrant for the database, logging either from MCP server or AI Agents and for the embeddings.
Use fastmcp python library for the creation of the MCP server(s). You are able to create more than 1 MCP server for using the tools.
Use the google-adk for creating, managing and orchestrating AI Agents.
