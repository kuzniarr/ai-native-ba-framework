# Stakeholders — Internal Room Booking (BKG)
_Last updated: 2026-08-26_
_⚠ Internal — Power/Interest columns for the delivery team only_

> All people in this file are fictional. Per the project convention, client personal names never appear in client-facing artifacts — this internal map is where names live.

## Meridian Group (Client)

| Name | Role | Contact | Time Zone | P/I | Notes |
|---|---|---|---|---|---|
| Dana Kovar | Operations Director, **Sponsor** | email | CET | H/L | Budget owner; approves scope changes with a cost impact. Not in requirements meetings — joins milestone reviews only. Sign-off needs lead time booked a week ahead. |
| Milo Brandt | Workplace Experience Manager, **PO** | Slack `#bkg-client` | CET | H/H | Product decisions, requirements approval, story acceptance. Primary elicitation contact. Available Tue/Thu; async in between and reliable on email within a day. **Away 2026-09-15 → 2026-09-26** — Petra Illes covers operational questions, but scope decisions wait for his return. |
| Petra Illes | Office Manager, Berlin (pilot office) | email | CET | L/H | Represents the Office Manager role; validates flows and mockups. **Source of the manual no-show rule** — pushed back on fully automatic marking during the 2026-07-30 flow review. Covers operational questions in Milo's absence; does not approve scope. |
| Jonas Weiss | IT & Security Lead | email | GMT | H/L | Owns Workspace SSO, data policy, repo access, and the AI-usage agreement. Involve early on anything touching data or integrations; does not engage with product detail. |
| Employee reps | End users (rotating, 4–6 per demo) | via Milo | CET / GMT | L/H | Feedback on prototypes at demos. Group-level entry — individuals rotate per session. |

## Delivery Team

| Name | Role | Contact | Notes |
|---|---|---|---|
| Olena Rud | Project Manager | Slack `#bkg-delivery` | Delivery plan, budget tracking, ceremony facilitation, escalations. Primary client contact for progress and financial matters. |
| Andrii Bondar | Business Analyst | Slack `#bkg-delivery` | Requirements elicitation and documentation, Product Context ownership, AC, design validation, traceability. **50% allocation.** |
| Marek Vlk | Tech Lead | Slack `#bkg-delivery` | Architecture, ADR log, technical specification, estimates. **Primary contact for technical, implementation, and feasibility questions.** Owns the open technical questions in the Tech Context. |
| Ines Duarte | Product Designer | Slack `#bkg-delivery` | Flows and mockups; takes roles, permissions, and states from the Product Context. Part-time — 60h/month. |
| Backend engineers (×2) | Engineering | Slack `#bkg-delivery` | NestJS, PostgreSQL, integrations. |
| Frontend engineer | Engineering | Slack `#bkg-delivery` | React, calendar view. |
| QA engineer | Quality | Slack `#bkg-delivery` | Test planning, AC verification, regression, QA sign-off. |

## RACI

> R = Responsible, A = Accountable, C = Consulted, I = Informed

| Deliverable | BA | PO | Sponsor | PM | Tech Lead | Designer | Dev | QA |
|---|---|---|---|---|---|---|---|---|
| Requirements elicitation | R | C | I | I | C | C | — | — |
| Requirements specification | A | C | I | I | C | C | I | I |
| Requirements approval | C | A | I | I | I | I | I | I |
| Change request approval | C | A | A (budget impact) | C | C | I | I | I |
| Technical specification | C | I | — | I | A | — | C | — |
| Design validation vs requirements | A | C | — | I | I | R | I | C |
| Room inventory import | C | A | I | R | C | — | R | C |
| Pilot launch sign-off | I | A | A | C | C | — | — | C |
| Sprint demo / review | R | C | I | A | C | C | C | C |

## Influence / Interest Notes

> Internal use only — not shared with the client.

- **High Power / High Interest** (manage closely): Milo, Olena, Marek. Milo is the daily channel; surface budget-impacting decisions to Dana through Olena, never directly.
- **High Power / Low Interest** (keep satisfied): Dana, Jonas. Dana gets tight milestone summaries; Jonas gets early notice on data and integration topics and nothing else.
- **Low Power / High Interest** (keep informed): Petra, employee reps, designer, dev, QA. Petra's operational feedback shapes the no-show domain — treat it as input, not as approval.

## Open Questions

- [ ] Petra Illes — confirm whether she represents the Warsaw and London Office Managers until rollout, or Berlin only. Affects who validates flows for the other two offices.
- [ ] Employee reps — confirm whether the group is fixed before the pilot or picked per demo.
