# AI-native BA framework

A working setup for a business analyst on a delivery project: the product state lives in one file, requirements are derived from it, and repeatable BA activities run as skills instead of pasted prompts.

Companion repository to the article. It holds a starter set — context files, a system prompt, five skills, and a worked example — not the full system: the article walks through about ten BA activities, five of them ship here as skills, the rest are built the same way on your own process.

An article walking through the framework in detail is on the way; the link will land here when it is published.

---

## What is in here

| Folder | What it holds |
|---|---|
| [`prompts/`](prompts/) | The system prompt — project-level rules, modes, the approval gate, anti-hallucination rules. |
| [`skills/`](skills/) | Five skills: building and updating the Product Context, routing an incoming item, decomposing an epic, writing AC. |
| [`examples/project-knowledge/`](examples/project-knowledge/) | The four context files, filled in for a fictional product. |
| [`docs/`](docs/) | Step-by-step setup for a new project and for one already under way. |

## Start here

**On a new project** — [`docs/setup-new-project.md`](docs/setup-new-project.md):

1. Create a project workspace in Claude, ChatGPT Projects, or Gemini Gems.
2. Paste [`prompts/system-prompt.md`](prompts/system-prompt.md) into the project instructions and replace the four marked sections.
3. Add `pc-from-zero` and `decompose-epic` from [`skills/`](skills/); add the rest as you reach them.
4. Connect your tracker, wiki, and design tool, keeping writes behind the approval gate.
5. Run `pc-from-zero` on your first source, then feed the rest one at a time.

**On a project already running** — [`docs/setup-existing-project.md`](docs/setup-existing-project.md). Same skeleton, different first move: synthesize the current state from tickets or specs before feeding it in, then verify high-level against reality before trusting it.

## The skills

| Skill | Does | Reads | Returns |
|---|---|---|---|
| [`pc-from-zero`](skills/pc-from-zero.md) | Builds the Product Context from scratch, source by source | Your raw sources | The full file plus a changelog per source |
| [`find-and-replace`](skills/find-and-replace.md) | Folds a decided change into an existing markdown file — the Product Context or any other | The current file and the new source | Find/replace edits you apply yourself |
| [`analyze-incoming`](skills/analyze-incoming.md) | Explains an incoming item and routes it to the next action | A comment, message, transcript snippet, or proposed feature | An explanation, a route, and a drafted question where needed |
| [`decompose-epic`](skills/decompose-epic.md) | Splits an epic into vertical user stories | The epic and the Product Context | Stories grouped by theme, with rationale and open questions |
| [`ears-ac`](skills/ears-ac.md) | Writes acceptance criteria in EARS for one story | The story and the Product Context | Criteria, a data dictionary, open questions |

None of them decides anything about the product. Each one prepares a decision to the point where you accept it or reject it, and every change to the Product Context goes through you.

## The example product

Every example in this repository describes **a fictional internal room-booking tool** for an invented company. The client, the people, the decisions, and the open questions are made up. Nothing here comes from a real project.

It is deliberately small but not trivial: three roles with different permissions, a booking state machine, recurring bookings, and an automated no-show rule — enough surface for the skills to have real material to work with.

Start with [`examples/project-knowledge/product_context.md`](examples/project-knowledge/product_context.md). Everything else in the example derives from it.

## Adapting it

The skills carry a house style — `must` rather than `shall`, criteria numbered continuously, `TBD` as a code chip, one design link per story. Those are conventions, not requirements. Change them to match how your team writes, and change them in the skill rather than in each output.

What is worth keeping as-is: the separation between what the AI proposes and what you approve, the rule that every statement carries a source, and the habit of marking an assumption as an assumption.

## License

MIT. Use, adapt, and redistribute freely.
