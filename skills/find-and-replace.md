---
name: find-and-replace
description: "Produce targeted OLD→NEW find/replace edits that bring an existing markdown file to its current decided state — product_context.md, project/tech context, stakeholders, a spec, or any .md the BA maintains. Each edit is delivered as a labelled OLD block and a separate NEW block, so the exact text can be copied one at a time and applied in the editor. Trigger ONLY when the BA explicitly asks for find/replace / edits. Do NOT trigger merely because a new source arrived — analysis comes first; edits are a separate, explicitly-requested commit step. When the target is the Product Context, an extra PC layer applies (source triage, precedence, inline citations, decision-only). Never regenerates the whole file; for building a Product Context from scratch use pc-from-zero."
---

# find-and-replace

Produce targeted OLD→NEW edits that bring an existing markdown file to its current decided state, without regenerating it. The BA applies them by find/replace in an editor (e.g. VS Code) and re-uploads the file. Works on any `.md` file the BA maintains. When the file is the Product Context, additional rules apply on top (see **PC layer**).

## Trigger gate — read first

Only produce edits when the BA explicitly requests find/replace (or an equivalent commit instruction — "make the edits", "update the file"). If a source merely arrived and the BA hasn't asked for edits, don't output OLD→NEW blocks — at that point the source is for analysis and discussion, not editing.

This gate exists because editing is a deliberate commit step, separate from thinking through a source. Settle the decision first, then explicitly ask for the edits once it's final. When unsure whether the BA wants edits yet, ask — don't assume.

## Inputs

- The current file — read it in full first. Never edit from memory; locate the exact current text for every change.
- One or more new sources or stated changes — transcript, decisions, client answers, a correction, anything that changes what the file should say.

## Process

### 1. Read the file and the changes in full

Read the target file and every change in full. For each change, find the exact text in the file that must be replaced. Aim for complete coverage — every change is located, nothing edited blind.

### 2. Produce the edits — separate OLD and NEW blocks

For each change, output the label, then the OLD text in its own code block, then the NEW text in its own code block:

**Edit N — [file / section]**

OLD:
```
[exact current text, verbatim — enough surrounding context to be a unique single find/replace]
```

NEW:
```
[exact replacement text]
```

- OLD and NEW each sit in their **own** code block, so each can be copied independently with one click. The label stays **outside** the block — the block contains only the verbatim text, so a copy is exactly what goes into find/replace.
- Point-edits by default; full-section replacement only when more than roughly half a section changes.
- Write the *final decided state* — never "previously X, now Y" inside the file.

## PC layer — additional rules when the target is the Product Context

When the file is the Product Context (`product_context.md`), apply these on top of the universal process:

- **Triage every change into one category:**

| Category | Meaning | Action |
|---|---|---|
| **Reversal** | Source decides something the Product Context already states *differently* | Replace the old statement. Never leave both versions. Most dangerous — check first. |
| **New** | Source adds something the Product Context is silent on | Add into the right section, with citation. |
| **Conflict** | Source contradicts the Product Context and no authority resolves it | Don't pick a winner or invent an edit. Stop and resolve with the BA in-pass. |
| **Already covered** | Source restates what the Product Context already has | No edit; treat as confirmed. |

- **Precedence when claims clash:** an explicit client decision in a new source supersedes an older Product Context statement (→ Reversal). A scope baseline (WBS, Scope & Vision) takes priority over conflicting prose. Two equal-authority sources that disagree with no resolution → Conflict, resolve in-pass.
- **Conflicts stop the pass** at the point they arise — clarify, ask the BA which way to go, wait for the decision, then continue from where it stopped. Don't defer to the end or emit edits past it.
- **Inline citations** — every new or changed statement carries its source: `(Sx HH:MM:SS)` / `(Sx §n)` / `(Sx WBS Rn)`, or `(Person, venue, date)` for a decision taken after the last numbered source.
- **Decision only, no history** — the Product Context holds the fixed decision; chronology goes elsewhere, never into the document.
- **Tag each edit block with its category** so the BA sees what they're applying.

## When to use vs pc-from-zero

- **find-and-replace (this skill):** the file already exists; edit it as targeted deltas.
- **pc-from-zero:** no Product Context file yet; build the §1–§11 skeleton and first draft.

## What NOT to do

- Don't produce edits before the BA explicitly asks for find/replace.
- Don't rewrite or re-emit the whole file — only targeted OLD→NEW blocks.
- Don't merge OLD and NEW into one block, and don't put the `OLD:` / `NEW:` label inside the block — each block holds only the verbatim text, for clean copying.
- For **acceptance criteria**, route content changes through `ears-ac` (house style), not raw find/replace — this skill is for context/prose files and mechanical edits.
- (PC) Don't silently resolve a real conflict — stop and ask in-pass.
- (PC) Don't write history or chronology into the Product Context — it holds the fixed decision only.
- Use canonical role names; never translate client glossary terms.

## Example (abridged)

Target: `stakeholders.md` in this repository, after Petra Illes confirms she also covers Warsaw until rollout — which closes the open question already sitting at the bottom of that file.

```
**Edit 1 — Meridian Group (Client) table, Petra Illes row**

OLD:
| Petra Illes | Office Manager, Berlin (pilot office) | email | CET | L/H | Represents the Office Manager role; validates flows and mockups. **Source of the manual no-show rule** — pushed back on fully automatic marking during the 2026-07-30 flow review. Covers operational questions in Milo's absence; does not approve scope. |

NEW:
| Petra Illes | Office Manager, Berlin and, until rollout, Warsaw | email | CET | L/H | Represents the Office Manager role; validates flows and mockups. **Source of the manual no-show rule** — pushed back on fully automatic marking during the 2026-07-30 flow review. Covers operational questions in Milo's absence; does not approve scope. |

**Edit 2 — Open Questions, delete the now-answered item**

OLD:
- [ ] Petra Illes — confirm whether she represents the Warsaw and London Office Managers until rollout, or Berlin only. Affects who validates flows for the other two offices.

NEW: (empty — the line is deleted; London stays open if it comes up separately)
```

## Output

Per edit: a label, an `OLD:` code block, and a separate `NEW:` code block — ready to copy one at a time and apply in the editor. No file regeneration. For the Product Context, blocks are category-tagged. After the BA applies them and re-uploads the file, verify the edits landed if asked.
