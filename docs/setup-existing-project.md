# Setting up on a project already under way

Harder than a new project, for one reason: the decisions were made before you started collecting them. You won't recover the history — you'll recover the current state.

That's enough to be useful. A context that says what the product does today, without saying why, still answers most of the questions you get asked.

---

## 1. Pick your authority

Before gathering anything, decide: **what's the source of truth for how the product behaves today?**

Usually one of:

- **Tickets** — the backlog is where decisions actually landed, specs lag behind or never existed at scope level.
- **Specifications** — the team works spec-first and ships what's written.
- **The running system** — if neither of the above holds, the code and the behaviour are the truth.

Pick one and say so out loud — this decides how you resolve every conflict later, so it's cheaper to name now than mid-cleanup. On most delivery projects it's tickets: specs describe intent, tickets describe what actually got built and often outlive the spec that spawned them.

## 2. Filter for what's still true

Don't gather everything — gather what's current.

A ticket's age isn't the filter. A recently closed ticket for a feature that got reworked twice is stale; a two-year-old ticket for a rule nobody's touched since is still accurate. What you're filtering for is: **does this still describe how the product behaves right now?** Drop what a later ticket superseded, what got reversed, what shipped and was then redesigned.

You know your product's history — this step is faster for you than it looks from outside.

## 3. Synthesize before you feed anything to pc-from-zero

This is the step that makes brownfield work, and it's not in the skill's default flow.

Don't hand `pc-from-zero` a pile of raw tickets. Have Claude read them first and produce a synthesis — the current state, in plain language, per feature area or per epic. Ask directly: *"Here are the tickets for [area]. Summarize the current behaviour these describe — not a changelog, the state as it stands now."*

Feed **that synthesis** into `pc-from-zero`, not the raw tickets. Two things follow from this:

- **Source order stops mattering.** When you feed dated transcripts one at a time, order matters because a later one can override an earlier one. A synthesis has already resolved that — there's nothing left to sequence.
- **Contradictions mostly don't reach the skill.** Summarizing forces a decision on anything two tickets disagree about. What's left for `pc-from-zero` to flag is what the synthesis itself couldn't resolve — genuinely unclear.

You can feed the whole synthesis in one pass if the product is small enough to summarize coherently. On a larger one, do it by feature area, so each synthesis stays something you can actually review before it goes in.

## 4. Verify — high-level, not exhaustively

You won't verify every line, and you don't need to. Go high-level:

- Ask the dev team to sanity-check the areas that feel least certain, rather than routing every rule through them — their time is the actual bottleneck here, not yours.
- Where you have code access, a tool like Copilot or Cursor can check a stated rule against the implementation directly — often faster than asking a person, and a reasonable first pass before you spend someone else's time.
- What's left unverified stays visibly unverified. Don't upgrade an assumption to a confirmed fact because verifying it felt like too much work.

## 5. Fill the gaps forward

Don't try to reconstruct missing history — you don't have it, and a plausible-sounding invented rationale is worse than an honest gap. Where the context is silent, leave it as an open question and resolve it at the next elicitation, refinement, or whenever it actually blocks a story.

## 6. Prove it on the next epic

Run `decompose-epic` against the context you just built, on whatever epic is next in the backlog.

Watch the **open questions**, not the decomposition itself: a flow the context doesn't cover, or a story whose rules the context doesn't support. That gap is the actual distance between your file and the product. Fix the file, run it again.

---

## Two things that go wrong here

**Backfilling the "why."** Tempting to write a plausible reason for a decision nobody remembers. Don't — a wrong rationale gets quoted back at you in a refinement, and by then it reads as fact.

**Treating this as equivalent to a new project.** It isn't. There's no decision history here and there won't be one — that's the real cost of starting mid-project, not something this process fixes. What it gets you is an accurate *current state*, which is most of what you need day to day.
