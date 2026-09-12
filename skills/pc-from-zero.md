---
name: pc-from-zero
description: "Build a project's Product Context (`product_context.md`) from scratch by incrementally synthesizing sources the BA provides — WBS, discovery transcripts, PRD, kick-off notes, client answers — into one traceable product source of truth. Use when there is no Product Context file yet and you are creating the skeleton and its first population. Owns the empty §1–§11 skeleton, the per-source incremental loop, inline source traceability, and finalize. Once the file exists and a new source must be folded in, use pc-update instead."
---

# Product Context — from zero

Build `product_context.md` as the single product source of truth, synthesized only from sources the BA provides. The document grows source by source; this skill owns the empty skeleton and the first build through to a usable draft.

## When to use vs pc-update

- **pc-from-zero (this skill):** no Product Context file exists. Create the §1–§11 skeleton and populate it from the first sources until the BA has a coherent draft.
- **pc-update:** the file already exists and a new source must be folded in as targeted find/replace edits. Hand off once the file is established.

The seam matters: this skill may create and shape the file; pc-update only patches an existing one.

## Inputs

Any combination, provided gradually and in an order the BA chooses. Do not expect specific documents and do not constrain the source list in advance. Typical: WBS, discovery or elicitation transcripts, client PRDs, kick-off notes, client answers to open questions.

## Core principles

These are the reason the document stays trustworthy. Hold them on every source.

1. **Know nothing about the product upfront.** All product knowledge comes only from sources the BA adds. Nothing comes from other projects or from what this kind of product usually does.
2. **Do not invent.** If a source does not cover something, write `TBD` and mark it as an open question inline — never fill the gap with a guess.
3. **Do not hedge.** No "likely", "presumably", "design assumption". If sources are silent, write "not defined in sources" and flag it.
4. **Record contradictions, do not resolve them.** If two sources disagree, keep both versions with their citations and flag the conflict for the BA. Do not pick a winner.
5. **Cite precisely.** Every statement carries a traceable inline reference: `(S3 §2.1)`, `(S2 WBS R41)`, `(S4 transcript 00:14:22)`. Decisions taken after the last numbered source are cited by person, venue, and date.
6. **Preserve domain terms exactly** as a source spells them. Once a term appears in a source, do not translate or rename it.

## Procedure

### 1. Create the skeleton

On the first source, instantiate the full §1–§11 skeleton below. Section names are fixed; every section starts empty and is populated only as a source supports it. Do not pre-fill a section with what the product "probably" has.

### 2. Per-source loop

After each source the BA provides:

1. Update the file incrementally — add new information into the right sections, refine prior statements this source clarifies, record contradictions, add the source to §10.1.
2. Deliver the full updated `product_context.md` as a file.
3. Give a short changelog in chat, five to ten lines: which sections changed, which rules, entities, roles, or constraints were added, which contradictions were recorded, which open questions were opened or closed.
4. Wait for the next source. Do not rebuild the document from scratch to show progress — updates are targeted and traceable.

### 3. Finalize

When the BA asks to finalize: verify the document is internally consistent, every section is populated where sources allow, the permission matrix legend is defined, and every `TBD` has a matching open question.

## Where open questions live

There is no separate open-questions section. An unresolved point is recorded **inline, where the decision would sit**, in one of three forms:

- `TBD` — the behaviour is decided but a value or copy is missing.
- `proposed — confirm` — a working answer is in place, awaiting client confirmation.
- "not defined in sources — tracked as an open question" — the rule itself is unknown.

This keeps the question next to the decision it blocks. Projects that run a separate open-questions register still keep the inline marker, so the document is readable on its own.

## Document skeleton

Instantiate exactly this structure. The slots stay empty until a source fills them.

```markdown
# Product Context — <project name from sources>
_Last updated: <date>_

## 1. Product overview
Two or three paragraphs: what the product is, for whom, the problem it replaces,
scale, launch target. All facts come from sources.

## 2. Users and roles
Subsections only where sources support them:
- 2.1 Role hierarchy — roles as sources name them, each with a brief description,
  plus role assignment rules.
- 2.2 User attributes — what is tracked on the user record.
- 2.3 User states and transition rules — states a user can be in, what triggers
  transitions, and what happens to their data.

## 3. Domain model
Subsections emerge from the product's own domain language, not from a fixed list
and not from the WBS epic structure. For each entity: attributes, states and
transitions, and the rules that govern them.

## 4. Functional domains
One subsection per functional area; names emerge from sources. For each:
capabilities offered, which roles can perform them, business rules and
constraints, and what is MVP versus a later phase.

## 5. System rules and automations
Rules that run without a role triggering them: time-based transitions, scheduled
jobs, mandatory validations, data-preservation rules, cascade effects.

## 6. Integrations
External systems the product integrates with. For each: purpose, direction
(inbound / outbound / bidirectional), constraints. What integrates and why — not
the tech stack.

## 7. Out of scope (MVP)
What is explicitly excluded, each with the source that excluded it.

## 8. Parallel tracks
Initiatives running alongside the MVP without blocking it. Status per source.

## 9. Glossary
Domain terms as sources spell them. Each term: definition plus source reference.

## 10. Source map
- 10.1 Sources processed — ID, name, date, type, short description.
- 10.2 Traceability — which section draws on which sources, one row per section.

## 11. Permission matrix
One consolidated table of every permission-bearing capability across all roles.
The only place permissions appear in tabular form — §4 describes capabilities in
prose and does not repeat the table.
Columns: Domain (matches §4) · Capability · one column per role · Source(s) ·
Confidence (Confirmed / Partially Confirmed / Open) · Notes.
Cell values: ✅ / ❌ / ✅ scoped / ❓ — define the legend once roles are known.
```

## Writing the decision, and its trace

A Product Context holds the **current decided state**, and enough of the decision's trace that a reader can tell a settled rule from a fresh one.

- Write the rule as it stands now, not as a chronology.
- Where a decision replaced an earlier one, say so in one clause: "Supersedes the earlier position that …". This is what stops a reader from re-opening a closed question.
- Where a rule is phase-dependent, mark the phase inline (`MVP`, `Future Phase`) rather than keeping two parallel descriptions.

## What not to do

- Do not write user stories or acceptance criteria. That is downstream work.
- Do not structure §3 or §4 by WBS epics or by UI screens. Structure comes from the product's own domain language.
- Do not split sub-artifacts into separate files — the permission matrix and the glossary live inside `product_context.md`.
- Do not rebuild the document from scratch on update. Increment only.
- Do not pre-fill any section with assumptions.

## Style

Direct, no preamble. Markdown. Bullets are full sentences. Tables where they add clarity, especially §10.2 and §11.

## Output

Save as `product_context.md`. Deliver the full file after each source, plus the in-chat changelog.
