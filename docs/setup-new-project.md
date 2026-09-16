# Setting up on a new project

A new project is the easier of the two starting points: the Product Context grows with the project, so every statement in it has a source from day one.

Budget half a day for the setup itself, then the first real source.

---

## 1. Create the project workspace

Create a project in Claude — or a ChatGPT Project, or a Gemini Gem; the structure carries over. Everything below lives inside it, so every chat you open starts with the same context.

**Check:** the project exists and you can open a chat inside it.

## 2. Install the skills

**What to have on hand:** none — the skills live in this repository.

Download this repository: green **Code** button → **Download ZIP** → unzip. The `skills/` folder holds one `.md` file per skill.

In Claude: your avatar (top right) → **Settings** → **Skills** → **+** (top right of the panel) → upload a skill's `.md` file → confirm the name and description → **Add**. Repeat per skill; order doesn't matter.

Start with two:
- `pc-from-zero` — you need it for step 5.
- `decompose-epic` — the first thing you'll run once scope arrives.

Add `ears-ac`, `analyze-incoming`, and `find-and-replace` when you reach them. Installing all five on day one means five untested skills; installing them as you hit the activity means each one gets exercised while you still remember what you wanted from it.

**Check:** open a chat inside the project, type `/` — the skills you added appear in the list.

## 3. Connect what you have

Connectors let Claude read and write your tools directly from chat. Without one, every operation is an export, a paste into chat, and a manual copy back.

Settings → **Connectors** → find your tool (Jira & Confluence, Google Drive, Figma, FigJam, or others) → **Connect** → authorize. Then return to your project and enable the connector for it.

**Check:** in a chat with the connector enabled, ask a read-only question — *"List available Confluence spaces and Jira projects."* Real data should come back. If it doesn't, the connector may be enabled on your account but not on this project — check the project's connector settings, not just your account's.

**Pitfall:** if a connector silently fails, Claude may generate plausible-looking content instead of telling you it couldn't fetch anything. Add to your prompt: *"If you can't access [tool], tell me — don't generate placeholder data."* For anything you'll rely on, spot-check the first couple of results against the real system.

## 4. Set the system prompt

**What to have on hand:** a short description of the project — client, stack, team, what you're building.

Copy [`prompts/system-prompt.md`](../prompts/system-prompt.md).

**By hand:** replace the `<like this>` sections — Role, Project context, Scope structure, and the epic flow under Modes. Leave the rest: style, behaviour, the approval gate, anti-hallucination, and the mode mechanics aren't project-specific.

**With Claude:** paste the template into a chat, add your project description, and ask: *"Fill this template with my project's values. If something is missing to fill a section, ask me for it — don't invent."* Claude fills what it can and asks for the rest.

**Where this goes matters.** Paste the result into your project's **Instructions** (avatar → your project → Instructions) — **not** into Project Knowledge as a file. Instructions are the operating contract, active in every chat; a file with the same content sitting in Knowledge does nothing on its own.

**Check:** start a fresh chat in the project and ask *"what's your role here?"* — the answer should reflect what you just set, not a generic description.

If you don't yet know your epic list or your team, write `TBD` and come back after kick-off. An honest `TBD` is better than an invented epic list that leaks into requirements later.

## 5. Build the Product Context

**What to have on hand:** your first source — usually the Scope & Vision document or the WBS.

Open a chat, call `pc-from-zero`, and give it that first source. Then keep going, one source at a time, roughly in the order they arrived:

```
source 1 — Scope & Vision
source 2 — WBS
source 3 — kick-off transcript
source 4 — elicitation #1
```

After each source the skill returns the full file plus a short changelog of what changed. Read the changelog — it's the cheapest review you'll get.

Save the result as `product_context.md` in your project's Knowledge. Compare it against [`examples/project-knowledge/product_context.md`](../examples/project-knowledge/product_context.md) — not to copy the content, but to see how dense a usable one is and where citations sit.

**Check:** ask the project a product question you already know the answer to — *"what happens when a user is deactivated?"* The answer should cite a section of the file, not guess.

## 6. Add the three supporting files

`project_context.md`, `tech_context.md`, and `stakeholders.md` don't need a skill — write them once from what you already know, using the examples as the shape:

- [`project_context.md`](../examples/project-knowledge/project_context.md) — delivery constraints, risks, identifiers, the source map, conflict resolution rules, conventions, Definition of Ready.
- [`tech_context.md`](../examples/project-knowledge/tech_context.md) — stack, integrations, architecture decisions. Get the tech lead to skim it once.
- [`stakeholders.md`](../examples/project-knowledge/stakeholders.md) — who decides what, and who covers for whom.

**Check:** all four files show up in the project's Knowledge panel.

You don't need all four before you start working — Product Context carries most of the value on its own, the rest fills in as the project does.

## 7. Run the first epic

Don't roll the framework out across the whole backlog. Take one epic and go through it end to end:

1. `decompose-epic` on that epic → review the stories and the open questions. The open questions are the most useful part of the first run — they show you where the Product Context is thinner than the epic assumes.
2. `ears-ac` on one story → check the criteria against what the Product Context actually says, not against what you expected it to say.
3. Whatever annoyed you — fix it in the skill, not in the output. The output is one story; the skill is every story after it.

That third point is the whole exercise. The first run of a skill is a draft of the skill.

The rest of your BA activities — Confluence structure, ticket sync, design validation, elicitation prep — aren't covered by the five skills in this repository. The article walks through how each one is built; the shape is the same as `decompose-epic` and `ears-ac`: describe the problem, show how you'd do it by hand, let Claude extract the pattern, run it a few times, fix what's off.

---

## What good looks like after two weeks

- Every product question you get is answered from the Product Context, not from memory.
- A statement in the Product Context can be traced to a source in under a minute.
- You've edited at least one skill after using it.
- Someone other than you has opened the Product Context and found a mistake in it.

That last one is the real signal. A context file nobody checks is a context file nobody trusts.
