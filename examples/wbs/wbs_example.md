# WBS — Internal Room Booking (BKG)

> Readable rendering of `wbs_example.xlsx`. The spreadsheet is the input the skills read; this file exists so the content is visible on GitHub. Keep the two in sync if you edit either.

**Layout.** Sheet `WBS`, header on row 1. Columns: `ID` · `Topic` · `User Story` · `Acceptance Criteria` · `Phase`.

An **epic** is an UPPERCASE `Topic` row with an empty `User Story`. The rows after it, until the next epic header, are its **parent stories**. `decompose-wbs-epic` splits those parent stories into child stories; the AC column here is the raw, pre-refinement note from scoping, not final acceptance criteria.

**This is a fragment** — two epics out of a larger WBS. Rows the Product Context cites but which are not here (R12, R14, R15, R16, R58, R60) belong to epics outside this example.

`ID` values are what the Product Context cites as `R-XX`. Numbers are stable within a WBS version; after a re-baseline they must be reviewed.

---

## ROOM MANAGEMENT

| ID | Topic | User Story | Acceptance Criteria | Phase |
|---|---|---|---|---|
| R17 | Add a room | As a User, I want to add a room to an office, so that it can be booked. | Office, name, capacity, equipment flags. Room name unique within the office. | MVP |
| R18 | Edit room attributes | As a User, I want to edit a room's attributes, so that the listing stays accurate. | Editable: name, capacity, equipment flags (screen, whiteboard, video). Office cannot be changed after creation. | MVP |
| R20 | Deactivate a room | As a User, I want to deactivate a room, so that it can no longer be booked. | Deactivated room disappears from booking search. Existing future bookings are kept. Reversible. | MVP |
| R21 | View rooms in scope | As a User, I want to see the rooms I manage, so that I can find one to edit. | Scope by office. List shows name, capacity, equipment, active flag. | MVP |
| R23 | Manage office settings | As a User, I want to set an office's working hours and timezone, so that bookings stay within them. | Working hours per office. Timezone per office. Applied to booking validation and display. | MVP |
| R24 | Import rooms from CSV | As a User, I want to import rooms from a CSV file, so that a new office is set up quickly. | CSV columns match room attributes. Validation report before commit. Duplicate room names rejected. | MVP |
| R26 | Manage the equipment flag list | As a User, I want to edit the list of equipment flags, so that new equipment types can be tracked. | Add, rename, retire a flag. Retiring a flag does not remove it from existing rooms. | Future |

## BOOKING MANAGEMENT

| ID | Topic | User Story | Acceptance Criteria | Phase |
|---|---|---|---|---|
| R41 | Book a room | As a User, I want to book a room for a time slot, so that I have a guaranteed space. | Conflict check at creation. Booking holds room, holder, start, end, attendees, status. Attendees optional. | MVP |
| R42 | See my bookings | As a User, I want to see my bookings, so that I know what I have reserved. | List view and calendar view. Own bookings only. | MVP |
| R43 | Cancel my booking | As a User, I want to cancel my booking, so that the room is freed for others. | Own bookings only. Room released immediately. | MVP |
| R44 | Move a booking | As a User, I want to move an existing booking, so that I do not have to cancel and rebook. | Change time, room, or both. Added 2026-08-25 after client answers. | MVP |
| R45 | Cancel any booking in my office | As a User, I want to cancel any booking in my office, so that I can resolve conflicts. | Office-scoped. Reason required. Holder notified with the reason. | MVP |
| R46 | No-show handling | As a User, I want unused rooms released automatically, so that they can be rebooked. | Grace period per room size. Booking auto-marked no-show, room released. | MVP |
| R47 | Recurring booking | As a User, I want to create a weekly recurring booking, so that I do not rebook every week. | Weekly only. Maximum 12 occurrences. Conflicting occurrence is skipped. | MVP |
| R48 | Booking notifications | As a User, I want booking notifications, so that I do not forget my meeting. | Confirmation at creation and reminder before start. Calendar invite attached. | MVP |
| R49 | Global booking rules | As a User, I want to set global booking rules, so that offices follow one policy. | Booking horizon, maximum duration, grace period editable by Admin. | Future |

---

## Notes for the reader

Two things in this WBS deliberately disagree with the Product Context, because that is what a real WBS does — it is a scoping baseline written earlier, and the Product Context is the resolved current state. `decompose-wbs-epic` is expected to surface both under **Gaps & conflicts** rather than silently follow either one:

- **R46** says the grace period varies by room size. The Product Context (§4.4) has it fixed globally at 15 minutes, with per-office configurability tracked as an open question. The Product Context wins; the WBS line is stale.
- **R24** puts a CSV room import in MVP as a product feature. The Product Context (§1, cutover note) treats loading room inventory as a one-time cutover activity, not a screen in the product. Whether an in-product import exists needs confirming.

Stories are written generically as `As a User` on purpose. The real role comes from the Product Context role list during decomposition — that is where a single WBS row may split into separate stories per role.
