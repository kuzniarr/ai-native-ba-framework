---
name: pc-update
description: "Produce OLD→NEW find/replace edits that bring an existing `product_context.md` to its current decided state from one or more new sources. Trigger only when the BA explicitly asks for edits — not merely because a new source arrived. Analysis and discussion of a source come first; editing is a separate, explicitly requested commit step. Updates the Product Context only; does not touch trackers, changelogs, or stories. To build a Product Context from scratch, use pc-from-zero."
---

# Product Context — update

Produce targeted OLD→NEW edits that bring an existing `product_context.md` to its current decided state. The BA applies them by find/replace in an editor and re-uploads the updated file.

## Trigger gate — read first

Only produce edits when the BA explicitly asks for them. If a source merely arrived and no edit was requested, do not output OLD→NEW blocks — at that point the source is for analysis and discussion.

This gate exists because editing is a deliberate commit step, separate from thinking a source through. The BA settles the decision first, then asks for the edits once it is final. Jumping straight to edits mixes the two and produces edits before the decision is sound. When unsure whether the BA wants edits yet, ask.

## Inputs

- **The current `product_context.md`** — read it in full first. It is large; never edit from memory.
- **One or more new sources**, any type: transcript, meeting decisions, client answers, PRD, an approved feature list. Multiple sources can be processed in one pass. What matters is that each source's full effect on the document is analysed, not how many there are.

## Procedure

### 1. Read both, then cross-check

Read every new source and the current Product Context in full. For each claim a source makes, locate what the document currently says on that exact point. Aim for complete coverage — every source claim checked. An incomplete cross-check is how reversals slip through unnoticed.

### 2. Triage every change into one category

This is the quality core: nothing slips when every item is classified.

| Category | Meaning | Action |
|---|---|---|
| **Reversal** | The source decides something the document already states *differently* | Replace the old statement. Never leave both versions. Most dangerous — look for these first. |
| **New** | The source adds something the document is silent on | Add into the right section, with citation. |
| **Conflict** | The source contradicts the document and no authority resolves it | Do not pick a winner and do not invent an edit. Stop and resolve with the BA in-pass. |
| **Already covered** | The source restates what the document already has | No edit; treat as confirmed. |

### 3. Name the real decision, and resolve conflicts in-pass

For Reversals and Conflicts, state the actual decision at stake in one line — what changed and why it matters — before the edit, so the BA reviews it rather than rubber-stamps it.

**A conflict stops the pass at the point it arises.** Do not defer it to the end and do not keep emitting edits past it. Clarify it, ask the BA which way to go, then continue from where you stopped.

Precedence when claims clash: an explicit client decision in a new source supersedes an older statement, which makes it a Reversal. The scope baseline takes priority over conflicting prose. Two equal-authority sources that disagree with nothing to resolve them is a Conflict.

### 4. Produce the edits

For each Reversal and New item, output an OLD→NEW block:

- **OLD** — the exact current text to find, verbatim, with enough surrounding text to be unique for a single find/replace.
- **NEW** — the exact replacement.

Point edits by default; replace a whole section only when more than roughly half of it changes. Write the final decided state — never "previously X, now Y" inside the document, though a one-clause "Supersedes the earlier position that …" is correct where a reader would otherwise re-open a closed question. Keep an inline citation on every new or changed statement. Tag each block with its category so the BA sees what they are applying.

## What not to do

- Do not produce edits before the BA asks for them.
- Do not rewrite or re-emit the whole file — only targeted OLD→NEW blocks.
- Do not silently resolve a real conflict. Stop and ask.
- Do not write a chronology into the document. It holds the decided state.
- Do not touch changelogs, trackers, stories, or AC. This skill updates the Product Context and nothing else.

## Output

Category-tagged OLD→NEW blocks, ready to apply. No file regeneration. After the BA applies them and re-uploads the file, verify the edits landed if asked.
