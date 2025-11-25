Below is the **formal `speckit.plan`–style** version of the previous plan.
**No code**, strictly Markdown, following the structure and tone used in the example plans inside the spec-kit repo.

---

# `/speckit.plan`

## Title

**ServiceMarket — Service-Based E-Commerce Platform (Ash Framework / Elixir)**

## Overview

ServiceMarket is an Elixir/Ash-powered service marketplace platform where customers can browse, schedule, and manage service bookings, while providers manage offerings, tasks, and fulfillment workflows. The system delivers a realtime, collaborative experience using Phoenix LiveView, PubSub, and OTP (GenServer/Task/Supervisors), with a structured REST API for integration.
The platform focuses on services (not goods), supporting bookings, task boards with drag-and-drop reordering, notifications, and provider dashboards.

## Goals

* Provide a marketplace for service discovery, scheduling, and order management.
* Enable service providers to manage services, bookings, and workflow tasks through realtime, collaborative task boards.
* Deliver a LiveView-powered UI with instant updates backed by Phoenix PubSub.
* Expose REST APIs for Services, Bookings, Tasks, Projects/Groups, and Notifications.
* Maintain robustness and concurrency guarantees via OTP primitives and Ash’s declarative resource model.
* Allow easy integration with payment gateways and external calendars.

## Non-Goals

* No physical goods or inventory management.
* No multi-tenant billing engine in the initial release.
* No SPA frontend—minimal JS, LiveView handles UI.
* No GraphQL API (can be added later).
* No complex AI-driven provider recommendations initially.

## Users / Personas

* **Customer**: Browses services, schedules bookings, tracks progress.
* **Service Provider**: Manages service listings, bookings, and task workflows.
* **Technician/Worker**: Executes tasks, updates progress on task boards.
* **Admin**: Moderates listings, categories, and user accounts.
* **System/Automation**: Manages reminders, retries, scheduled tasks.

---

# Scope

## In-Scope

* User authentication & roles
* Service categories & services
* Bookings & scheduling
* Tasks & drag-and-drop task boards
* Projects/Service Groups (optional organizational grouping)
* Notifications (in-app + persistent storage)
* Realtime updates (LiveView + PubSub)
* REST API for core resources
* Provider dashboards
* Presence indicators
* Background processes for reminders & retries
* Postgres storage & Ash resource modeling

## Out-of-Scope (Initial Version)

* Full-blown payment processing (stubbed or simple integration only)
* Multi-region deployments
* Mobile applications
* Advanced analytics
* Marketplace disputes/resolution system
* Provider subscription billing

---

# Technical Vision

## Architecture Summary

ServiceMarket is built on **Elixir + Ash Framework** with PostgreSQL as the primary datastore.
Phoenix LiveView powers the UI while Phoenix PubSub distributes realtime updates across the system.
Ash resources define domain models, validations, relationships, policies, and actions.
OTP GenServers manage stateful or concurrent flows (e.g., booking conflict checks, in-memory throttling).
Background jobs (Oban optional) handle email, reminders, and payment callbacks.

## Key Advantages of Elixir & Ash

* **LiveView** → realtime UI without SPA complexity.
* **PubSub** → instant cross-user sync for boards, bookings, and notifications.
* **GenServer / OTP** → reliable concurrency, supervision, and fault tolerance.
* **Ash Framework** → declarative resources & APIs with drastically reduced boilerplate.
* **BEAM VM** → perfect for long-lived connections and collaborative workflows.

---

# System Components

## Backend Services

* **Phoenix Application**: handles HTTP, LiveView sessions, and APIs.
* **Ash Domain**: Resources and Actions for Services, Bookings, Tasks, Notifications.
* **Realtime Hub (PubSub)**: broadcasts updates for boards, bookings, and notifications.
* **Background Workers**: scheduled reminders, retries, provider notifications.
* **TaskBoard Engine**: manages drag-and-drop reordering and state transitions (Ash + PubSub).
* **Notification Engine**: emits persistent + ephemeral notifications to clients.

## Frontend

* **LiveView UI**:

  * Service list
  * Booking flow
  * Provider dashboard
  * Task board with drag-and-drop
  * Notifications panel
  * Presence indicators
* Minimal JavaScript (JS hooks for drag-and-drop only)

---

# Domain Model (Conceptual Resources)

### User

Attributes: name, email, role, profile info.
Roles: admin, provider, customer, worker.
Relationships: bookings, services, tasks, notifications.

### ServiceCategory

Categorized service listings (hierarchical).

### Service

Metadata: title, description, pricing, availability.
Provider-owned.
Linked to Bookings.

### Project / ServiceGroup

Optional container for multi-service engagements.

### Booking (Order)

Customer schedules a service.
States: pending → confirmed → in_progress → completed / canceled.
Triggers notifications and task creation.

### Task / WorkItem

Linked to Booking.
Attributes: status, position, board column, assigned user.
Supports drag-and-drop movement.

### Notification

Stored events for users.
Realtime + persistent.

### Audit / Activity

System and user actions recorded for traceability.

---

# API Surface (REST)

## Public APIs

* `GET /services`
* `GET /services/:id`
* `POST /bookings`
* `GET /bookings/:id`

## Provider APIs

* `POST /services`
* `PUT /services/:id`
* `GET /providers/:id/services`
* `GET /projects/:id/board`

## Task & Board APIs

* `PATCH /tasks/:id`
* `POST /tasks/reorder`
* `POST /tasks/assign`

## Notification APIs

* `GET /notifications`
* `POST /notifications/mark_read`

## Admin APIs

User moderation, category management, global settings.

---

# Realtime Features

## Live Updates

* Task board position changes
* Booking status updates
* Provider dashboards
* Customer order progress
* Presence (who’s online, who’s editing)

## PubSub

PubSub broadcasts after Ash actions complete.
All LiveViews subscribed to relevant topics re-render instantly.

## Presence

Used for:

* Active collaborators on task boards
* Provider dash “who’s online”
* Real-time edit awareness

---

# UX Interactions

## Task Board

* Drag-and-drop tasks between columns
* Local reordering
* Optimistic UI
* Conflict resolution through server reconciliation
* Multi-user collaborative editing

## Booking Flow

* Service → schedule selection → confirmation
* Realtime provider notifications
* Customer sees updates instantly

## Provider Dashboard

* Incoming bookings
* Task board view
* Notifications stream
* Live presence indicators

---

# Concurrency & Reliability

## Booking Consistency

* DB transactions for scheduling
* Optional GenServer locks for “hot” timeslots
* Optimistic locking for state-sensitive operations

## Supervisors

* Crash-safe background workers
* Taskboard engine worker pool
* Notification broadcaster

## Background Tasks

* Reminders
* Email/SMS updates
* Payment callbacks
* Retry logic

---

# Security & Access Control

* Session/JWT-based authentication
* Ash policies enforce resource-level authorization
* Role-based access
* Rate limiting on booking flows
* Sanitized inputs + Ash validations

---

# Deployment

* Elixir releases + Postgres
* Deploy target: Fly.io, Kubernetes, or VM
* Telemetry: metrics + logs + dashboards
* Zero-downtime LiveView deploys

---

# Milestones

1. **Foundation**

   * Project setup
   * User auth
   * Core Ash resources

2. **Service & Booking**

   * Service CRUD
   * Booking creation + workflows
   * Notifications (basic)

3. **LiveView UI (Phase I)**

   * Customer flows
   * Provider dashboard scaffold

4. **Task Board + Realtime**

   * Columns, positions, drag-and-drop
   * Presence
   * PubSub updates

5. **Background Systems**

   * Reminders
   * Optional payment integration

6. **Stabilization & Deployment**

   * Tests
   * Load testing
   * CI/CD pipeline
   * Production release

---

# Acceptance Criteria

* New bookings appear in provider dashboards within **1 second**.
* Moving a task updates all connected users’ boards instantly.
* Customers receive state change notifications for each booking.
* Providers can reorder tasks without refresh.
* Concurrent scheduling attempts are safely handled.
* All exposed REST endpoints return correct, validated JSON data.

---

# Risks & Mitigations

### Drag-and-drop Lag

**Mitigation:** Optimistic UI + server authoritative reconciliation.

### Booking Conflicts

**Mitigation:** DB transactions + optional GenServer slot locks.

### Complex provider workflows

**Mitigation:** Configurable task board columns and transitions.

### Ash learning overhead

**Mitigation:** Internal guidelines + progressive integration.

---

If you'd like, I can also provide:

✅ `/speckit.context`
✅ `/speckit.design`
✅ `/speckit.decisions`
or
✅ `/speckit.api` (full human-readable API contract)

Just tell me which one to generate next.
