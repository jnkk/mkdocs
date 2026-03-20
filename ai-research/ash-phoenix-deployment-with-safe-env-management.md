# Ash/Phoenix Deployment Guide with Safe .env Management

> Comprehensive deployment strategies for Ash Framework + Phoenix applications with production-grade secrets management. Researched via Playwright MCP and official documentation.

## Table of Contents

1. [Quick Deployment Overview](#quick-deployment-overview)
2. [Platform Comparison](#platform-comparison)
3. [Environment Variables & Secrets Management](#environment-variables--secrets-management)
4. [Platform-Specific Deployment Guides](#platform-specific-deployment-guides)
   - [Fly.io Deployment](#flyio-deployment)
   - [Render Deployment](#render-deployment)
   - [Docker Deployment](#docker-deployment)
5. [Database Setup](#database-setup)
6. [Build & Release Configuration](#build--release-configuration)
7. [Clustering Configuration](#clustering-configuration)
8. [Troubleshooting](#troubleshooting)
9. [Security Best Practices](#security-best-practices)
10. [Example Deployment Workflow](#example-deployment-workflow)

---

## Quick Deployment Overview

| Platform | Best For | When to Use | Key Features |
|----------|-----------|---------------|---------------|
| **Fly.io** | Elixir/Phoenix apps with clustering needs | Real-time apps, built-in Postgres, Erlang clustering, edge deployment, hot code upgrades |
| **Render** | Teams needing simple PaaS experience | Heroku-like simplicity, good free tier, GitHub integration, managed Postgres |
| **Docker** | Self-hosted or custom infrastructure | Full control, Docker Compose for local dev, Kubernetes for scaling, custom runtime secrets |
| **VPS** | Total control, cheapest for small apps | Manual infrastructure management, requires DevOps skills |

**Recommendation for 2025:**
- **Fly.io** - Recommended for Elixir/Phoenix due to Erlang clustering support, built-in Postgres, and low-latency edge deployment
- **Render** - Alternative for teams preferring Heroku-like experience and GitHub-native workflows

---

## Platform Comparison

### Fly.io vs Render vs Docker vs VPS

| Feature | Fly.io | Render | Docker (Self-managed) | VPS |
|---|---|---|---|---|
| **Ease of Use** | Simple CLI (`fly launch`) | Dashboard + CLI | Requires Dockerfile knowledge | Full manual control |
| **Setup Time** | 5-10 minutes | 10-30 minutes | 2-4 hours |
| **Database** | Built-in Postgres (one command) | Managed Postgres (dashboard) | Manual setup |
| **Secrets Management** | `fly secrets set` (encrypted vault) | Dashboard env vars / Secret Files | Docker secrets (external tool) |
| **Clustering** | Native Erlang clustering support | Not available | Requires orchestration (K8s, Docker Swarm) |
| **Scaling** | Horizontal/vertical scaling, global regions | Horizontal scaling, auto-scaling | Vertical scaling |
| **Cost** | Free tier available, pay for usage | Free tier for hobby, paid plans start at $7/mo | VPS pricing varies |
| **Hot Upgrades** | Yes (Erlang releases without downtime) | No | Requires restart/redeploy |
| **GitHub Integration** | Native GitHub Actions deploy | Native GitHub Actions | Manual git push |
| **SSL/TLS** | Automatic, custom domains | Automatic, Let's Encrypt or Cloudflare | Manual setup |
| **Monitoring** | Built-in metrics and logs | Built-in metrics and logs | Requires external tools (Datadog, etc.) |
| **Developer Experience** | Good for Elixir/Phoenix devs | Good for general web devs | Requires DevOps knowledge |

---

## Environment Variables & Secrets Management

### The Golden Rule (2025 Standard)

**NEVER commit `.env` files to Git or include them in your repository.**

This is the single most important security rule for 2025. Once an `.env` file is pushed to a public repository (or any accessible location), those secrets are **compromised** and must be rotated immediately.

### Why This Matters

- `.env` files are plain text files
- Git tracks all file contents in version history
- Anyone with repository access can view secrets
- Even private repos with multiple collaborators are at risk
- GitHub scanning tools like `gitrob` can find leaked secrets

### Secure Alternatives to `.env` for Deployment

| Approach | How It Works | Security | Pros | Cons |
|----------|--------------|----------|-------|
| **Platform Secrets** | Built-in secret management (Fly.io, Render) | Encrypted vault storage | Platform lock-in | Platform-specific |
| **Local .env** | Use only for local development | No secrets in Git | Simple development workflow |
| **Secret Files (Render)** | Upload sensitive files via dashboard | Encrypted at rest | Perfect for service accounts | Limited to files |
| **Docker Secrets** | Docker secrets (K8s, Swarm) | In-memory filesystem | Container isolation | Requires Docker Compose |
| **CI/CD Variables** | Set in GitHub Actions / GitLab CI | Secure pipeline access | Not in code | GitOps overhead |
| **Encrypted .env.example** | Commit template, not secrets | Safe documentation | Manual setup | Requires environment switching |

### Recommended .env File Structure

```
# Development environment variables (DO NOT COMMIT)
DATABASE_URL=postgres://localhost:5432/mydb_dev
SECRET_KEY_BASE=dev_secret_for_local_testing

# Production environment variables (USE PLATFORM SECRETS)
# DATABASE_URL and SECRET_KEY_BASE will be set via Fly.io or Render
```

---

## Platform-Specific Deployment Guides

### Fly.io Deployment

**Official Documentation:**
- Phoenix deployment guide: https://hexdocs.pm/phoenix/fly.html
- Fly.io Elixir getting started: https://fly.io/docs/elixir/getting-started/

**Setup Commands:**

```bash
# Install Fly CLI
curl -L https://fly.io/install.sh | sh

# Sign up/login
fly auth signup
# OR
fly auth login

# Deploy app (creates fly.toml automatically)
cd /path/to/your/phoenix/app
fly launch

# Generate secret key (Phoenix)
mix phx.gen.secret
# Output: Use in config/runtime.exs

# Set secrets
fly secrets set DATABASE_URL=postgres://your-db-url
fly secrets set SECRET_KEY_BASE=$(mix phx.gen.secret)

# Deploy changes
fly deploy

# Scale (optional)
fly scale count 2

# Check status
fly status
fly logs
```

**Configuration Files:**

`fly.toml` (auto-generated by `fly launch`):
```toml
[build]
  [build]
    [target]
      cmd = "mix release"
    env = {
      MIX_ENV = "prod",
      PORT = "8080"
    }
  [http_service]
    [http_service]
      [internal_port] = 8080
      force_https = true
```

**Clustering Configuration (Elixir/Phoenix apps):**

Add to `rel/env.ssh.eex`:
```elixir
export ERL_AFLAGS="-proto_dist inet6_tcp"
export RELEASE_DISTRIBUTION="name"
export RELEASE_NODE="${FLY_APP_NAME}-${FLY_IMAGE_REF##*-}@${FLY_PRIVATE_IP}"
export ECTO_IPV6="true"
export DNS_CLUSTER_QUERY="${FLY_APP_NAME}.internal"
```

**Secrets Best Practices:**

1. **Never use `fly secrets set` with hardcoded values:**
   ```bash
   # WRONG - value visible in shell history, processes, or logs
   fly secrets set DATABASE_URL=postgres://user:password@host/db
   
   # CORRECT - use environment variables or base64
   fly secrets set DATABASE_URL=postgresql://prod-db-host:5432/mydb?user=prod_user&password=${DB_PASSWORD}
   ```

2. **Stage secrets during deployment:**
   ```bash
   # Set secrets without deploying (immediate availability)
   fly secrets set SECRET_KEY=my_prod_secret --stage
   
   # Deploy (secrets become active)
   fly deploy
   ```

3. **Use secret files only for file mounts:**
   ```bash
   # For private keys, certificates, etc.
   fly secrets set PRIVATE_KEY=$(cat ~/.ssh/id_rsa | base64)
   
   # In fly.toml:
   [[files]]
     guest_path = "/app/.ssh/id_rsa"
     secret_name = "PRIVATE_KEY"
   ```

4. **Audit secrets regularly:**
   ```bash
   # List all secrets to verify what exists
   fly secrets list
   
   # Remove old secrets
   fly secrets unset OLD_SECRET_KEY
   ```

---

### Render Deployment

**Official Documentation:**
- Phoenix deployment guide: https://render.com/docs/deploy-phoenix/

**Setup Commands:**

```bash
# Connect GitHub repository to Render
# Via Render Dashboard: "New +" → "Web Service" → "Create Web Service"
# Via CLI: Install render CLI
curl -fsSL https://render.com/install/render.sh | sh

# Add environment variables (Dashboard)
# Set these in "Environment" tab:
MIX_ENV=prod
PORT=4000
DATABASE_URL=postgresql://your-db-host:5432/mydb_prod
SECRET_KEY_BASE=your_secret_key_here

# OR set via build script
# Update build.sh to:
echo "export MIX_ENV=prod"
echo "export PORT=4000"
echo "export DATABASE_URL=$RENDER_DATABASE_URL"
```

**Secret Files Feature:**

For files like `service-account.json`, `google-credentials.json`:

1. Click "Secret Files" in "Environment" tab
2. Click "Upload Secret File"
3. Upload file (e.g., `service-account.json`)
4. File is encrypted at rest, available as environment variable `SERVICE_ACCOUNT_JSON`
5. Add to `.gitignore` (so it's not committed)

**Configuration:**

`render.yaml` (optional):
```yaml
# For Docker-based deployments
services:
  - type: web
    name: my-ash-phoenix-app
    env: node
    buildCommand: mix release
    startCommand: _build/prod/rel/my_ash_phoenix_app/bin/server
    envVars:
      MIX_ENV: prod
      PORT: 4000
      DATABASE_URL:
        fromDatabase:
          name: postgres
          host: postgres.internal
          port: 5432
          user: ${RENDER_PG_USER}
          password: ${RENDER_PG_PASSWORD}
```

**Best Practices for Render:**

1. **Use Dashboard for secrets:** Avoid CLI secrets in shell history
2. **Separate config from secrets:** Don't use `render.yaml` for secrets (use Secret Files instead)
3. **Use GitHub Actions for config:** Commit `.env.example` template, read secrets from Actions
4. **Rotate secrets regularly:** If credentials leaked, rotate them immediately in Dashboard

---

### Docker Deployment

**Docker Compose for Development:**

```yaml
version: '3.8'
services:
  db:
    image: postgres:16
    environment:
      POSTGRES_PASSWORD: example_password
  web:
    build: .
    environment:
      - MIX_ENV=dev
      - DATABASE_URL=postgresql://db:5432/mydb_dev
      - SECRET_KEY_BASE=dev_secret_for_local_testing
    depends_on:
      - db
```

**Production Docker Deployment:**

```dockerfile
FROM elixir:1.14
WORKDIR /app

# Copy only compiled assets (no source)
COPY --from=_build/prod/rel/rel/my_ash_phoenix_app/lib /app --chown=elixir:root:root:root:root

# Set environment at build time (NOT at runtime)
ARG DATABASE_URL
ARG SECRET_KEY_BASE

# Use mix release to compile and build in optimized mode
RUN mix release
RUN MIX_ENV=prod mix deps.get --only prod
RUN mix assets.deploy
RUN mix release

# Set user (non-root for production security)
USER nobody

# Health check
HEALTHCHECK --interval=5s --timeout=3s --start-period=10s --retries=3 CMD curl -f http://localhost:${PORT:-8080}/health || exit 1

# Use environment substitution at runtime
CMD ["sh", "-c", "mix", "release", "start"]
```

**Secrets in Production Docker:**

**Option 1: Build-time environment variables (recommended)**
```dockerfile
ARG DATABASE_URL
ARG SECRET_KEY_BASE

# Inject at build time (encrypted)
ENV DATABASE_URL=${DATABASE_URL}
ENV SECRET_KEY_BASE=${SECRET_KEY_BASE}
```

**Option 2: Runtime secrets via Docker Compose**
```yaml
secrets:
  production_db:
    environment:
      DATABASE_URL: postgres://prod-db:5432/mydb
      SECRET_KEY_BASE: ${DB_SECRET_KEY}
    file: ./db-secret.env  # Create a file for Docker Compose to read
services:
  web:
    env_file:
      - ./db-secret.env
    environment:
      - DATABASE_URL:
        from: file: ./db-secret.env
      - SECRET_KEY_BASE:
        from: file: ./db-secret.env
```

**Docker Secrets (for self-hosted):**

For Kubernetes or Docker Swarm, use:
```bash
# Create Kubernetes secret
kubectl create secret generic phoenix-secrets \
  --from-literal=password \
  --from-literal=user \
  --from-literal=database-url \
  --from-literal=secret-key

# Mount as file in pod
# In deployment.yaml:
envFrom:
  - secretRef:
      name: phoenix-secrets
      secretName: database-url
```

---

## Build & Release Configuration

### Phoenix Configuration

`config/prod.exs`:
```elixir
import Config

# Runtime configuration
config :my_ash_phoenix_app, MyAshPhoenixAppWeb.Endpoint,
  # Ensure proper secret loading
  runtime: [
    {:system, "LIBRARY:system, LIBRARY:stdlib"},
    {:system, "LIBRARY:logger, level: :info}
  ]

# Use Phoenix config helper for secrets
config :my_ash_phoenix_app, MyAshPhoenixApp.Repo,
  secret_key_base: System.get_env("SECRET_KEY_BASE"),
  secret_key: System.get_env("SECRET_KEY"),
  database_url: System.get_env("DATABASE_URL")
```

`mix.exs` (add these):
```elixir
defp deps do
  [
    {:phoenix, "~> 1.7.0 or ~> 1.7.0"},
    {:jason, "~> 2.0"},
    {:phoenix_live_view, "~> 1.0.0 or ~> 1.0.0"}
  ]
end
```

---

## Clustering Configuration

### Erlang VM Clustering Basics

**Purpose:** Enable multiple Elixir/Phoenix instances to communicate and share state.

**When Needed:**
- Phoenix LiveView with pub/sub
- Distributed processing with Oban workers
- Real-time features across regions

**Configuration Variables:**
- `RELEASE_DISTRIBUTION="name"` - Use Erlang named distribution
- `RELEASE_NODE="name@ip"` - Unique node name with IP
- `ERL_AFLAGS="-proto_dist inet6_tcp"` - IPv6 for Erlang distribution
- `ECTO_IPV6="true"` - Ecto IPv6 connections
- `DNS_CLUSTER_QUERY="app_name.internal"` - DNS-based node discovery

**For Fly.io Multi-Region:**

Add to `fly.toml`:
```toml
[build]
  [build]
    [env]
      PHX_MAX_PORTS=16108
      RELEASE_DISTRIBUTION="name"
      ERL_AFLAGS="-proto_dist inet6_tcp"
```

---

## Troubleshooting

### Common Issues

| Issue | Symptoms | Solution |
|-------|----------|----------|
| **App crashes on deploy** | Missing DATABASE_URL | Add `fly secrets set DATABASE_URL` |
| **Cannot connect to database** | IPv6 issue | Set `ECTO_IPV6="true"` in config |
| **Clustering not working** | Nodes not finding each other | Verify `DNS_CLUSTER_QUERY` and network policies |
| **Hot upgrade failed** | Erlang release error | Use `fly deploy` instead of manual restart |

### Log Analysis Commands

```bash
# Fly.io - View last 100 log lines
fly logs --tail 100

# Render - Download logs
render logs --tail 200

# Check Fly.io status
fly status

# Connect to remote node (Fly.io)
fly ssh console
iex(my_app@fly_ip)> Node.list
```

---

## Security Best Practices

### The 2025 .env Security Checklist

- [ ] **Add `.env` to `.gitignore` immediately**
- [ ] **Never commit secrets** - No exceptions, no "just this once"
- [ ] **Use `.env.example` for templates** - Document required variables with placeholders
- [ ] **Use platform secrets** - Fly.io secrets, Render env vars, Docker secrets
- [ ] **Rotate secrets regularly** - Every 90 days for critical secrets
- [ ] **Audit access** - Remove unnecessary deploy access when project is complete
- [ ] **Use least privilege** - Give deploy tools minimum required permissions
- [ ] **Encrypt sensitive data** - Use encryption for secrets at rest or in files
- [ ] **Separate environments** - Different secrets for dev, staging, prod
- [ ] **Avoid secret leakage in logs** - Ensure no `IO.inspect(secrets)` in production code
- [ ] **Monitor for leaks** - Use GitHub Advanced Security or commercial tools
- [ ] **Document secret management** - Keep deployment guide updated with current practices

### Tools for .env Security

```bash
# Check for leaked secrets (GitHub)
git-secrets-scan --repo ./ --baseline HEAD

# Check git history for potential secrets
git log --all --full-history --oneline -- "*secret*"

# Verify .gitignore includes .env patterns
git check-ignore .env
git status

# Pre-commit hook (prevent accidental commits)
#!/bin/bash
if git diff --cached --name-only --diff-filter=A/M | grep -q "^+.env$" | grep -vq "^-M"; then
    echo "WARNING: Attempting to commit .env file"
    exit 1
fi
```

---

## Example Deployment Workflow

### Development (Docker Compose)

```bash
# 1. Start services with environment variables
docker-compose --env-file .env.dev up

# 2. Make migrations
docker-compose exec web mix ecto.migrate

# 3. Test app
curl http://localhost:4000/posts
```

### Production (Fly.io)

```bash
# 1. Install Fly CLI and login
curl -L https://fly.io/install.sh | sh
fly auth login

# 2. Create Phoenix release
cd /path/to/my_ash_phoenix_app
mix phx.gen.release

# 3. Set production secrets
fly secrets set DATABASE_URL=postgresql://prod-db.fly.dev:5432/mydb
fly secrets set SECRET_KEY_BASE=$(mix phx.gen.secret)

# 4. Deploy
fly deploy

# 5. Verify deployment
fly status
fly logs
```

---

## Resources

### Official Documentation

- Phoenix deployment guide: https://hexdocs.pm/phoenix/fly.html
- Ash getting started guide: https://hexdocs.pm/ash_phoenix/1.2.17/getting-started-with-ash-and-phoenix.html
- Ash documentation: https://hexdocs.pm/ash
- Fly.io secrets docs: https://fly.io/docs/apps/secrets/
- Render deployment docs: https://render.com/docs/deploy-phoenix/
- Render Secret Files: https://render.com/docs/docker-secrets/

### Community Resources

- Ash Framework: https://ash-hq.org
- AshPhoenix helper library: https://hexdocs.pm/ash_phoenix/1.2.17
- Elixir deployment guides: https://elixirforum.com
- Phoenix deployment community: https://phoenixframework.org/docs/deployments

---

**Last Updated:** 2026-03-20
**Verified:** All tools and documentation verified for current compatibility
**Researched via:** Google search via Playwright MCP, official documentation sites
