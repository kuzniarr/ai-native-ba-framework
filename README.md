# AI-native BA framework

A working setup for a business analyst on a delivery project: the product state lives in one file, requirements are derived from it, and repeatable BA activities run as skills instead of pasted prompts.

This repository holds the pieces — context files, a system prompt, five skills, and a worked example — so you can take the parts that fit your project rather than adopt the whole thing.

Written up in detail here: `<link to the article>`.

---

## What is in here

| Folder | What it holds |
|---|---|
| [`prompts/`](prompts/) | The system prompt — project-level rules, modes, the approval gate, anti-hallucination rules. |
| [`skills/`](skills/) | Five skills: building and updating the Product Context, decomposing an epic, writing AC, reviewing AC. |
| [`examples/project-knowledge/`](examples/project-knowledge/) | The four context files, filled in for a fictional product. |
| [`examples/wbs/`](examples/wbs/) | The scope baseline the decomposition skill reads, as a spreadsheet and as readable markdown. |
| [`docs/`](docs/) | Step-by-step setup for a new project and for one already under way. |

## Start here

**On a new project** — [`docs/setup-new-project.md`](docs/setup-new-project.md):

1. Create a project workspace in Claude, ChatGPT Projects, or Gemini Gems.
2. Paste [`prompts/system-prompt.md`](prompts/system-prompt.md) into the project instructions and replace the four marked sections.
3. Add `pc-from-zero` and `decompose-wbs-epic` from [`skills/`](skills/); add the rest as you reach them.
4. Connect your tracker, wiki, and design tool, keeping writes behind the approval gate.
5. Run `pc-from-zero` on your first source, then feed the rest one at a time.

**On a project already running** — [`docs/setup-existing-project.md`](docs/setup-existing-project.md). Same skeleton, different first move: you rebuild the current state from specs, tickets, and the running system, then verify it against reality before trusting it.

## The skills

| Skill | Does | Reads | Returns |
|---|---|---|---|
| [`pc-from-zero`](skills/pc-from-zero.md) | Builds the Product Context from scratch, source by source | Your raw sources | The full file plus a changelog per source |
| [`pc-update`](skills/pc-update.md) | Folds a new source into an existing Product Context | The current file and the new source | Find/replace edits you apply yourself |
| [`decompose-wbs-epic`](skills/decompose-wbs-epic.md) | Splits one WBS epic into vertical stories | The WBS and the Product Context | Stories grouped by parent, with split flags and gaps |
| [`ears-ac`](skills/ears-ac.md) | Writes acceptance criteria in EARS for one story | The story and the Product Context | Criteria, a data dictionary, open questions |
| [`ac-validation`](skills/ac-validation.md) | Reviews acceptance criteria for coverage and quality | A story with its AC | Findings only — it never rewrites |

None of them decides anything about the product. Each one prepares a decision to the point where you accept it or reject it, and every change to the Product Context goes through you.

## The example product

Every example in this repository describes **a fictional internal room-booking tool** for an invented company. The client, the people, the decisions, and the open questions are made up. Nothing here comes from a real project.

It is deliberately small but not trivial: three roles with different permissions, a booking state machine, recurring bookings, an automated no-show rule, and two places where the scope baseline disagrees with the Product Context — because that is what a real backlog looks like, and because the decomposition skill is supposed to catch exactly that.

Start with [`examples/project-knowledge/product_context.md`](examples/project-knowledge/product_context.md). Everything else in the example derives from it.

## Adapting it

The skills carry a house style — `must` rather than `shall`, criteria numbered continuously, `TBD` as a code chip, one design link per story. Those are conventions, not requirements. Change them to match how your team writes, and change them in the skill rather than in each output.

What is worth keeping as-is: the separation between what the AI proposes and what you approve, the rule that every statement carries a source, and the habit of marking an assumption as an assumption.

## Licence

MIT. Use, adapt, and redistribute freely.
