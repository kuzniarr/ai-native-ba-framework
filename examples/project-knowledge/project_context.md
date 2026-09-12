# Project Context — Internal Room Booking (BKG)
_Last updated: 2026-08-26_

> All data in this file describes a fictional project used as a working example.

## Project

Internal meeting-room booking tool for Meridian Group (professional services, ~900 employees, offices in Berlin, Warsaw, London). Replaces per-office booking spreadsheets that cannot prevent double bookings. Delivery model is T&M with a fixed MVP target of November 2026. The delivery team covers planning → design → development → QA → pilot launch. Project codename: **BKG**.

Product scope, roles, and permissions live in the Product Context, not here. This file covers the delivery side.

## Key Constraints

- **MVP launch:** November 2026, pilot in Berlin only; rollout to Warsaw and London after the pilot month.
- **BA allocation:** 50%. Elicitation is batched — one session every two weeks, async questions in between.
- **Client availability:** PO available Tue/Thu; sponsor joins milestone reviews only.
- **Time zones:** client CET/GMT, delivery team EET. Effective overlap is roughly four hours.
- **Sprints:** 2-week. Ceremonies Tue/Thu.
- **Approvals:** every product decision requires client sign-off; nothing ships on team assumptions.
- **Compliance:** WCAG AA; employee data stays inside the client's Google Workspace tenant.

## Risks

| Risk | Impact | Notes |
|---|---|---|
| No-show flow depends on two unanswered questions | High | Grace-period configurability and series behaviour on no-show are open. Escalate if unanswered two sprints before launch. |
| Offices keep the spreadsheet running in parallel | Med | Pilot success criterion covers this; adoption owned by the client sponsor. |
| Single elicitation channel (biweekly) | Med | Mitigated by async question lists sorted most-blocking-first. |
| Room inventory CSV quality | Med | Client-owned field mapping; a bad import blocks the pilot. Import tested on staging two weeks before cutover. |

## Project Identifiers

| Tool | Value |
|---|---|
| Project codename | BKG |
| Jira | Jira Cloud, project key `BKG` |
| Confluence | Space `BKG` — epic specifications |
| Figma / FigJam | Client-owned org account; flows in FigJam, mockups in Figma |
| Slack | `#bkg-delivery` (internal), `#bkg-client` (shared) |
| Repo | Client GitLab (access via client IT) |
| Staging / QA URLs | TBD — environments not provisioned |

## Project Knowledge — Source Map

| Logical name | File | Origin | Trust level | Notes |
|---|---|---|---|---|
| Product Context | `product_context.md` | BA synthesis from S1–S6 | ✅ Primary source of truth for product | Scope, roles, permissions, domain model, glossary. First reference for any product question. |
| Project Context | `project_context.md` | BA synthesis — delivery | ✅ Delivery context | This file. Constraints, risks, identifiers, conflict resolution, conventions, DoR. |
| Tech Context | `tech_context.md` | BA synthesis, reconciled with the tech lead | ✅ Baseline (BA-facing digest) | Stack, integrations, architecture, constraints. Decision-level digest — field-level and API detail is fetched on demand, not mirrored. |
| Stakeholders | `stakeholders.md` | BA synthesis | ✅ Baseline | Contacts, decision authority, RACI. |
| WBS | `wbs.xlsx` | Delivery WBS, client-confirmed | ✅ Scope baseline | Source for the `R-XX` row numbers cited throughout the Product Context. **Re-baselined 2026-08-25** — R44 added. |
| Requirements (written) | Confluence space `BKG` | BA | ✅ Canonical for the current wording of a story + AC | One page per epic. Holds the **wording**; the Product Context holds the **decision** behind it — see Conflict Resolution, point 5. |

Sources S1–S6 listed in Product Context §10 are **inputs to BA synthesis, not files in Project Knowledge**. Project Knowledge holds synthesised outputs only.

## Conflict Resolution Rule

1. **Product Context is canonical** for product, roles, permissions, glossary, and scope. In a conflict with any other file, the Product Context wins.
2. **WBS row citations in the Product Context** take precedence over any narrative interpretation of the WBS.
3. **Delivery-side files** (Tech Context, Stakeholders) are authoritative within their own domain — stack, team — and never override the Product Context on a product decision.
4. **New decisions** from elicitation or client messages enter the Product Context first, then propagate to dependent artifacts.
5. **Requirement wording vs product decision:** the Confluence epic page is canonical for the current **wording** of a story and its AC; the Product Context is canonical for the **decision** behind that wording; Jira carries what the team builds. Where a page and the Product Context disagree, the Product Context wins on the decision and the page is corrected — never the reverse.
6. **WBS ↔ Product Context discrepancies are surfaced to the BA before applying** — never reconciled silently. Precedence (1–2) decides the outcome; this point governs the procedure.

## Conventions

A convention is a rule that would otherwise have to be re-stated in every chat.

- **WBS row references** are cited as `R<number>` (e.g. `R41`). Numbers are stable within a WBS version; after a re-baseline, references must be reviewed.
- **Source citations in the Product Context** use `Sx §<section>` or `Sx transcript HH:MM:SS`. Decisions taken after the last numbered source are cited by person, venue, and date.
- **Story references** are by title, not ordinal number — numbers drift on renumbering.
- **`proposed` / `TBD` tags:** any requirement element tagged `proposed` or `TBD` must have a concrete open question whose answer clears the tag. No orphan tags.
- **Statement labelling:** non-trivial statements are tagged `[client decision]` / `[inference]` / `[assumption to confirm]`. An inference is never presented as a decision.
- **Dates** are ISO `YYYY-MM-DD` in every artifact.
- **Skill fidelity:** where an activity has a dedicated skill, follow that skill and do not deviate. Any story + AC written for Confluence or Jira is produced with `ears-ac` — never freehand, not even for a single criterion.
- **No source citations in client-facing requirement text.** Traceability lives in the Product Context source map, not in the story.
- **No client personal names** in requirements, changelog, or any client-facing artifact. The company name is allowed. Internal artifacts may name freely.
- **Client questions** are written to a quality bar: one decision per question, filler stripped, our stance shown (confirm / clarify / challenge), ordered most-blocking-first, surfaced as a full set rather than drip-fed.
- **Changelog trigger:** write a changelog item when an output changes baselined scope or adds a feature. A clarification that changes nothing baselined gets no item.
- **AI usage frame:** AI use on the project is agreed with the client and documented in the BA Approach. Artifacts are checked for sensitive data before any upload.

## Definition of Ready

A story is Ready when all of the following hold. Items 1–5 are the BA's; 6–7 are shared with the team.

1. **Story statement** is in `As a <role>, I want <capability>, so that <value>` form, uses canonical role names, and is traceable to its parent WBS row (`R-XX`).
2. **Acceptance Criteria** are written with `ears-ac` — numbered, grouped by meaning, EARS `must`, with a Data Dictionary wherever the story carries a form.
3. **One design link** is on the story. A story whose screens are not designed is not Ready — it goes back to the design track, not into a sprint.
4. **No orphan tags** — every `proposed` or `TBD` element has a concrete open question behind it, routed to Client or Internal.
5. **No unanswered blocking question.** A story with an open blocker is not Ready even when every other field is filled; non-blocking questions may travel with the story.
6. **Refined with the team**, and the feedback from that refinement is applied to the requirement, not just noted.
7. **In Jira** — the ticket sits under its epic with the story + AC in the description and an estimate derived from the WBS.
