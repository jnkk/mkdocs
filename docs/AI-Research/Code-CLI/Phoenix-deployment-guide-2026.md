---
title: Phoenix Deployment Guide 2026
icon: material/rocket-launch
tags:
  - elixir
  - phoenix
  - deployment
  - devops
  - ci-cd
  - docker
  - kubernetes
date_added: 2026-03-21
last_updated: 2026-03-21
---

# Phoenix Deployment Guide 2026

Comprehensive guide to deploying Phoenix applications to production environments, covering modern strategies, cloud providers, CI/CD pipelines, and best practices.

**Researched via:**
- Official Phoenix documentation (hexdocs.pm)
- OneUptime production deployment guide
- Fly.io community discussions
- Gigalixir deployment guides
- AWS deployment examples
- Blue-green and canary deployment patterns
- CI/CD implementation guides

## Table of Contents

- [Overview](#overview)
- [Deployment Strategies](#deployment-strategies)
- [Mix Releases](#mix-releases)
- [Docker Deployment](#docker-deployment)
- [Cloud Providers](#cloud-providers)
  - [Fly.io](#flyio)
  - [Gigalixir](#gigalixir)
  - [Render](#render)
  - [Railway](#railway)
  - [Heroku](#heroku)
  - [AWS](#aws)
  - [Google Cloud](#google-cloud)
- [CI/CD Pipelines](#cicd-pipelines)
- [Monitoring & Observability](#monitoring-observability)
- [Deployment Patterns](#deployment-patterns)
- [Production Checklist](#production-checklist)

## Overview

Phoenix is a web framework for the Elixir programming language that gives you peace of mind from development to production. Deploying Phoenix applications involves understanding Elixir releases, proper configuration management, database migrations, and infrastructure setup.

Phoenix applications benefit from BEAM virtual machine'"'"'"s fault tolerance and hot code upgrades. Combined with OTP releases, you get a deployment model that is reliable, efficient, and straightforward once you understand fundamentals.

### Key Advantages

- **Fault tolerance**: BEAM VM provides exceptional reliability and self-healing capabilities
- **Hot code upgrades**: Zero-downtime deployments are possible with Erlang/OTP
- **Concurrency**: Built-in actor model and lightweight processes
- **Real-time**: Native support for WebSockets and Phoenix Channels
- **Scalability**: Distributed systems support with clustering

## Deployment Strategies

### Three Main Deployment Steps

When preparing an application for deployment, there are three main steps:

1. **Handling of your application secrets**
2. **Compiling your application assets**
3. **Starting your server in production**

### Basic Production Script

```bash
# Initial setup
mix deps.get --only prod
MIX_ENV=prod mix compile

# Compile assets
MIX_ENV=prod mix assets.deploy

# Custom tasks (like DB migrations)
MIX_ENV=prod mix ecto.migrate

# Finally run server
PORT=4001 MIX_ENV=prod mix phx.server
```

### Environment Variables

Required environment variables for production:

```bash
# Required
export SECRET_KEY_BASE="your-64-character-secret-key"
export DATABASE_URL="ecto://user:pass@host/database"
export PHX_HOST="your-domain.com"
export PORT="4000"

# Optional but recommended
export POOL_SIZE="20"
export ECTO_IPV6="false"

# For clustering (if using distributed Elixir)
export RELEASE_NODE="my_app@10.0.0.1"
export RELEASE_COOKIE="your-erlang-cookie"

# Application-specific
export SMTP_HOST="smtp.example.com"
export REDIS_URL="redis://localhost:6379"
```

**Important:** Generate SECRET_KEY_BASE using `mix phx.gen.secret`, not by copying values.

## Mix Releases

Mix releases bundle your application, Erlang runtime, and all dependencies into a self-contained directory. This eliminates the need to install Elixir or Erlang on your production servers.

### Building Releases

Phoenix 1.6 and later include release configuration by default.

**mix.exs configuration:**

```elixir
defmodule MyApp.MixProject do
  use Mix.Project

  def project do
    app: :my_app,
    version: "0.1.0",
    elixir: "~> 1.14",

    # Release configuration
    releases: [
      my_app: [
        include_executables_for: [:unix],
        applications: [runtime_tools: :permanent]
      ]
    ]
  end
end
```

**Building the release:**

```bash
# Set environment to production
export MIX_ENV=prod

# Get dependencies
mix deps.get --only prod

# Compile
MIX_ENV=prod mix compile

# Compile assets
MIX_ENV=prod mix assets.deploy

# Create release
mix phx.gen.release

# Build with release
MIX_ENV=prod mix release
```

The release is created in `_build/prod/rel/my_app/`. This directory contains everything needed to run your application.

### Running the Release

```bash
# Start your system
_build/prod/rel/my_app/bin/my_app start

# Start with Phoenix server
_build/prod/rel/my_app/bin/server

# Run migrations in release
_build/prod/rel/my_app/bin/my_app eval "MyApp.Release.migrate"
```

### Custom Release Module for Migrations

Create `lib/my_app/release.ex`:

```elixir
defmodule MyApp.Release do
  @moduledoc """
  Functions for running migrations and other release tasks.
  These are used via release commands: bin/my_app eval "MyApp.Release.migrate"
  """

  @app :my_app

  # Run all pending migrations
  def migrate do
    load_app()
    for repo <- repos() do
      {:ok, _, _} = Ecto.Migrator.with_repo(repo, &Ecto.Migrator.run(&1, :up, all: true))
    end
  end

  # Rollback to last migration
  def rollback(repo, version) do
    load_app()
    {:ok, _, _} = Ecto.Migrator.with_repo(repo, &Ecto.Migrator.run(&1, :down, to: version))
  end

  # Get current migration status
  def migration_status do
    load_app()
    for repo <- repos() do
      {:ok, status, _} = Ecto.Migrator.with_repo(repo, fn repo ->
        Ecto.Migrator.migrations(repo)
      end)
      IO.puts("Migrations for #{inspect(repo)}:")
      Enum.each(status, fn {status, version, name} ->
        IO.puts(" #{status} #{version} #{name}")
      end)
    end
  end

  # Seed database
  def seed do
    load_app()
    for repo <- repos() do
      {:ok, _, _} = Ecto.Migrator.with_repo(repo, fn _repo ->
        seed_path = priv_path_for(repo, "seeds.exs")
        if File.exists?(seed_path) do
          Code.eval_file(seed_path)
        end
      end)
    end
  end

  defp repos do
    Application.fetch_env!(@app, :ecto_repos)
  end

  defp load_app do
    Application.load(@app)
  end

  defp priv_path_for(repo, filename) do
    app = Keyword.get(repo.config(), :otp_app)
    priv_dir = "#{:code.priv_dir(app)}"
    Path.join([priv_dir, "repo", filename])
  end
end
```

### Clustering with Releases

Phoenix applications running on multiple nodes need clustering configuration. Phoenix supports two types of transports for its Socket implementation: WebSocket and Long-Polling.

**For Long-Polling to work properly in multi-node deployments, your application must either:**

1. Utilize Erlang VM'"'"'"s clustering capabilities (default Phoenix.PubSub adapter)
2. Choose a different Phoenix.PubSub adapter (such as Phoenix.PubSub.Redis)
3. Or your deployment option must implement sticky sessions

## Docker Deployment

Docker provides consistent deployments across environments. Phoenix releases work well with container technologies.

### Multi-Stage Dockerfile

A production-ready Dockerfile using multi-stage builds to minimize image size:

```dockerfile
# Dockerfile
# Build stage - compiles application
FROM hexpm/elixir:1.15.7-erlang-26.1.2-alpine-3.18.4 AS builder

# Install build dependencies
RUN apk add --no-cache build-base git npm

# Set working directory
WORKDIR /app

# Install hex and rebar
RUN mix local.hex --force && \
    mix local.rebar --force

# Set build environment
ENV MIX_ENV=prod

# Copy dependency files first for better caching
COPY mix.exs mix.lock ./
COPY config config/

# Install and compile dependencies
RUN mix deps.get --only $MIX_ENV
RUN mix deps.compile

# Copy compile-time config
COPY config/config.exs config/${MIX_ENV}.exs config/
RUN mix compile

# Copy application code
COPY priv priv COPY lib lib COPY assets

# Build assets
RUN mix assets.deploy

# Copy runtime configuration
COPY config/runtime.exs config/

# Create release
COPY rel rel
RUN mix release

# Runtime stage - minimal image for running application
FROM alpine:3.18.4 AS runner

# Install runtime dependencies
RUN apk add --no-cache libstdc++ openssl ncurses-libs

# Set working directory
WORKDIR /app

# Create non-root user for security
RUN addgroup -S myapp && adduser -S myapp -G myapp
USER myapp

# Copy release from builder stage
COPY --from=builder --chown=myapp:myapp /app/_build/prod/rel/my_app ./

# Set environment variables
ENV HOME=/app
ENV MIX_ENV=prod
ENV PHX_SERVER=true

# Expose application port
EXPOSE 4000

# Health check endpoint
HEALTHCHECK --interval=30s --timeout=5s --start-period=5s --retries=3 \
  CMD wget --no-verbose --tries=1 --spider http://localhost:4000/health || exit 1

# Start application
CMD ["bin/my_app", "start"]
```

### Docker Compose for Local Testing

```yaml
# docker-compose.yml
version: "3.8"

services:
  app:
    build:
      context: .
      dockerfile: Dockerfile
    ports:
      - "4000:4000"
    environment:
      DATABASE_URL: ecto://postgres:postgres@db/my_app_prod
      SECRET_KEY_BASE: ${SECRET_KEY_BASE}
      PHX_HOST: localhost
      PORT: 4000
    depends_on:
      db:
        condition: service_healthy
    restart: unless-stopped

  db:
    image: postgres:15-alpine
    environment:
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: postgres
      POSTGRES_DB: my_app_prod
    volumes:
      - postgres_data:/var/lib/postgresql/data
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U postgres"]
      interval: 5s
      timeout: 5s
      retries: 5

volumes:
  postgres_data:
```

### Running with Docker

```bash
# Generate secret key base
export SECRET_KEY_BASE=$(mix phx.gen.secret)

# Build image
docker-compose build

# Start services
docker-compose up -d

# View logs
docker-compose logs -f app
```

## Cloud Providers

### Fly.io

Fly.io is a PaaS that deploys your servers close to your users with built-in distribution support.

**Key Features:**
- Built-in DNS clustering support
- Private networking with DNS queries
- Automatic scaling
- Global deployment

**Getting Started:**

```bash
# Install Fly CLI
curl -L https://fly.io/install.sh | sh

# Launch app
fly launch
```

Fly automatically:
- Creates a `fly.toml` configuration file
- Attaches a PostgreSQL database
- Sets up SSL certificates
- Configures DNS and networking

**Important Notes:**
- Fly provides private networks with DNS service discovery out of the box
- LiveView sockets may fall over to long-poll immediately on Fly (community forum discussion)
- Use standard PostgreSQL configuration for best performance

### Gigalixir

Elixir-specific Platform as a Service with automatic deployment pipelines.

**Key Features:**
- Elixir-optimized infrastructure
- Buildpack system for auto-detection
- Standard and Free tier databases
- ASDF version management support

**Getting Started:**

```bash
# Install ASDF
git clone https://github.com/asdf-vm/asdf.git
. asdf/asdf.sh

# Install versions
asdf install erlang 28.4.1
asdf install elixir 1.19.5-otp-28
asdf install nodejs 25.8.1

# Set active versions
asdf set erlang 28.4.1
asdf set elixir 1.19.5-otp-28
asdf set nodejs 25.8.1

# Create app
gigalixir create

# Create database (optional)
gigalixir pg:create --free

# Link to repo
gigalixir git:remote <your-repo-url>

# Deploy
git push -u gigalixir main
```

**Versions Supported:**
- Phoenix: 1.8.5
- Elixir: 1.19.5
- Erlang/OTP: 28.4.1

### Render

Render has first-class support for Phoenix applications with guides for hosting with Mix releases and as a Distributed Elixir Cluster.

**Getting Started:**
1. Create a Render account
2. Connect your GitHub repository
3. Create a new Web Service
4. Render will auto-detect Elixir/Phoenix
5. Configure environment variables
6. Deploy

**Key Features:**
- Automatic build system (Railpack)
- Managed databases (PostgreSQL, MySQL)
- Native Docker support
- Auto-scaling capabilities
- Preview environments for pull requests

### Railway

Railway offers multiple deployment methods:
1. From a GitHub repository
2. Using CLI
3. One-click deploy from a template

**CLI Deployment:**

```bash
# Install Railway CLI
npm install -g @railway/cli

# Login
railway login

# Initialize project
railway init

# Deploy
railway up
```

**Recommended Settings:**
- Start command: `mix ecto.setup && mix phx.server`
- Environment variable: `MIX_ENV=prod`
- Database: Add PostgreSQL service
- IPv6: Set `ECTO_IPV6=true` for private networking

### Heroku

Heroku remains one of the most popular PaaS options for Phoenix deployment.

**Getting Started:**

```bash
# Install Heroku CLI
npm install -g heroku

# Login
heroku login

# Create app
heroku create my-app

# Add buildpack
heroku buildpacks:set https://github.com/HashNuke/heroku-buildpack-elixir.git

# Set environment variables
heroku config:set SECRET_KEY_BASE=$(mix phx.gen.secret)
heroku config:set DATABASE_URL=ecto://user:pass@host/database
heroku config:set PHX_HOST=my-app.herokuapp.com

# Deploy
git push heroku main
```

**Configuration:**
- Buildpack auto-detects Elixir
- Uses PostgreSQL by default (free tier available)
- Requires `runtime.exs` configuration
- Automatic SSL provided

### AWS

AWS provides multiple deployment options for Phoenix applications.

#### AWS EC2 with GitHub Actions

**Advantages:**
- Full control over infrastructure
- Cost-effective for sustained workloads
- Familiarity with AWS ecosystem

**Example GitHub Actions Workflow:**

```yaml
# .github/workflows/deploy.yml
name: Build & Deploy Phoenix (linux/x86) to EC2

on:
  push:
    branches: [ main ]
  workflow_dispatch:
  env:
    MIX_ENV: prod
    ELIXIR_VERSION: "1.18.4"
    OTP_VERSION: "27.3.4.2"
    BASE_OTP_VERSION: "27"
    APP_NAME: "myapp"
    PHX_PORT: "4000"
    PHX_HOST: "example.com"

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Build & Release
        run: |
          mix deps.get --only prod
          MIX_ENV=prod mix compile
          MIX_ENV=prod mix assets.deploy
          MIX_ENV=prod mix release

      - name: Package Release
        run: |
          cd _build/prod/rel/my_app
          tar -czf ../../my_app-${{ github.sha }}.tar.gz .

      - name: Upload Release Artifact
        uses: actions/upload-artifact@v4
        with:
          name: release-tarball
          path: my_app-${{ github.sha }}.tar.gz

  deploy:
    runs-on: ubuntu-latest
    needs: build
    steps:
      - name: Download Release
        uses: actions/download-artifact@v4
        with:
          name: release-tarball
          path: .

      - name: Configure SSH
        run: |
          mkdir -p ~/.ssh
          echo "${{ secrets.EC2_SSH_KEY }}" > ~/.ssh/id_rsa
          chmod 600 ~/.ssh/id_rsa

      - name: Deploy to EC2
        uses: appleboy/ssh-action@v1.0.0
        with:
          host: ${{ secrets.EC2_HOST }}
          username: deploy
          key: ${{ secrets.EC2_SSH_KEY }}
          port: 22
          script: |
            # Extract release
            cd /opt/myapp
            tar -xzf /tmp/my_app.tar.gz

            # Run migrations
            ./bin/my_app eval "MyApp.Release.migrate"

            # Restart application
            sudo systemctl restart myapp

            # Wait for health check
            sleep 5
            for i in {1..30}; do
              if curl -s http://localhost:4000/health | grep -q "ok"; then
                echo "Health check passed"
                break
              fi
              echo "Waiting for health check..."
              sleep 2
            done
```

**Key AWS Services:**
- **EC2**: Virtual machines for full control
- **Elastic Beanstalk**: Managed platform with auto-scaling
- **ECS/Fargate**: Container orchestration
- **Lambda**: Serverless functions (with some limitations)
- **RDS**: Managed PostgreSQL/MySQL
- **ElastiCache**: Redis caching
- **ALB/ELB**: Load balancing

### Google Cloud

#### Google Kubernetes Engine (GKE)

Google provides two main options for Phoenix deployment:

**Option 1: Google Kubernetes Engine**
- Managed Kubernetes cluster
- Auto-scaling and self-healing
- Regional deployment
- Integrated with Google Cloud services

**Option 2: Google Cloud Run**
- Serverless container platform
- Auto-scaling from zero
- Pay-per-use pricing
- Good for variable workloads

**Example GKE Deployment:**

```yaml
# kubernetes/deployment.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-app
  labels:
    app: my-app
spec:
  replicas: 3
  selector:
    matchLabels:
      app: my-app
  template:
    metadata:
      labels:
        app: my-app
    spec:
      containers:
        - name: my-app
          image: gcr.io/my-project/my-app:latest
          ports:
            - containerPort: 4000
          env:
            - name: PORT
              value: "4000"
            - name: PHX_HOST
              value: "example.com"
            - name: SECRET_KEY_BASE
              valueFrom:
                secretKeyRef:
                  name: my-app-secrets
                  key: secret-key-base
            - name: DATABASE_URL
              valueFrom:
                secretKeyRef:
                  name: my-app-secrets
                  key: database-url
          livenessProbe:
            httpGet:
              path: /health
              port: 4000
              initialDelaySeconds: 30
              periodSeconds: 10
          readinessProbe:
            httpGet:
              path: /health/ready
              port: 4000
              initialDelaySeconds: 5
              periodSeconds: 5
          resources:
            requests:
              memory: "256Mi"
              cpu: "250m"
            limits:
              memory: "512Mi"
              cpu: "500m"
---
apiVersion: v1
kind: Service
metadata:
  name: my-app
  labels:
    app: my-app
spec:
  selector:
    app: my-app
  ports:
    - port: 80
      targetPort: 4000
  type: ClusterIP
---
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: my-app
  annotations:
    kubernetes.io/ingress.class: nginx
    cert-manager.io/cluster-issuer: letsencrypt-prod
spec:
  tls:
    - hosts:
      - example.com
      secretName: my-app-tls
  rules:
    - host: example.com
      http:
        paths:
          - path: /
            pathType: Prefix
            backend:
              service:
                name: my-app
                port:
                  number: 80
```

## CI/CD Pipelines

### GitHub Actions for Phoenix

GitHub Actions allow you to build, test, and deploy Phoenix applications automatically.

**Elixir CI Action Template:**

```yaml
# .github/workflows/ci.yml
name: Elixir CI

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  test:
    runs-on: ubuntu-latest
    name: mix test
    services:
      db:
        image: postgres:15
        env:
          POSTGRES_USER: postgres
          POSTGRES_PASSWORD: postgres
          POSTGRES_DB: postgres

    steps:
      - uses: actions/checkout@v4

      - name: Setup Elixir
        uses: erlef/setup-beam@v1
        with:
          otp-version: "27"
          elixir-version: "1.18"

      - name: Restore dependencies cache
        uses: actions/cache@v4
        with:
          path: deps
          key: ${{ runner.os }}-mix-${{ hashFiles(''**/mix.lock') }}
          restore-keys: |
            ${{ runner.os }}-mix-${{ hashFiles('**/mix.lock') }}
            ${{ runner.os }}-build-

      - name: Install dependencies
        run: mix deps.get

      - name: Run tests
        run: mix test

  build:
    needs: test
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Restore dependencies cache
        uses: actions/cache@v4
        with:
          path: deps
          key: ${{ runner.os }}-mix-${{ hashFiles(''**/mix.lock') }}

      - name: Build Release
        run: |
          mix deps.get --only prod
          MIX_ENV=prod mix compile
          MIX_ENV=prod mix assets.deploy
          MIX_ENV=prod mix release

      - name: Upload Release
        uses: actions/upload-artifact@v4
        with:
          name: release-tarball
          path: _build/prod/rel/my_app

  deploy:
    needs: build
    if: github.ref == '"'"'"refs/heads/main'"'"''
    runs-on: ubuntu-latest
    steps:
      - uses: actions/download-artifact@v4
        with:
          name: release-tarball
          path: .

      - name: Deploy to Production
        run: |
          # Add your deployment command here
          echo "Deploying to production..."
```

### Preview Environments

**Bunnyshell** offers automatic preview environments for Phoenix:
- Zero CI/CD maintenance
- Automatic webhooks on pull requests
- Ephemeral environments created/destroyed on PR events
- Support for GitHub, GitLab, Bitbucket

**Setup:**
1. Connect Bunnyshell account
2. Link GitHub repository
3. Create project and environment
4. Toggle "Create ephemeral environments on pull request"

## Monitoring & Observability

### Telemetry Setup

Phoenix uses Telemetry for instrumentation. Configure it to export metrics.

```elixir
# lib/my_app/telemetry.ex
defmodule MyApp.Telemetry do
  use Supervisor
  import Telemetry.Metrics

  def start_link(arg) do
    Supervisor.start_link(__MODULE__, arg, name: __MODULE__)
  end

  @impl true
  def init(_arg) do
    children = [
      {:telemetry_poller, measurements: periodic_measurements(), period: 10_000}
    ]
    Supervisor.init(children, strategy: :one_for_one)
  end

  def metrics do
    [
      # Phoenix Metrics
      summary("phoenix.endpoint.start.system_time", unit: {:native, :millisecond}),
      summary("phoenix.endpoint.stop.duration", unit: {:native, :millisecond}),
      summary("phoenix.router_dispatch.start.system_time", tags: [:route], unit: {:native, :millisecond}),
      summary("phoenix.router_dispatch.stop.duration", tags: [:route], unit: {:native, :millisecond}),

      # Database Metrics
      summary("my_app.repo.query.total_time", unit: {:native, :millisecond}, description: "Total time spent on database queries"),
      summary("my_app.repo.query.queue_time", unit: {:native, :millisecond}, description: "Time queries spend waiting for a connection"),

      # VM Metrics
      summary("vm.memory.total", unit: {:byte, :megabyte}),
      summary("vm.total_run_queue_lengths.total"),
      summary("vm.total_run_queue_lengths.cpu"),
      summary("vm.total_run_queue_lengths.io")
    ]
  end

  defp periodic_measurements do
    [
      {MyApp.Telemetry, :measure_users, []},
      {MyApp.Telemetry, :measure_connections, []}
    ]
  end

  def measure_users do
    :telemetry.execute([:my_app, :users], %{total: MyApp.Accounts.count_users()}, %{})
  end

  def measure_connections do
    :telemetry.execute([:my_app, :connections], %{active: Phoenix.Tracker.list(MyAppWeb.Presence, "users") |> length()}, %{})
  end
end
```

Add Prometheus exporter to `mix.exs`:

```elixir
# Add to dependencies
{:telemetry_metrics_prometheus, "~> 1.1"}

# Add to application.exs
defmodule MyApp.Application do
  use Application

  def start(_type, _args) do
    children = [
      MyApp.Repo,
      MyAppWeb.Telemetry,
      MyAppWeb.Endpoint,
      # Add Prometheus exporter
      {TelemetryMetricsPrometheus, metrics: MyApp.Telemetry.metrics()}
    ]

    opts = [strategy: :one_for_one, name: MyApp.Supervisor]
    Supervisor.start_link(children, opts)
  end
end
```

### Health Check Endpoints

Add health check endpoints for load balancers and container orchestrators.

```elixir
# lib/my_app_web/controllers/health_controller.ex
defmodule MyAppWeb.HealthController do
  use MyAppWeb, :controller

  @doc """
  Basic health check - returns 200 if app is running.
  Used by load balancers and container health checks.
  """
  def index(conn, _params) do
    conn
    |> put_status(:ok)
    |> json(%{status: "ok"})
  end

  @doc """
  Deep health check - verifies database connectivity.
  Use for readiness probes in Kubernetes.
  """
  def ready(conn, _params) do
    case check_database() do
      :ok ->
        conn
        |> put_status(:ok)
        |> json(%{status: "ok", database: "connected"})
      {:error, reason} ->
        conn
        |> put_status(:service_unavailable)
        |> json(%{status: "error", database: reason})
    end
  end

  defp check_database do
    try do
      Ecto.Adapters.SQL.query!(MyApp.Repo, "SELECT 1", [])
      :ok
    rescue
      e -> {:error, Exception.message(e)}
    end
  end
end
```

Add routes to `router.ex`:

```elixir
# lib/my_app_web/router.ex
defmodule MyAppWeb.Router do
  use MyAppWeb, :router

  scope "/", MyAppWeb do
    pipe_through [:api]
    get "/health", HealthController, :index
    get "/health/ready", HealthController, :ready
  end
end
```

## Deployment Patterns

### Blue-Green Deployment

Blue-green deployment maintains two identical production-ready environments. At any given time, only one environment serves live traffic while the other remains on standby.

**How It Works:**
1. Deploy new version to Green environment
2. Test and validate Green environment
3. Instantly switch all traffic from Blue to Green via load balancer
4. Old Blue environment becomes backup/standby

**Advantages:**
- Instant rollback (just switch traffic back to Blue)
- Zero downtime deployment
- Extensive testing before full cutover
- Reduced risk exposure

**Disadvantages:**
- Double infrastructure cost (maintain two full environments)
- Increased complexity

**Load Balancer Configuration:**

```yaml
# AWS Application Load Balancer
resource "aws_lb_target_group" "blue" {
  name        = "${var.app_name}-blue"
  port        = 8080
}

resource "aws_lb_target_group" "green" {
  name        = "${var.app_name}-green"
  port        = 8080
}

resource "aws_lb_listener" "front_end" {
  load_balancer_arn = "${var.lb_arn}"
  port              = 443
  protocol           = "HTTPS"

  default_action {
    type             = "forward"
    target_group_arn = "${var.blue_tg_arn}"
  }
}
```

### Canary Deployment

Canary deployment gradually shifts a small percentage of traffic to a new version before exposing it to the entire user base.

**How It Works:**
1. Deploy new version to small percentage (e.g., 5%) of users
2. Monitor key metrics: performance, errors, user feedback
3. Gradually increase traffic percentage (10%, 25%, 50%, 100%)
4. Monitor throughout rollout

**Advantages:**
- Real-world validation with minimal exposure
- Quick detection of issues
- Gradual rollout reduces risks
- Automatic rollback based on metrics

**Disadvantages:**
- More complex to orchestrate
- Longer deployment time
- Requires robust monitoring and automation

**Canary Ingress with Nginx:**

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: myapp-canary
  annotations:
    # Enable canary mode
    nginx.ingress.kubernetes.io/canary: "25"
spec:
  tls:
    - hosts:
      - example.com
      secretName: myapp-tls
  rules:
    - host: example.com
      http:
        paths:
          - path: /
            pathType: Prefix
            backend:
              service:
                name: myapp
                port:
                  number: 80
```

### Rolling Deployment

Rolling updates incrementally replace instances of the old application version with new ones.

**How It Works:**
- Replace instances one at a time (e.g., 10% at a time)
- Each instance handles traffic during replacement
- No immediate traffic switch

**Advantages:**
- Simpler than blue-green
- More efficient resource utilization
- Good for frequent, smaller updates

**Disadvantages:**
- Slower full rollout
- Mixed versions during rollout
- Requires more sophisticated coordination

**Rolling Deployment Script:**

```bash
#!/bin/bash
# deploy.sh - Rolling deployment script
set -e

SERVERS="server1.example.com server2.example.com server3.example.com"
APP_DIR="/opt/my_app"
RELEASE_TAR="my_app.tar.gz"

echo "Starting rolling deployment..."

for SERVER in $SERVERS; do
  echo "Deploying to $SERVER..."
  
  # Upload new release
  scp $RELEASE_TAR deploy@$SERVER:/tmp/
  
  # Deploy on server
  ssh deploy@$SERVER << 'EOF'
  set -e
  
  # Extract new release
  cd /opt/my_app
  tar -xzf /tmp/my_app.tar.gz
  
  # Run migrations
  bin/my_app eval "MyApp.Release.migrate"
  
  # Restart application
  sudo systemctl restart my_app
  
  # Wait for health check
  sleep 5
  for i in {1..30}; do
    if curl -s http://localhost:4000/health | grep -q "ok"; then
      echo "Health check passed"
      break
    fi
    echo "Waiting for health check..."
    sleep 2
  done
  
  # Cleanup
  rm /tmp/my_app.tar.gz
  
  echo "Successfully deployed to $SERVER"
  
  # Wait before deploying to next server
  sleep 10
done

echo "Rolling deployment complete!"
```

## Production Checklist

Before deploying to production, verify these items:

### Security
- [ ] SECRET_KEY_BASE is set and secure
- [ ] SSL/TLS configured
- [ ] Database credentials are not in code
- [ ] Firewall rules configured
- [ ] Rate limiting enabled
- [ ] CSRF tokens configured

### Database
- [ ] Migrations tested on staging
- [ ] Backup strategy configured
- [ ] Connection pool sized appropriately
- [ ] Timeout values configured for production
- [ ] Read replicas configured for high availability

### Application
- [ ] Assets compiled with digest
- [ ] Static manifest generated
- [ ] Cache configuration appropriate for production
- [ ] Server mode enabled
- [ ] Logging configured for production
- [ ] Error tracking configured

### Monitoring
- [ ] Health check endpoint working
- [ ] Metrics collection enabled
- [ ] APM integration configured
- [ ] Logging configured for log aggregation
- [ ] Alerting rules configured
- [ ] Dashboard set up

### Infrastructure
- [ ] Load balancer configured
- [ ] Auto-scaling configured (if needed)
- [ ] CDN configured (if serving static assets)
- [ ] DNS configured
- [ ] SSL certificates valid
- [ ] CDN cache rules configured

### Performance
- [ ] Response time baselines measured
- [ ] Database query performance monitored
- [ ] Memory usage tracked
- [ ] CPU usage tracked
- [ ] Connection pooling tested
- [ ] Static assets cached appropriately

## Common Issues and Solutions

### Issue: "Could not find static manifest"

**Solution:**
```bash
# Compile assets
MIX_ENV=prod mix assets.deploy

# Or remove configuration from config/prod.exs
# Comment out: cache_static_manifest: "priv/static/cache_manifest.json"
```

### Issue: LiveView socket falls over to longpoll immediately

**Solution:**
- Ensure clustering is configured correctly
- Use Phoenix.PubSub.Redis adapter for multi-node deployments
- Configure sticky sessions on load balancer
- Check network connectivity between nodes

### Issue: Database connection pool exhaustion

**Solution:**
```elixir
# config/runtime.exs
pool_size = String.to_integer(System.get_env("POOL_SIZE") || "10")

# Configure connection timeout
timeout: 60_000
queue_target: 5_000
```

### Issue: Memory leaks in production

**Solution:**
```elixir
# config/prod.exs
# Enable BEAM VM monitoring
config :logger,
  level: :info,
  handle_otp_reports: true

# Set VM memory limits
config :my_app, MyAppWeb.Endpoint,
  # Configure websocket timeout
  websocket_timeout: 60_000
```

## Recommended Resources

### Official Documentation
- [Phoenix Deployment Guide](https://hexdocs.pm/phoenix/deployment.html)
- [Phoenix Releases Guide](https://hexdocs.pm/phoenix/releases.html)
- [Phoenix Fly.io Guide](https://hexdocs.pm/phoenix/fly.html)
- [Phoenix Gigalixir Guide](https://hexdocs.pm/phoenix/gigalixir.html)
- [Phoenix Heroku Guide](https://hexdocs.pm/phoenix/heroku.html)

### Community Guides
- [OneUptime Production Deployment](https://oneuptime.com/blog/post/2026-02-02-phoenix-production-deployment/view)
- [Bunnyshell Preview Environments](https://www.bunnyshell.com/guides/preview-environments-for-phoenix-elixir/)
- [Railway Phoenix Guides](https://docs.railway.app/guides/phoenix)

### Tools and Platforms
- [Fly.io](https://fly.io)
- [Gigalixir](https://gigalixir.com)
- [Render](https://render.com)
- [Railway](https://railway.app)
- [AWS Documentation](https://docs.aws.amazon.com)

---

**Last Updated:** 2026-03-21
**Next Review:** 2027-03-21
