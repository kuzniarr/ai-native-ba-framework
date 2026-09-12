# Setting up on a new project

A new project is the easier of the two starting points: the Product Context grows with the project, so every statement in it has a source from day one.

Budget roughly half a day for the setup, then the first real source.

---

## 1. Create the project workspace

Create a project in Claude (or a ChatGPT Project, or a Gemini Gem — the structure carries over). Everything below lives inside it, so that every chat you open starts with the same context.

## 2. Paste the system prompt

Copy [`prompts/system-prompt.md`](../prompts/system-prompt.md) into the project instructions.

Replace the four marked sections — Role, Project context, Scope structure, and the epic flow under Modes — with your own. Leave the rest: style, behaviour, the approval gate, anti-hallucination, and the mode mechanics are not project-specific.

If you do not yet know your epic list or your team, put `TBD` and come back after kick-off. An honest `TBD` is better than an invented epic list that then leaks into requirements.

## 3. Add the skills

Copy the files from [`skills/`](../skills/) into your project's skills. Start with two:

- `pc-from-zero` — you will need it for the next step.
- `decompose-wbs-epic` — the first thing you will run once scope arrives.

Add `ears-ac`, `ac-validation`, and `pc-update` when you reach them. Installing all five on day one means five untested skills; installing them as you hit the activity means each one gets exercised while you still remember what you wanted from it.

## 4. Connect what you have

Connect the tools you already work in — issue tracker, wiki, drive, design tool. Without a connector every operation is an export, a paste into chat, and a manual copy back.

Two rules from the start:

- Keep writes behind the approval gate that is already in the system prompt. Reads can run freely.
- Check what you are about to upload. The approval frame you agreed with the client covers *whether* you use AI; it does not cover *what* you put into it.

## 5. Build the Product Context from the first source

Open a chat, call `pc-from-zero`, and give it your first source — usually the Scope and Vision document or the WBS.

Then keep going, one source at a time:

```
source 1 — Scope & Vision
source 2 — WBS
source 3 — kick-off transcript
source 4 — elicitation #1
```

Cluster sources by when they arrived and feed them in that order. This matters: a later source refines an earlier one, and feeding them out of order makes the newer statement look like the contradiction.

After each source the skill returns the full file plus a short changelog of what changed. Read the changelog. It is the cheapest review you will get.

Save the result as `product_context.md` in your project knowledge. Compare it against [`examples/project-knowledge/product_context.md`](../examples/project-knowledge/product_context.md) — not to copy the content, but to see how dense a usable one is and where the citations sit.

## 6. Add the three supporting files

`project_context.md`, `tech_context.md`, and `stakeholders.md` do not need a skill — write them once from what you already know, using the examples as the shape:

- [`project_context.md`](../examples/project-knowledge/project_context.md) — delivery constraints, risks, identifiers, the source map, conflict resolution rules, conventions, Definition of Ready. Write the Definition of Ready even if it feels premature; it is what you point at when a story arrives half-finished.
- [`tech_context.md`](../examples/project-knowledge/tech_context.md) — stack, integrations, architecture decisions. Get the tech lead to read it once.
- [`stakeholders.md`](../examples/project-knowledge/stakeholders.md) — who decides what, and who covers for whom.

## 7. Run one epic end to end

Do not roll the framework out across the whole backlog. Take one epic:

1. `decompose-wbs-epic` on that epic → review the stories, the split flags, and the gaps.
2. `ears-ac` on one story → check that the criteria match what the Product Context actually says.
3. Fix what annoyed you in the skill, not in the output. The output is one epic; the skill is every epic after it.

That third step is the whole point. The first run of a skill is a draft of the skill.

---

## What good looks like after two weeks

- Every product question you get is answered from the Product Context, not from memory.
- A statement in the Product Context can be traced to a source in under a minute.
- You have edited at least two skills after using them.
- Someone other than you has opened the Product Context and found a mistake in it.

That last one is the real signal. A context file nobody checks is a context file nobody trusts.
