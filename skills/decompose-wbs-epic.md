---
name: decompose-wbs-epic
description: "Decompose a WBS epic into smaller, independent, vertical user stories (INVEST), each written as a full 'As a role, I want, so that' statement and grouped under the parent WBS story it came from. Reads the WBS spreadsheet and the Product Context, names the splitting pattern used, and surfaces split decisions and WBS-versus-Product-Context gaps for BA review. Use when breaking a WBS epic into refinement-ready stories, before acceptance criteria."
---

# decompose-wbs-epic

## Purpose

Turn one WBS epic into smaller, valuable **vertical** user stories, each traceable to the parent WBS story it came from. Story statements only — acceptance criteria are added afterwards by `ears-ac`.

## Inputs

The BA names the epic. Read both of these without being asked.

**1. WBS spreadsheet.** Layout used here (adjust to your own sheet, but keep the epic/parent distinction):

- Header on row 1. Columns: `ID` · `Topic` · `User Story` · `Acceptance Criteria` · `Phase`.
- An **epic** is an UPPERCASE `Topic` row with an empty `User Story`.
- The rows after it, until the next epic header, are its **parent stories**. From each take: the `ID`, the short title in `Topic`, the full statement in `User Story`, the raw `Acceptance Criteria`, and `Phase`.
- The raw AC column is a scoping note written before refinement, not final acceptance criteria. Treat it as a seed, not as truth.

Read it with code rather than by eye:

```python
import openpyxl, glob
wb = openpyxl.load_workbook(glob.glob('**/*wbs*.xlsx', recursive=True)[0], data_only=True)
ws = wb['WBS']
# From row 2: an UPPERCASE Topic with an empty User Story is an epic header;
# the rows after it are its parent stories, up to the next header.
```

**2. Product Context.** The authoritative source for content. Read it **as a whole** — a rule relevant to a story may live in any section, not only the one matching the epic's name. Where the Product Context and the raw WBS AC conflict, the Product Context is the resolved state.

## Process

For the named epic, take each parent WBS story in turn:

- If it is already an atomic, INVEST-compliant vertical slice, keep it as **one story** and say so in a line.
- Otherwise decompose it into child stories, each a full statement: `As a <role>, I want <action>, so that <benefit>.`

Group every resulting story under its parent WBS story title — that grouping is the traceability link. Then add **Split flags** and **Gaps & conflicts**.

## Decomposition guidelines

- Split **vertically**. Each story delivers one complete, user-visible piece of value end to end — the interface, the logic behind it, and the data it needs together — rather than one technical layer on its own.
- Apply **INVEST**: Independent, Negotiable, Valuable, Estimable, Small, Testable.
- **Name the splitting pattern** used on each parent. Common ones:
  1. **Workflow Steps** — sequential stages of one flow.
  2. **CRUD Operations**.
  3. **Business Rule Variations** — different rules or branches, often a role-based split.
  4. **Data Variations**.
  5. **Data Entry Methods** — the same outcome reached through different interfaces.
- **Create is not View.** Keep a create action and a list or view action as separate vertical slices, even when they share a screen.
- **Branch complexity is a split smell.** If one `want` pulls in many conditions, exceptions, or role variations, the split is too coarse. Re-split so each story keeps one clean intention. Those conditions become AC downstream; catching them here keeps the AC short.
- **Shared steps become one canonical story.** When the same behaviour recurs across flows in the epic, create one story for it and have the other flows reference it — never duplicate it as a child under each flow.
- When a candidate child is trivial or naturally belongs inside another, do not decide alone — raise it as a **Split flag**.

## Story statement rules

The `want` must be clean. Detail lives in the AC, not in the statement.

- **One `want` is one user intention.** No compound actions joined with "and". Split them, or pick the single real intention.
- **State the genuine goal, in active voice.** Name what the user wants to achieve (`create a booking`), not the mechanical sub-action (`fill in the form`), and never passive system phrasing (`be taken to`, `be routed to`).
- **Keep conditions, optionality, field lists, and interface detail out of the statement.** They are acceptance criteria.
- **Automatic system outcomes are not stories.** An auto-redirect or a default assignment happens without the user acting — it is AC of the triggering story.

## Role logic

WBS stories are often written generically as `As a User`. Resolve the real role from the Product Context role list.

- If behaviour differs by role, split into separate stories per role — that is a Business Rule Variation.
- If behaviour is the same across roles, write one story naming them together: `As an Office Manager or Employee, I want …`.
- **Same action, same actor.** An identical action takes the same role wherever it appears. A different actor for the same action is either a genuine rule variation or an inconsistency to resolve — never a silent mismatch.
- If the role cannot be determined from the WBS or the Product Context, write `TBD` and list it under Gaps.

## Gaps & conflicts

Surface these for BA review rather than resolving them silently.

- **WBS ↔ Product Context divergence** — where a parent's raw WBS AC differs from the Product Context. Flag it **only when it changes a field, a rule, or a scope decision**. Treat a pure rename as silent mapping: use the current term and move on.
- **Dependency or duplicate** — where a child slice duplicates or depends on another parent story in the same epic.
- **Role TBD** — any unresolved role.
- **Missing** — a flow that lives in the Product Context but did not land in any story, and anything the Product Context is silent on.

That last one is the reason this step is worth running on a whole epic rather than story by story: a gap between the context and the backlog is only visible when both are in view at once.

## Anti-hallucination

- Use only the epic's WBS rows and the Product Context. Introduce no roles, entities, flows, or fields that are not in them.
- Anything unknown goes to `TBD` and is listed under Gaps.
- Use Product Context glossary terminology only.

## Output format

````
# Decomposition — <Epic name> (WBS Topic: <TOPIC>)

## <Parent WBS story title> (<ID>)
Pattern: <pattern(s)>
1. As a <role>, I want <action>, so that <benefit>.
2. As a <role>, I want <action>, so that <benefit>.

## <Parent WBS story title> (<ID>)
Already an atomic slice; kept as one story.
As a <role>, I want <action>, so that <benefit>.

## Split flags
- <merge or boundary decisions for the BA>

## Gaps & conflicts
- WBS ↔ PC: <divergence; the Product Context is the resolved state>
- Dependency / duplicate: <…>
- Role TBD: <…>
- Missing: <…>
````

## Example (abridged)

From the example WBS in this repository, epic `BOOKING MANAGEMENT`:

```
## Book a room (R41)
Pattern: Workflow Steps + CRUD (Create)
1. As an Employee, I want to search for rooms available in a time slot, so that I can pick one that fits my meeting.
2. As an Employee, I want to book an available room for a time slot, so that I have a guaranteed space.
3. As an Employee, I want to invite attendees to my booking, so that they know which room to come to.

## Cancel my booking (R43)
Already an atomic slice; kept as one story.
As an Employee, I want to cancel my own booking, so that the room is freed for others.

## Split flags
- Attendee invitation (story 3) could fold into story 2, since attendees are set on the same form. Kept separate because attendees are optional and the capacity warning is its own behaviour.

## Gaps & conflicts
- WBS ↔ PC: R46 states the grace period varies by room size; Product Context §4.4 fixes it globally at 15 minutes, with per-office configurability tracked as an open question. The Product Context is the resolved state.
- WBS ↔ PC: R24 puts a CSV room import in MVP as a product feature; Product Context §1 treats loading room inventory as a one-time cutover activity. Confirm whether an in-product import is in scope.
- Missing: Product Context §4.4 allows an Office Manager to mark a no-show manually before the grace period elapses. No WBS row covers it and no story above carries it.
```

Return the whole thing as markdown.
