---
name: decompose-epic
description: "Decompose an epic or large story into smaller independent vertical user stories using INVEST principles. Use when breaking approved scope into implementation-ready stories."
---

# decompose-epic

## Purpose

Decompose an epic or large story into smaller, independent, valuable user stories — each delivering user or business value. Vertical slices, INVEST-compliant. Story statements only — acceptance criteria are added afterwards by `ears-ac`.

## When to invoke

- After an epic is defined and approved
- Before writing acceptance criteria
- When a large story needs to be broken into deliverable pieces

## Input

The epic, as any of:

- Free text description (pasted directly) — the default when no connector is set up
- A Confluence page URL, if the BA has a connector that can read it
- A Jira epic key, if the BA has a connector that can read it

**Product Context** — read it as a whole before decomposing. A rule relevant to the epic may live in any section, not only the one matching its name.

## Process

Take the epic and decompose it into smaller, independent, valuable user stories.

### Guidelines

- Split **vertically**, not technically. Each story delivers one complete, user-visible piece of value — interface, logic, and data together — rather than one technical layer on its own.
- Each story must represent a **complete, deliverable slice** of functionality.
- Apply **INVEST**: Independent, Negotiable, Valuable, Estimable, Small, Testable.
- Keep stories consistent in form: `As a <role>, I want <action>, so that <benefit>.`
- Group related stories under short functional headings.

### Splitting patterns

Name which pattern(s) were used on each group:

1. **Workflow Steps** — sequential stages of one flow.
2. **CRUD Operations**.
3. **Business Rule Variations** — different rules or branches, often a role-based split.
4. **Data Variations**.
5. **Data Entry Methods** — the same outcome reached through different interfaces.

### Role logic

Resolve the role from the Product Context role list. If the epic text is generic about who acts, use the Product Context to determine the actual role per story — don't leave it as a placeholder.

### Anti-hallucination

- Use only the epic text and the Product Context. Introduce no roles, entities, or rules not present in either.
- Anything unclear: mark it `TBD` and note it under Open Questions rather than guessing.
- Use Product Context glossary terminology only.

## Output format

````
# Decomposition — <Epic name>

**<Group heading>**
Pattern: <pattern(s)>
1. As a <role>, I want <action>, so that <benefit>.
2. As a <role>, I want <action>, so that <benefit>.

**<Group heading>**
Pattern: <pattern(s)>
1. As a <role>, I want <action>, so that <benefit>.

## Rationale
1–2 sentences: how the chosen split keeps each story a vertical, valuable slice.

## Open Questions
- <uncertainty, missing context, or assumption — for BA review>
````

## Example (abridged)

From the Product Context in this repository, epic **Booking Management**:

```
**Creating and finding a booking**
Pattern: Workflow Steps + CRUD (Create)
1. As an Employee, I want to search for rooms available in a time slot, so that I can pick one that fits my meeting.
2. As an Employee, I want to book an available room for a time slot, so that I have a guaranteed space.

**Managing an existing booking**
Pattern: CRUD + Business Rule Variations
3. As an Employee, I want to move my own booking before it starts, so that I don't have to cancel and rebook.
4. As an Employee, I want to cancel my own booking before it starts, so that the room is freed for others.
5. As an Office Manager, I want to cancel any booking in my office, so that I can resolve conflicts.

## Rationale
Creation and search are split from management because they're reached at different points in the flow and have different actors for the cancel case (Employee vs Office Manager).

## Open Questions
- When moving a single occurrence detaches it from its recurring series (§3.4), does the notice to the holder say so specifically, or is it the same generic move confirmation used elsewhere (§4.5)? Not stated in the Product Context.
```

## Output destination

If the epic source is in Confluence and a connector is available, ask the BA: publish stories as a child page under the epic, or return as markdown for manual handling? Otherwise, return the full output as markdown.

## What this skill does NOT do

- Does not write Acceptance Criteria — use `ears-ac` after stories are approved.
- Does not create Jira tickets or split by implementation area (BE/FE/QA) — that's downstream work this repository doesn't cover.
