# Product Context — Internal Room Booking (BKG)
_Last updated: 2026-08-26_

> All data in this file describes a fictional product used as a working example. Client, people, and decisions are invented.

> **Reading the references.** Every statement carries an inline source — `(S4 transcript 00:28:30)`, `(S2 WBS R41)`, `(S1 §2.1)`. The sources behind `S1`–`S6` are listed in §10.1. Decisions taken after the last numbered source are cited by person and date instead.

## 1. Product overview

Internal meeting-room booking tool for Meridian Group, a professional-services company with offices in Berlin, Warsaw, and London (S1 §2.1). It replaces a per-office spreadsheet that cannot prevent double bookings and gives no visibility into room utilisation (S1 §1.3; S3 transcript 00:04:10). MVP covers three offices, 42 rooms, ~900 employees (S1 §2.1; S2 WBS R12).

Employees book rooms from a shared calendar; Office Managers govern rooms and resolve conflicts within their office; Admins manage access, offices, and global rules (S1 §2.2). Room hardware — door panels, occupancy sensors, badge check-in — is **Future Phase** and does not affect MVP behaviour (S2 WBS R58; S3 transcript 00:22:41).

MVP launch target is **November 2026**, piloted in Berlin before rollout to Warsaw and London (S3 transcript 00:06:55; S3 Decisions "Phased rollout"). All WBS rows tagged Phase: MVP are targeted for that window; no MVP row moves to Future Phase without a scope-change escalation.

> **Cutover note (project activity — not product behaviour).** Existing spreadsheet bookings are not migrated: the pilot starts from an empty booking table on a cut-off date, and the spreadsheet stays read-only for history (Milo Brandt, kick-off follow-up, 2026-07-16). Room inventory *is* loaded, from a CSV the client prepares per office; field mapping is client-owned, the import is tested by the team. Listed here only so the dependency is visible from the Product Context — it does not expand MVP scope.

## 2. Users and roles

### 2.1 Role hierarchy

Three roles, listed top-down. All three are defined in WBS R14 (S2 WBS R14):

- **Admin** — manages users, offices, rooms, and global booking rules; cross-office visibility. **Does not take part in day-to-day booking** — no creating, moving, or cancelling of own bookings in MVP (S1 §2.2; S3 transcript 00:12:30; confirmed Milo Brandt, 2026-08-25).
- **Office Manager** — governs rooms and bookings within their own office: cancels any booking in that office, marks no-shows, edits room attributes (S1 §2.2; S4 R7).
- **Employee** — books rooms, manages their own bookings, invites attendees (S1 §2.2).

**Role capability inheritance:** an Office Manager retains all Employee capabilities — they book rooms for themselves exactly as an Employee does (S4 transcript 00:18:05).

Role assignment rules (S2 WBS R15; refined by S4):
- Only an **Admin** assigns the Office Manager role, and only within a specific office (S4 R8).
- **Roles are scoped per office.** A user holds exactly one role per office but may hold different roles in different offices — rare, supported in MVP; the data model is many-to-many (user ↔ office ↔ role) (S3 transcript 00:14:52; S4 R9).
- **An Office Manager cannot assign or remove roles**, including their own (S4 R8).
- Default role on user creation is Employee (S2 WBS R15).
- **Last-Office-Manager safeguard:** an Admin cannot remove the Office Manager role from the only remaining Office Manager in an office — another must be assigned first (Milo Brandt, refinement 2026-08-12). Supersedes the earlier position that any role assignment could be reversed freely.

### 2.2 User attributes

Tracked on the user record (S2 WBS R16): first name, last name, work email, **home office** (single-valued; drives the default calendar view and the cross-office rule in §4.1), state (§2.3).

Users are provisioned from Google Workspace on first sign-in (§6). There is no separate registration flow (S5 §Tech notes).

### 2.3 User states and transition rules

- **active** — can sign in and book.
- **deactivated** — cannot sign in; future bookings held by this user are auto-cancelled and the rooms released; past bookings are retained for reporting (S4 R11; S4 transcript 00:26:14).
- Deactivation is reversible; reactivation does **not** restore cancelled bookings (S4 R11).
- Only an Admin deactivates or reactivates a user (S4 R11).
- Deactivating a Workspace account does **not** deactivate the app user — app state is managed separately by an Admin (S5 §Tech notes). Whether the two should be synced is **not defined in sources** — tracked as an open question.

## 3. Domain model

### 3.1 Office

Attributes: name, timezone, working hours (S2 WBS R23). An office is the scope unit for roles, rooms, and bookings. Offices span two timezones (Berlin and Warsaw CET, London GMT), so timezone is load-bearing, not cosmetic (S5 §Tech notes).

### 3.2 Room

Attributes: office, name, capacity, equipment flags (screen, whiteboard, video), active flag (S2 WBS R18).

A room set to **inactive** disappears from booking search but keeps its existing future bookings — those are **not** auto-cancelled, and the Office Manager resolves them case by case (S4 transcript 00:31:20; S4 Decisions "Inactive rooms"). Supersedes the earlier working assumption that deactivating a room releases its bookings.

### 3.3 Booking

Attributes: room, holder, start, end, attendees, status, source (single / recurring) (S2 WBS R41).

**Statuses and transitions:** `booked` → `checked-in` → `completed`; `cancelled` and `no-show` are terminal.

- `booked` → `checked-in` — manual, by the holder in the app; check-in opens 10 minutes before start, together with the reminder (S4 R12; S4 transcript 00:28:30). Hardware check-in is Future Phase (S2 WBS R58).
- `booked` → `no-show` — automatic, after the grace period elapses with no check-in (S4 R12).
- `booked` → `cancelled` — by the holder before start, or by an Office Manager at any time (S4 R13).
- `checked-in` → `completed` — automatic at end time (S4 R12).

**Holder** — the Employee who created the booking; the only Employee who can cancel or move it (S4 R13; S6 A1).

Bookings are never hard-deleted; terminal states are retained for reporting (S5 §Tech notes).

### 3.4 Recurring booking

Weekly only in MVP, maximum 12 occurrences (S4 R15; S4 transcript 00:38:02).

- Cancelling a series cancels all future occurrences; past ones are retained for history (S4 R16).
- If a single occurrence conflicts with an existing booking, that occurrence is **skipped** and the holder is notified — the whole series is not rejected (S4 R16).
- Moving a single occurrence **detaches** it: it becomes a standalone booking and no longer follows series-level cancellation (S6 A2).
- Whether a `no-show` on one occurrence affects the rest of the series is **not defined in sources** — tracked as an open question.

## 4. Functional domains

### 4.1 Booking creation

An Employee creates a booking for a room in their home office; an Office Manager creates bookings the same way (§2.1 inheritance). An Admin does not (§2.1).

Rules applied at creation, all enforced server-side (S5 §Tech notes):

- **Conflict check** — overlapping slots for the same room are rejected with an explicit message naming the conflicting slot and its holder (S2 WBS R41; S3 transcript 00:19:40).
- **Booking horizon — 30 days.** Anything further ahead is rejected (S3 Decisions "Booking horizon").
- **Maximum duration — 4 hours**, all roles. Longer slots are rejected (S4 R17; S4 Decisions "Booking duration").
- **Working hours** — bookings outside the office's working hours are rejected (S2 WBS R23).
- **Cross-office booking is not supported in MVP** — an Employee books only in their home office (S3 transcript 00:16:30; see §7).

**Attendees are optional.** Each attendee receives a calendar invite attached to the confirmation (S4 R19; §4.5). An attendee count above room capacity produces a **warning, not a block** (S4 transcript 00:35:40).

The 30-day horizon and the 4-hour maximum are **fixed values in MVP**; the admin UI for editing them is Future Phase (S2 WBS R49).

### 4.2 Moving a booking

**MVP — the holder moves their own booking** (time, room, or both) up to the start time (S6 A1). This closes the earlier question of whether editing exists at all; the previous working assumption was cancel-and-rebook, superseded by S6.

- A move **re-runs the full creation rule set** — conflict, horizon, duration, working hours (S6 A1).
- An Office Manager does **not** move other people's bookings; conflicts are resolved through cancellation with a reason (S6 A1).
- On a move, the holder and attendees receive an updated invite (S6 A1; §4.5).
- Moving after the start time is not possible for anyone — the booking can only be cancelled (S6 A1).

### 4.3 Cancellation

- An Employee cancels their **own** booking up to the start time (S4 R13).
- After the start time, only an Office Manager can cancel, within their own office (S4 R13; S4 transcript 00:29:55).
- An Office Manager cancellation **requires a reason**; the reason is included in the notice to the holder (S2 WBS R45; S4 R18).
- Cancellation releases the room immediately (§5).

### 4.4 Check-in and no-show handling

- **Check-in** opens 10 minutes before start and is performed by the holder in the app (S4 R12; S4 transcript 00:28:30).
- **Grace period is 15 minutes**, applied globally (S4 R12; S4 Decisions "Grace period"). Whether it should be configurable per office is **not defined in sources** — the global value is `proposed — confirm` and tracked as an open question.
- After the grace period the booking is auto-marked `no-show` and the room is released (S4 R12).
- An Office Manager can also mark a booking `no-show` **manually** before the grace period elapses — e.g. when the room is visibly empty (S4 R12; requested by Petra Illes, flow review 2026-07-30).
- **No-show statistics are visible to Admins and Office Managers only, never to Employees** — the client was explicit that this must not become a public leaderboard (S4 transcript 00:44:18; S4 Decisions).

### 4.5 Notifications

All notifications are email; there is no in-app notification centre in MVP (S5 §Tech notes).

- **Booking confirmation** to the holder at creation, with a calendar invite attached; attendees receive the same invite (S2 WBS R48; S4 R19).
- **Reminder** to the holder 10 minutes before start, carrying the check-in link (S2 WBS R48; S4 R12).
- **Updated invite** to the holder and attendees when a booking is moved (S6 A1).
- **Cancellation notice** when an Office Manager cancels — to the **holder only**, including the reason; attendees are not notified separately (S4 R18; S4 transcript 00:41:30). Supersedes the earlier draft that also notified attendees.
- **Series-skip notice** to the holder when a recurring occurrence is skipped on conflict (S4 R16).
- Templates are fixed in MVP; per-office or per-admin customisation is Future Phase (S2 WBS R49).

### 4.6 Room and office management

- **Room create / edit** — Admin across all offices; Office Manager within their own office (S1 §2.2; S4 R7).
- **Room deactivation** — same scope; existing future bookings are kept (§3.2).
- **Office create / edit, working hours, timezone** — Admin only (S1 §2.2).
- Room equipment flags are a fixed list in MVP (screen, whiteboard, video); an editable list is Future Phase (S2 WBS R18). Whether the client needs additional flags at launch is `TBD` — tracked as an open question.

### 4.7 User and access management

- **User list** — Admin sees all offices; Office Manager sees their own office (S4 R10).
- **Assign / remove the Office Manager role** — Admin only, per office, subject to the last-Office-Manager safeguard (§2.1).
- **Deactivate / reactivate a user** — Admin only (§2.3).
- Office Managers do not create users; provisioning is via Workspace sign-in (§6).

## 5. System rules and automations

Rules that run without a role triggering them:

- Rooms are released automatically on `no-show` and on cancellation; **no manual release action exists** (S4 R12).
- The grace-period timer (`booked` → `no-show`) and completion (`checked-in` → `completed`) run as scheduled jobs (S5 §Tech notes).
- Deactivating a user auto-cancels their future bookings and releases the rooms (§2.3).
- All times are stored in UTC and displayed in the **office** timezone, not the user's device timezone (S5 §Tech notes).
- Working hours are per office and are validated at creation and on every move (S2 WBS R23; S6 A1).

## 6. Integrations

| Integration | Purpose | Direction | Notes |
|---|---|---|---|
| Google Workspace SSO | Sign-in and user provisioning for all users | Inbound | Every employee already has a Workspace account; no separate credentials. Deactivation in Workspace does not deactivate the app user — see §2.3 |
| Corporate email | Confirmations, reminders, move updates, cancellation notices | Outbound | Calendar invites (.ics) attached to confirmations and move updates (S4 R19) |
| Room hardware (panels, sensors, badges) | Automatic check-in | — | **Future Phase**; no MVP integration (S2 WBS R58). Badge rollout runs as a parallel track (§8) |

## 7. Out of scope (MVP)

- Room hardware: panels, occupancy sensors, badge check-in (S2 WBS R58)
- Catering and equipment requests attached to a booking (S3 transcript 00:23:15)
- Cross-office booking (S3 transcript 00:16:30)
- Approval workflows for premium rooms — Future Phase (S2 WBS R60)
- Utilisation analytics beyond a basic no-show count — Future Phase (S4 transcript 00:46:02)
- Admin UI for global booking rules — Future Phase; MVP ships the §4.1 rules as fixed values (S2 WBS R49)
- In-app notification centre — email only in MVP (S5 §Tech notes)
- Migration of existing spreadsheet bookings — see the cutover note in §1

## 8. Parallel tracks

- **Badge-access rollout** — client IT is rolling out badge access across offices in parallel. MVP does not depend on it; Future-Phase hardware check-in will (S3 transcript 00:22:41; S5 §Tech notes). Status as of 2026-08: Berlin complete, Warsaw in progress, London not started.

## 9. Glossary

- **Booking** — a reserved time slot for one room, held by one Employee (S2 WBS R41).
- **Holder** — the Employee who created the booking; the only Employee who can cancel or move it (S4 R13).
- **Check-in** — the holder's in-app confirmation that the booking is in use; opens 10 minutes before start (S4 transcript 00:28:30).
- **No-show** — a booking whose holder did not check in within the grace period (S4 R12).
- **Grace period** — minutes after start time before a booking counts as a no-show (S4 R12).
- **Recurring booking** — a series of bookings created from one weekly rule (S4 R15).
- **Home office** — the office a user belongs to; determines where they can book (S2 WBS R16).
- **Office** — a physical location with its own rooms, working hours, and timezone (S2 WBS R23).

## 10. Source map

### 10.1 Sources processed

| ID | Source | Date | Type | Notes |
|---|---|---|---|---|
| S1 | Scope & Vision | 2026-06-30 | Document | Product framing, offices, roles |
| S2 | WBS (baseline) | 2026-07-02 | Spreadsheet | Scope baseline. **Re-baselined 2026-08-25** — R44 (move booking) added after S6 |
| S3 | Kick-off meeting | 2026-07-14 | Transcript + Decisions | Phased rollout, booking horizon, cross-office exclusion |
| S4 | Elicitation #1 | 2026-07-28 | Transcript + Decisions | Statuses, no-show, recurring bookings, notifications |
| S5 | Tech kick-off notes | 2026-08-04 | Notes | Timezones, scheduled jobs, SSO, server-side validation |
| S6 | Client answers on open questions | 2026-08-25 | Async (email) | A1 move booking · A2 occurrence detach |

Decisions taken after S6 are cited by person, venue, and date rather than a source ID — e.g. *(Milo Brandt, refinement 2026-08-12)*.

### 10.2 Traceability

| PC section | Source refs |
|---|---|
| §1 Product overview | S1 §1.3, §2.1; S2 WBS R12; S3 00:04:10, 00:06:55; Milo Brandt 2026-07-16 |
| §2.1 Role hierarchy | S1 §2.2; S2 WBS R14, R15; S3 00:12:30, 00:14:52; S4 R7–R9; Milo Brandt 2026-08-12 |
| §2.2 User attributes | S2 WBS R16; S5 §Tech notes |
| §2.3 User states | S4 R11; S4 00:26:14; S5 §Tech notes |
| §3.2 Room | S2 WBS R18; S4 00:31:20 |
| §3.3 Booking | S2 WBS R41; S4 R12, R13; S4 00:28:30 |
| §3.4 Recurring booking | S4 R15, R16; S4 00:38:02; S6 A2 |
| §4.1 Booking creation | S2 WBS R41, R23, R49; S3 00:19:40, 00:16:30; S4 R17, R19; S4 00:35:40 |
| §4.2 Moving a booking | S6 A1; S2 WBS R44 |
| §4.3 Cancellation | S2 WBS R45; S4 R13, R18; S4 00:29:55 |
| §4.4 Check-in and no-show | S4 R12; S4 00:44:18; Petra Illes 2026-07-30 |
| §4.5 Notifications | S2 WBS R48, R49; S4 R12, R16, R18, R19; S6 A1 |
| §4.6 Room and office management | S1 §2.2; S2 WBS R18; S4 R7 |
| §4.7 User and access management | S4 R8, R10, R11 |
| §5 System rules | S2 WBS R23; S4 R12; S5 §Tech notes |
| §6 Integrations | S2 WBS R48, R58; S4 R19; S5 §Tech notes |
| §7 Out of scope | S2 WBS R49, R58, R60; S3 00:16:30, 00:23:15; S4 00:46:02 |
| §8 Parallel tracks | S3 00:22:41; S5 §Tech notes |

## 11. Permission matrix

Legend: ✅ allowed · ✅ scoped — own office only · ❌ not allowed · ❓ open

| Domain | Capability | Admin | Office Manager | Employee | Source(s) | Confidence | Notes |
|---|---|---|---|---|---|---|---|
| Booking | Create a booking | ❌ | ✅ | ✅ | S3 00:12:30; Milo Brandt 2026-08-25 | Confirmed | Admin is out of day-to-day booking (§2.1); OM books as an Employee |
| Booking | Move own booking (before start) | ❌ | ✅ | ✅ | S6 A1 | Confirmed | Re-runs all creation rules |
| Booking | Move another user's booking | ❌ | ❌ | ❌ | S6 A1 | Confirmed | Conflicts are resolved by cancellation instead |
| Booking | Cancel own booking (before start) | ❌ | ✅ | ✅ | S4 R13 | Confirmed | |
| Booking | Cancel any booking in office | ❌ | ✅ scoped | ❌ | S4 R13; S3 00:12:30 | Confirmed | Reason required (S2 WBS R45) |
| Booking | Cancel a booking after start time | ❌ | ✅ scoped | ❌ | S4 R13; S4 00:29:55 | Confirmed | |
| Booking | Create a recurring series | ❌ | ✅ | ✅ | S4 R15 | Confirmed | Weekly, max 12 occurrences |
| No-show | Check in to own booking | ❌ | ✅ | ✅ | S4 00:28:30 | Confirmed | Opens 10 min before start |
| No-show | Mark no-show manually | ❌ | ✅ scoped | ❌ | S4 R12; Petra Illes 2026-07-30 | Confirmed | Before the grace period elapses |
| No-show | View no-show statistics | ✅ | ✅ scoped | ❌ | S4 00:44:18 | Confirmed | Never visible to Employees |
| Rooms | Create / edit a room | ✅ | ✅ scoped | ❌ | S1 §2.2; S4 R7 | Confirmed | |
| Rooms | Deactivate a room | ✅ | ✅ scoped | ❌ | S4 00:31:20 | Confirmed | Existing bookings are kept (§3.2) |
| Access | Assign / remove the Office Manager role | ✅ | ❌ | ❌ | S4 R8 | Confirmed | Per office; last-OM safeguard (§2.1) |
| Access | Deactivate / reactivate a user | ✅ | ❌ | ❌ | S4 R11 | Confirmed | Future bookings auto-cancelled (§2.3) |
| Access | View the user list | ✅ | ✅ scoped | ❌ | S4 R10 | Confirmed | |
| Config | Manage offices, working hours, timezone | ✅ | ❌ | ❌ | S1 §2.2; S2 WBS R23 | Confirmed | |
| Config | Edit global booking rules | ❓ | ❌ | ❌ | S2 WBS R49 | Partially Confirmed | No admin UI in MVP — rules are fixed values (§4.1); tracked as an open question for Future Phase |
