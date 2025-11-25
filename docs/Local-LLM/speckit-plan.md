# Spec Plan — Service Marketplace (Ash Framework / Elixir)

**Name:** ServiceMarket (e-commerce for services)
**Summary:** A marketplace for booking and managing services (not physical goods). Built with Elixir/Ash Framework, PostgreSQL, Phoenix LiveView for realtime UI. Exposes a REST API for Projects/Services/Tasks/Bookings/Notifications and supports drag-and-drop task/board interactions with realtime updates.

---

## High-level goals

* Enable service providers to list services and manage bookings.
* Enable customers to browse, book, and track service tasks.
* Provide a drag-and-drop board view (kanban) for service workflows with realtime collaboration.
* Expose REST APIs: Projects (or ServiceGroups), Services, Tasks (work items), Bookings/Orders, Notifications.
* Strong consistency for bookings, resilient background processing for long-running jobs, and realtime notifications.

---

## Tech stack

* **Backend language / framework:** Elixir + Ash Framework (resources + APIs)
* **Web framework / realtime UI:** Phoenix + LiveView (server-rendered, realtime)
* **Database:** PostgreSQL (Ash Postgres adapter)
* **Pub/Sub / messaging:** Phoenix.PubSub (or Ash PubSub integration)
* **Background jobs / workers:** GenServer / Task / Oban (optional)
* **Authentication:** Guardian / Pow / Ash authentication patterns (token and session)
* **Frontend:** Phoenix LiveView + minimal JS hooks for drag/drop and client behaviors (Blazor equivalent replaced by LiveView)
* **APIs:** REST endpoints generated/defined via Ash APIs (JSON), optionally GraphQL via Absinthe if later desired

---

## Actors / Roles

* **Admin** — full site management, manage providers, categories, global settings
* **Service Provider** — create/maintain services, manage schedule, accept/decline bookings, manage tasks on boards
* **Customer** — browse services, make bookings, view order and task progress
* **Worker / Technician** — assigned to tasks, update status on boards
* **System / Scheduler** — background processes for reminders, retries, external integrations

---

## Core Domain Resources (Ash-style conceptual list)

* **User** — attributes: name, email, role, profile, contact info
* **ServiceCategory** — hierarchical categories for services
* **Service** — title, description, price model (fixed / hourly), provider_id, availability windows, metadata
* **Project / ServiceGroup** — optional grouping of related services for big contracts
* **Booking / Order** — customer_id, service_id, scheduled_time, status (pending, confirmed, in_progress, completed, canceled), payment_info_reference
* **Task / WorkItem** — booking_id, assigned_to, status, priority, board_column, position (for ordering)
* **Notification** — user_id, type, payload, read_at, priority
* **ProviderProfile** — provider-specific details, ratings, response time SLA
* **Audit / Activity** — timeline entries for changes (for history & audit)

*(Each resource will define validations, aggregates, relationships and actions in Ash — planned but not coded here.)*

---

## APIs (REST)

### Public / Customer APIs

* `GET /services` — list / search services
* `GET /services/:id` — service detail
* `POST /bookings` — create booking (auth required)
* `GET /bookings/:id` — view booking status

### Provider / Project APIs

* `GET /providers/:id/services` — provider services
* `POST /services` — create service (provider auth)
* `PUT /services/:id` — update service
* `GET /projects/:id/board` — retrieve board state (tasks grouped by column)
* `PATCH /tasks/:id` — update task (status / assignee / position)

### Tasks & Boards

* `POST /tasks/reorder` — reorder tasks within/between columns (server accepts new positions)
* `POST /tasks/assign` — assign user to task

### Notifications

* `GET /notifications` — list user notifications
* `POST /notifications/mark_read` — mark notifications read

### Admin

* CRUD endpoints for users, categories, site settings

*(All endpoints return JSON; use standard HTTP status codes and include pagination and filtering.)*

---

## Realtime / UI plan

* **Primary UI:** Phoenix LiveView pages for:

  * Service catalog & search
  * Provider dashboard
  * Drag-and-drop Kanban board for bookings/tasks
  * Booking flow & realtime status updates
* **Drag-and-drop:** implement with LiveView + JS hooks to capture drag events and push minimal events to the server to update task positions. Server updates broadcast via PubSub.
* **Realtime updates:** when tasks/bookings change, publish events on Phoenix.PubSub; LiveView subscribers update clients instantly (no polling).
* **Presence:** use Phoenix Presence for showing who’s viewing/editing a board and for optimistic collaboration cues (e.g., “Ari is editing Task X”).
* **Optimistic UI:** small client-side interactions via JS hooks for snappy drag UX, with server authoritative ordering.

---

## Realtime architecture (how updates flow)

1. Client drag-drop triggers a LiveView event (or JS hook + pushEvent).
2. LiveView calls Ash action to reorder/update Task resource.
3. Ash action persists change to Postgres.
4. After commit, an Ash event / Phoenix PubSub broadcast is emitted (notification + board update).
5. Interested LiveView processes receive it and re-render; browser updates in-place.

---

## Background & concurrency strategy

* **GenServer(s):** manage short-lived booking state machines, throttling, and in-memory caches if needed (e.g., rate limiter, transient locks for concurrent booking attempts).
* **Supervision trees:** each worker / scheduler managed under supervisors for restartability.
* **Long-running / scheduled jobs:** prefer Oban (database-backed job processing) for retries, heavy tasks (notifications, external payments). Use GenServers for small, low-latency coordination tasks.
* **Transactions / consistency:** use DB transactions for booking & task moves when atomicity is required. Use optimistic locking/versioning for concurrency-sensitive records.

---

## Notifications & communication

* **Immediate in-app notifications:** Phoenix.PubSub + LiveView push to connected clients.
* **Persistent notifications:** stored in `Notification` resource (so users who are offline can fetch them).
* **Delivery channels:** in-app (LiveView), email (via Swoosh), SMS/webhooks via background jobs.
* **Notification patterns:** event-driven (on booking change, task reassignment, booking reminders).

---

## Security & Authorization

* **Authentication:** JWT/session + refresh tokens; support provider OAuth later.
* **Authorization:** resource-level policies (Ash supports policy engines) with role checks (Admin/Provider/Customer).
* **Data validation & sanitization:** server-side validations in Ash resources for inputs and business rules.
* **Rate limiting & abuse controls:** GenServer or Plug-level rate limits for sensitive endpoints (bookings, logins).

---

## Extensibility & integrations

* **Payments:** integrate Stripe or local payment gateways via background jobs.
* **Calendar sync:** webhook or OAuth with Google Calendar for provider schedule sync.
* **External CRM / accounting:** webhooks for export of bookings/invoices.
* **GraphQL (optional):** add Absinthe on top for richer client integration later.

---

## Testing & Quality

* **Unit tests:** resource actions & validations (Ash resource tests).
* **Integration tests:** API endpoints, LiveView flows for critical paths (booking creation, task board operations).
* **Realtime tests:** simulate PubSub broadcasts, presence usage, concurrency scenarios.
* **Load & concurrency tests:** booking contention, many simultaneous board updates.
* **CI:** run test suite, static checks (Credo), Dialyzer, and release build pipeline.

---

## Data migrations & deployment

* **Migrations:** Ecto migrations for Postgres (or Ash Postgres managed migrations). Keep migration plan with rollbacks.
* **Deploy:** build releases with Mix release; use Docker images with minimal Elixir runtime (distillery/mix release). Deploy to Kubernetes, Fly.io, or traditional VM.
* **Observability:** logs (Logger), metrics (telemetry), tracing for long flows. Monitor queue/backlog health (Oban dashboard).

---

## Milestones (example)

1. Project scaffolding, DB models (Ash resources) and core REST endpoints.
2. Authentication + provider/customer flows + bookings persistence.
3. LiveView-based provider dashboard + simple board UI (no DnD).
4. Add drag-and-drop reordering + server-side ordering persistence.
5. Realtime notifications + presence.
6. Background jobs integration (reminders, payments).
7. Hardening, tests, deployment automation.

---

## Acceptance criteria (example)

* User can create a booking that becomes a task assigned to a provider.
* Provider sees booking appear on their LiveView board within 1s of creation.
* Users receive persistent notifications for booking state changes.
* Dragging a task between columns updates the DB and broadcasts to all connected clients.
* Concurrent booking attempts for the same slot are handled safely (only one confirmed).

---

## Non-functional requirements

* **Scalability:** horizontally scale Phoenix endpoints and job workers; stateful processes remain small and supervised.
* **Latency:** LiveView updates should be sub-second for typical events.
* **Durability:** bookings and critical notifications persisted in DB and retried on failure.
* **Resilience:** supervisors ensure workers restart on crashes.

---

## Why use Elixir & Ash for this project (key advantages)

* **Realtime-first (Phoenix LiveView):** LiveView gives server-rendered, stateful UI with realtime DOM updates — ideal for collaborative task boards and instant booking status updates without a heavy SPA. This dramatically reduces frontend complexity compared to client-side frameworks while keeping excellent UX.
* **Built-in PubSub & Presence:** Phoenix.PubSub and Presence make broadcasting updates and tracking connected users simple and robust — perfect for multi-user boards, presence indicators, and live notifications.
* **Concurrency & fault tolerance (GenServer & OTP):** Elixir/OTP primitives (GenServer, Supervisors) allow modeling in-memory coordinators, locks, and stateful processes safely. Supervisors provide automatic restart strategies for reliability.
* **Ash Framework productivity:** Ash provides declarative resource definitions, actions, and policies — dramatically reducing boilerplate for CRUD, validations, and API surface. Ash's adapters (Ash Postgres) streamline persistence, and Ash APIs can centralize REST endpoints with consistent behavior.
* **Performance & scalability:** BEAM (Erlang VM) excels at handling many concurrent connections (WebSocket/LiveView) and long-running processes with low memory overhead. Good fit for many simultaneous users interacting with boards and realtime updates.
* **Ecosystem compatibility:** Easy integration with mature Elixir libraries (Oban for jobs, Swoosh for email, Telemetry for metrics) and straightforward deployment via releases or container images.
* **Safe concurrency patterns:** Compared to ad-hoc locking solutions, OTP patterns encourage explicit, monitored processes for stateful coordination (reducing race conditions and improving maintainability).

---

## Risks & mitigations

* **Drag-and-drop latency on high-latency networks:** Use optimistic UI/local reordering and background reconciliation. Keep server the source of truth but reduce chattiness by batching reorder operations.
* **Concurrent booking conflicts:** Use DB transactions and optimistic locks; consider dedicated GenServer or distributed locking for hot slots.
* **Learning curve for Ash:** Ash is powerful but opinionated; allocate time for developers to learn resource/action patterns and lifecycle hooks.

---

## Notes / Next steps (non-code)

* Create a mapping from business rules to Ash resource actions (create_booking, confirm_booking, cancel_booking, move_task).
* Draft API contracts (JSON shapes) for each endpoint and notification payloads.
* Prototype the LiveView board UI to validate UX for drag-and-drop and presence indicators.
* Decide on background job system (Oban recommended) and payment provider integration details.

---

If you want, I can now convert this plan into a formal `speckit.plan`-style spec (fields mapped to the spec-kit schema) in plain Markdown (still no code) — or expand any section (API contract examples, DB schema outline, or acceptance tests). Which section should I expand next?
