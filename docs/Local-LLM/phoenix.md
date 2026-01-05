# Garis Miring Speckit:

## Always Include:

```json

{
  "servers": {
    "tidewave-local": {
      "command": "/home/jnkk/mcp/mcp-proxy",
      "args": ["http://localhost:4000/tidewave/mcp"]
    }
  }
}

```

```markdown


(claude)
ash_dashboard
clarity
ex_check
```

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

## /speckit.specify Example:
```markdown
/speckit.specify Develop Garis Miring, website for gig workers in Indonesia. Use spec-driven design patern. Indonesian as the default website language (English is supported later). Make the "building blocks" 'customizable'.
The typical gig workers in Indonesia are (not everything, will add more later) handyman, repairman, cleaners, and labour workers. Each gig worker are able to create a personal store or a cooperative store (or organization store), where other members can also join (by invitation). Workers' information (details, loocation and others) is available to those are registered to the website.
The app will make money by using Google Adsense or using Meta's Ad services. 

For location: detail lokasi tempat tinggal seseorang, yang mencakup nama jalan, nomor rumah, RT/RW, kelurahan, kecamatan, kota/kabupaten, provinsi, dan kode pos
```

## /speckit.plan Example:
```markdown
/speckit.plan Monolith Elixir version 1.19.4 repo. We are already inside an Ash and Phoenix Framework so we are using the Elixir programming language. Use the default Postgres from Phoenix Framework as the database, also add electric-sql (https://github.com/electric-sql/electric) and phoenix_sync (https://github.com/electric-sql/phoenix_sync), both can be install via `mix`. Always use the LiveView whenever possible, with the help of `ash_typescript` library for the frontend logic. 
ALWAYS USE Tidewave's tools for evaluating code `project_eval`, querying the database, etc.

Use `get_docs` to access documentation and the `get_source_location` tool to
find module/function definitions.
```
