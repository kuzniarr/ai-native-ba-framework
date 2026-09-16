---
name: ears-ac
description: "Write Acceptance Criteria in EARS notation for an approved user story: numbered criteria grouped by meaning, a Data Dictionary for forms, one design link per story, and the story's open questions beneath it. Presentation follows the reader's experience; a coverage pass plus a state-reachability filter mean no real case is missed and no unreachable one is invented. Uses must, not shall. The source of truth is the current Product Context. Use after stories are decomposed and approved, before refinement."
---

# ears-ac

## Purpose

Write Acceptance Criteria in EARS notation for an approved user story. The output should be good enough that team refinement confirms and fills `TBD`s rather than rewrites. Content comes from the Product Context, the designs, and the raw WBS AC as a seed.

## Before writing any criterion — think from the user

The most common failure is writing what the system records, reports, or triggers elsewhere — the *consequences* of an action — instead of what the user does here. Before the first criterion, and again for each one, ask:

1. **Who is the user and what do they do here?** The action they take in this flow is the criterion.
2. **Is this a consequence rather than an action?** If the line describes a downstream record, report, notification, or a change owned by another story, it belongs in Related stories, not here.
3. **Is it one action, or several glued with "and"?** Split it, or drop the part that is not this story's.
4. **Would the story read complete without this line?** If yes, cut it.

Write the user's behaviour first. Add a consequence as a criterion only where the user directly observes it in this same flow.

## Inputs

1. **The approved story**, with its statement and parent WBS reference.
2. **Product Context** — the authoritative source. Read the **current** document as a whole and reconcile **every** criterion against it. A rule, value, message, role, or state relevant to this story may live in any section, not only the one matching the epic. Do not rely on memory of an earlier version. Where the Product Context and the raw WBS AC differ, the Product Context is the resolved state and the difference becomes an open question. Write `TBD` only after a full pass finds nothing.
3. **Designs** — the design-level source for behaviour and screen names. A concrete value visible on a mockup is real input, not a guess.
4. **WBS** — the parent story's raw AC as a seed.

## EARS patterns

| Pattern | Structure |
|---|---|
| Ubiquitous | The system must [response]. |
| Event-driven | When [trigger], the system must [response]. |
| State-driven | While [state], the system must [response]. |
| Optional feature | Where [feature is present], the system must [response]. |
| Unwanted behaviour | If [condition], then the system must [mitigation]. |

Use **must**, never *shall*. Clause order: While → When → the system must. Phrase from the user where it reads more simply (`the user can …`).

## Presentation — write for the reader

How criteria are arranged matters as much as what they say. A reader should move through them the way they move through the feature.

- **Order follows the reader's experience.** First the context or entry point, then the happy path in its natural sequence, then validation, then negative and edge cases. Do not scatter them.
- **A completing action never precedes the choice it depends on.** A criterion ending in "submit" or "save" comes after the criteria for the inputs it acts on.
- **Group top-down.** For a multi-faceted story, give the map before the detail: the high-level view first, then one whole branch, then the next, without mixing levels. Group names come from the story's own meaning, not from a fixed set like "Steps / Validation".
- **Choose the form by readability.** These are options, not templates — uniform stamping kills readability as surely as clutter does.
  - **Numbered criteria** — the default.
  - **Sub-point list under one criterion** — several homogeneous items sharing one outcome.
  - **Reference table** — several homogeneous items where each has its **own** short outcome. Use it whenever you find yourself writing near-identical criteria differing only in which item and what it does.
  - **Data Dictionary** — form fields with rules.
- **List versus table, the deciding test.** Same outcome for every item → sub-point list. A different outcome per item → reference table.
- **Branches stay visible.** When one trigger splits into outcomes, show every branch — never hide them behind "the system handles it". Grouping branches is fine; losing one is not.

## Coverage pass

Most gaps found in refinement are missing cases, not bad wording. Before finalising, walk these five and write a criterion for every case the flow can actually reach.

1. **Happy path and forward outcome.** The main success flow, ending with what happens next. The terminal outcome is the single most-forgotten criterion.
2. **Alternative branches.** Where one trigger splits by role, state, or input, surface each branch.
3. **Negative and error states.** Invalid input; an expired or already-used link; a provider error or user cancel; a blocked state — with the approved message from the Product Context where one exists.
4. **Edge cases.** The empty path; back-navigation and field persistence; an empty search result; an action already completed; an upstream change that must reset a downstream choice.
5. **Role and permission.** Do **not** write a standalone criterion restating who may perform the action — the role is already in the story statement. Name a role inside a criterion only where the behaviour itself **branches** by role, and then state only that branch.

This is completeness of behaviour, not bulk. Every criterion must still earn its place.

## State-reachability filter

Before writing a criterion about a state or condition, confirm the user can physically reach that state in **this** flow. If they cannot, do not write it.

Derive reachability from the state's own definition in the Product Context. Do not copy a state pair from one flow into another out of habit. A criterion describing a state the user can never be in on this screen is noise.

## Decided, proposed, or open — never invent behaviour as fact

Run this gate on every criterion before writing it.

1. **Full pass over the current Product Context first.** If it answers the point, in any section, the point is **decided**: write a plain criterion, no tag, no question.
2. **The Product Context is silent, but the call is the team's to make** — a standards default, a modelling detail, a routine judgment no client decision depends on. This is an **internal decision**: write it as a plain criterion, no tag and no question. Never route a decision you own to the client.
3. **The Product Context is silent and the point is a genuine product or client unknown.** Only now is it a question. Write the criterion with a working answer ending `(proposed — confirm)` and add the matching open question. Where no sound answer exists, write the open question alone and no criterion.

Two further cases:

- **Unknown value or copy** where the behaviour itself is decided: write the criterion with `` `TBD` `` in place and add the matching open question.
- **Non-functional requirement** — how fast, how many, under what load. Never a functional criterion. Park it in Open Questions flagged as an NFR so it is not lost. Functional empty and error states stay in the AC.

## Output — one block per story

Block and group headers in **bold**; the design link given once near the top, never per criterion; every `TBD` written as a `` `TBD` `` code chip.

````
**Story:** As a <role>, I want <action>, so that <benefit>.

**Design:** <one link, or several if the story spans screens>

**Pre-conditions:** <only for a state-driven flow that needs them>

**Acceptance Criteria**

**<Group name>**            (group by meaning, only where it helps)
1. <criterion>
2. <criterion>

**<Group name>**            (numbering continues, never resets)
3. <criterion>

**Data Dictionary**         (only for a form whose fields carry rules)
| Field Name | Field Type | Required? | Accepted Information | Comments |
|---|---|---|---|---|

**Related stories**         (parts of the flow owned by sibling stories)
1. <part of the flow that lives elsewhere> → see <story>

**Out of Scope**            (only what is deliberately not done)
1. <excluded, Future Phase, or not collected>

**Open Questions**
1. <one atomic question; propose a default where sensible>
````

## Acceptance criteria rules

- **Numbered, continuous across the story.** Group headings organise the list but never reset the count. Related stories, Out of Scope, and Open Questions each restart at 1.
- **No criterion title.** Each numbered item is the requirement itself, not `Create booking — When the user …`.
- **One shared outcome becomes one criterion with sub-conditions.** `The system must reject the booking if: 1) the slot overlaps an existing booking; 2) the slot falls outside the office's working hours.`
- **Behaviour, not interface.** State what the system does or the outcome — no controls, colours, menus, or widget names. Name a screen in prose only to disambiguate.
- **Atomic and explicit.** One requirement per criterion. Enumerate roles, values, and branches; never let "or / and" mask several requirements.
- **No vague verbs.** Never "restrict", "manage", "handle", "control". Spell out precisely what is and is not allowed.
- **Testable.** A concrete value or limit, or `` `TBD` ``. No "fast", no "user-friendly".
- **No meta criteria.** Do not write a criterion pointing at the Data Dictionary. The table plus one "outside its accepted information" criterion cover it.
- **No decision metadata.** A criterion states behaviour only — no source citation, no date, no section reference, no rationale, no "per <person>". That trace lives in the Product Context.
- **Behaviour belongs in criteria.** If a line describes what the system does, it is a numbered criterion, never prose in the preamble. Every piece of content has one home: behaviour → a criterion; an excluded item → Out of Scope; a flow owned elsewhere → Related stories. There is no catch-all Note block.
- **No inline cross-references.** A criterion never points at a sibling story. That belongs in Related stories.
- **Describe the user's action, not its downstream consequences.** What the action causes elsewhere belongs to the stories that own those entities and is named in Related stories.

## Optional blocks — only when they add clarity, never empty

- **Data Dictionary** — only for a form whose fields carry rules. Columns exactly: Field Name, Field Type, Required?, Accepted Information, Comments. Put the validation message, any default, and the visible label in Comments. A field captured in the table is not repeated in the criteria. A single field goes inline.
- **Related stories** — parts of this flow owned by siblings. These are dependencies, not exclusions.
- **Out of Scope** — only what is deliberately not done. Never list sibling stories here. Omit when nothing genuine applies; never write "n/a".
- **Pre- and post-conditions** — only for a state-driven flow where they aid clarity.

## House style

- **must**, never *shall*.
- **Block and group headers in bold.** No underline or colour — they rarely survive a paste into a wiki.
- **Every `TBD` as a code chip**, so it stands out and survives the paste. Never a bare or buried "tbd".
- **One design link near the top**, never repeated under each criterion.
- **Open questions are plainly written**, with a proposed default where sensible. Do not prefix an owner or route — the BA assigns that afterwards. Never reference a criterion by number inside a question; phrase it so it stands alone.
- English; one term per concept used identically throughout; no filler, no rationale inside a criterion, no pronouns, no emoji.

## Anti-hallucination

Content comes only from the approved story, the current Product Context, the designs, and the WBS seed. Introduce no field, message, role, branch, or rule that is not in them. Anything unknown becomes `` `TBD` `` plus an open question, or a criterion marked `(proposed — confirm)`. Never present a guess as decided.

## Example (abridged)

Written against the example Product Context in this repository.

````
**Story:** As an Employee, I want to book an available room for a time slot, so that I have a guaranteed space.

**Design:** Figma — Booking / Create

**Acceptance Criteria**

**Creating a booking**
1. The user selects a room, a start time, and an end time, and submits the booking.
2. The user can add attendees to the booking; attendees are optional.
3. When the booking is created, the system must set its status to `booked` and record the user as its holder.

**Validation**
4. The system must reject the booking if: 1) the slot overlaps an existing booking for the same room; 2) the start is more than 30 days ahead; 3) the duration exceeds 4 hours; 4) any part of the slot falls outside the office's working hours.
5. When the slot overlaps an existing booking, the system must name the conflicting slot and its holder in the rejection message.
6. When the number of attendees exceeds the room's capacity, the system must warn the user and still allow the booking.
7. The user can book only rooms in their home office.

**Confirmation**
8. When the booking is created, the system must send a confirmation to the holder with a calendar invite attached, and send the same invite to each attendee.

**Data Dictionary**
| Field Name | Field Type | Required? | Accepted Information | Comments |
|---|---|---|---|---|
| Room | select | Yes | an active room in an office where the user holds a role | rejection message names the conflicting holder |
| Start date and time | date-time | Yes | not in the past; no more than 30 days ahead; within the room office's working hours | granularity `TBD` |
| End date and time | date-time | Yes | after the start; no more than 4 hours after the start; within working hours | granularity `TBD` |
| Attendees | multi-select | No | `TBD` — no constraint on the attendee pool is stated in the Product Context | above room capacity shows a warning, not a block |

**Related stories**
1. Finding a room that is free in the wanted slot → see Search for rooms available in a time slot.
2. Changing the time or room after creation → see Move a booking.

**Out of Scope**
1. Cross-office booking — excluded from MVP.
2. Catering and equipment requests attached to a booking — excluded from MVP.

**Open Questions**
1. What time granularity does the slot picker use — 15, 30, or 60 minutes? Proposed: 15 minutes.
2. Can a user book a slot that has already started today, or only future slots? Proposed: future slots only.
3. Who can be added as an attendee — only users in the holder's home office, or anyone in the company? Not stated in the Product Context.
````

## Final gate — run before delivering

1. Numbering is continuous across all AC groups; Related stories, Out of Scope, and Open Questions each restart at 1.
2. A Data Dictionary is present if the story has a form with field-level rules, and no field in the table is repeated in the criteria.
3. Every `TBD` is a code chip and has a matching open question.
4. Every `(proposed — confirm)` has a matching open question. No orphan tags.
5. No sibling-story reference appears inline in a criterion.
6. No NFR is written as a functional criterion.
7. Each criterion is atomic — no "or / and" hiding several requirements.
