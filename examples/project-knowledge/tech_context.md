# Tech Context — Internal Room Booking (BKG)
_Last updated: 2026-08-26_

> All data in this file describes a fictional project used as a working example.

> **Source reconciliation:** technical facts here are reconciled against the engineering **Technical Specification** owned by the Tech Lead, which is canonical for stack, data model, and API. Product decisions follow the **Product Context**. Where the two diverge, the divergence is flagged in Open Technical Questions rather than resolved silently. This file is a **decision-level digest** — field-level, API, and full-ER detail is fetched on demand, not mirrored here. **Reconcile trigger:** new ADR · data-model change · epic kickoff.

## Stack

| Layer | Technology | Notes |
|---|---|---|
| Frontend | React + TypeScript | Single-page app; the calendar view is the core screen. (ADR-002) |
| Backend | NestJS (Node.js) + TypeScript | Modular monolith; REST API. (ADR-003) |
| Database | PostgreSQL (Amazon RDS) | System of record: users, offices, rooms, bookings. Migrations in repo. (ADR-004) |
| Scheduled jobs | BullMQ + Redis | Grace-period timer and booking completion. (ADR-006) |
| Auth | Google Workspace SSO (OIDC) | **Roles resolved in the app database, not in the IdP.** (ADR-005) |
| Email | SendGrid | Transactional only; `.ics` attachments generated app-side. (ADR-007) |
| Infrastructure | AWS ECS Fargate + ALB | Staging + production; pilot behind a feature flag. |
| CI/CD | GitHub Actions → ECR → ECS rolling deploy | |
| Monitoring | CloudWatch + Sentry | |
| PM tool | Jira Cloud — project key `BKG` | |
| Design | Figma / FigJam | |

## External Integrations

| Integration | Purpose | Direction | Notes |
|---|---|---|---|
| Google Workspace SSO | Sign-in and user provisioning for all roles | Inbound | Single tenant. App validates the token, maps `sub` to a user, resolves the role from the app DB. **Workspace deactivation does not deactivate the app user** — app state is managed by an Admin (Product Context §2.3). (ADR-005) |
| SendGrid | Confirmations, reminders, move updates, cancellation notices | Outbound | Fixed templates in MVP; per-office customisation is Future Phase. `.ics` invites attached to confirmations and move updates. (ADR-007) |
| Room hardware (panels, sensors, badges) | Automatic check-in | — | **Future Phase.** No MVP integration. ⚠ The badge rollout is run by client IT on its own schedule; no interface contract exists yet — confirm ownership before the Future-Phase epic is estimated. |

> **Out of scope (MVP):** HR system sync, corporate directory write-back, mobile app, public API.

## Architecture Notes

**Pattern.** Modular monolith. One NestJS process with domain boundaries enforced as modules; no inter-service calls at MVP. Extracting a module to a separate service requires an ADR. (ADR-003)

**Request flow.** Browser → ALB → NestJS API → validate Workspace token → authorise against the app DB → PostgreSQL / Redis. Scheduled workers consume queued jobs → PostgreSQL → SendGrid.

**Module map (indicative).** Identity & access · Offices & rooms · Bookings · Notifications · Reporting. Modules communicate in-process; the shared kernel is auth context, office scoping, logging, error handling.

**Conflict enforcement.** The conflict check is enforced at the database level — an exclusion constraint on room + time range inside the booking transaction. The API-level check exists to produce the error message naming the conflicting holder; the constraint is the guarantee. This matters for the move flow too: a move is a transactional update under the same constraint, not a delete-and-create. (ADR-008)

**Time handling.** All timestamps are stored in UTC. Display and working-hours validation use the **office** timezone, never the device timezone. Offices span CET and GMT, so this is load-bearing. (ADR-009)

**Confirmed decisions (ADR log).** Custom build over an off-the-shelf booking tool (ADR-001) · React SPA (ADR-002) · NestJS modular monolith (ADR-003) · PostgreSQL (ADR-004) · Workspace SSO with roles in the app DB (ADR-005) · BullMQ for scheduled transitions (ADR-006) · SendGrid (ADR-007) · DB-level conflict constraint (ADR-008) · UTC storage with office-timezone display (ADR-009).

## Domain Model & Data

Decision-level view. Field-level detail (types, enums, phase tags) and the full ER diagram live in the engineering Technical Specification — fetch that when a task needs exact fields. This section carries what is needed to write stories and AC: which entities exist, what they mean, and how scoping works.

**Storage legend:** PG = system of record · Redis = ephemeral / queues · Workspace = credentials.

**Identity and access**
- **User** — account, home office, state (`active` / `deactivated`), Workspace link (PG + Workspace).
- **OfficeRole** — user ↔ office ↔ role. Many-to-many: one role per office, different roles possible in different offices (Product Context §2.1) (PG).

**Offices and rooms**
- **Office** — name, timezone, working hours. The scope unit for roles, rooms, and bookings (PG).
- **Room** — office, name, capacity, equipment flags, active flag (PG).

**Bookings**
- **Booking** — room, holder, start, end, status, source (single / recurring) (PG).
- **BookingAttendee** — booking ↔ user; invited attendees only, no status tracking in MVP (PG).
- **RecurrenceRule** — weekly rule, max 12 occurrences; occurrences are materialised as individual bookings at creation (PG).

**Tenancy and scope.** Every query on rooms, bookings, and users is scoped by office. An Office Manager's scope is the offices where they hold that role; an Admin bypasses office scoping. Scope is resolved once per request from `OfficeRole` and carried in the auth context.

## Key Technical Constraints

- All booking rules (conflict, horizon, duration, working hours) are enforced server-side; the UI mirrors them but is never the guarantee.
- No hard deletes on bookings — terminal states are retained for reporting.
- The 30-day horizon and 4-hour maximum are configuration constants in MVP, not editable data (Product Context §4.1).
- Scheduled jobs must be idempotent: a re-run of the grace-period timer must not re-mark an already terminal booking.

## Open Technical Questions

- [ ] Grace timer mechanism — one scheduled job per booking, or a periodic sweep? Affects load at scale. Owner: Tech Lead. `TBD`.
- [ ] Attendee lookup — read-only directory search via Workspace, or an app-side user list only? Affects the attendee picker and the capacity warning. Owner: Tech Lead. `TBD`.
- [ ] ⚠ Room-hardware interface — no contract exists and the rollout is client-owned. Confirm ownership before the Future-Phase epic is estimated.
