# Ash Framework

# 1. Big Picture: Ash + Phoenix (MVC-ish view)

```mermaid
flowchart TD
    Browser -->|HTTP Request| PhoenixRouter
  
    PhoenixRouter --> ControllerOrLiveView
  
    ControllerOrLiveView -->|calls| AshDomain
  
    AshDomain -->|exposes| AshResource
  
    AshResource -->|maps to| Database[(PostgreSQL)]
  
    Database --> AshResource
    AshResource --> AshDomain
    AshDomain --> ControllerOrLiveView
    ControllerOrLiveView -->|HTML / JSON| Browser
```

# 2. Domain → Resource relationship (your case)

```mermaid
flowchart TB
    AppConfig["config/config.exs<br/>ash_domains"]

    AppConfig --> AccountsDomain[Toko.Accounts]
    AppConfig --> StoreDomain[Store.Domain]
    AppConfig --> CatalogDomain[Store.Domains.Catalog]

    CatalogDomain --> ProductResource[Store.Products.Product]
    CatalogDomain --> CategoryResource[Store.Categories.Category]

```

# 3. What happens when you add a Domain but forget config

```mermaid
flowchart TB
    CatalogDomain[Store.Domains.Catalog<br/>use Ash.Domain]
    Config["config/config.exs<br/>ash_domains"]

    CatalogDomain -. NOT LISTED .-> Config

    CatalogDomain --> Warning[
        ⚠️ Warning
        Domain not present in ash_domains
    ]

```

# 4. Where to change what (visual cheat sheet)

```mermaid
flowchart TB
    Change{What did you change?}

    Change -->|Field / Rule| Resource
    Change -->|Grouping / Boundary| Domain
    Change -->|DB structure| Migration
    Change -->|HTTP / UI| Phoenix

    Resource --> ResourceFile["lib/store/resources/*.ex"]
    Domain --> DomainFile["lib/store/domains/*.ex"]
    Migration --> MigrationFile["priv/repo/migrations/*.exs"]
    Phoenix --> PhoenixFile["lib/toko_web/*"]

```

# 5. Typical beginner workflow (step-by-step)

```mermaid
flowchart TD
    Idea[Feature Idea]

    Idea --> Resource["Ash Resource attributes + actions"]
    Resource --> Migration["Generate Migration"]
    Migration --> Repo["Postgres Updated"]
    Resource --> Domain["Add to Domain"]
    Domain --> Config["Add Domain to config"]
    Config --> Phoenix["Call from LiveView / Controller"]
    Phoenix --> UI["Render HTML / JSON"]

```

# 6. Why the warning exists (safety rail)

```mermaid
flowchart TB
    Domain[New Ash.Domain]
    Config[ash_domains list]

    Domain --> Check{Is domain in config?}

    Check -->|Yes| OK[✅ App boots cleanly]
    Check -->|No| Warn[⚠️ Compile-time warning]

```
