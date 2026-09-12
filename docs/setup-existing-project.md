# Setting up on a project already under way

Harder than a new project, for one reason: the decisions were made before you started collecting them. You will not recover the history — you will recover the current state.

That is enough to be useful. A context that says what the product does today, without saying why, still answers most of the questions you get asked.

---

## 1. Decide what "current state" means here

Before gathering anything, answer one question: **what is the authority on how the product behaves today?**

Usually one of these:

- **The running system** — if it is live, the code and the behaviour are the truth, and the documents are a description of it that may have drifted.
- **The specifications** — if the team works spec-first and ships what is written.
- **The tickets** — if neither of the above holds and the backlog is where decisions actually landed.

Pick one. When two sources disagree later, this is the tiebreaker, and deciding it now is much easier than deciding it in the middle of a conflict.

## 2. Gather what exists

Collect, in rough order of usefulness:

- specifications, wiki pages, requirement documents;
- the WBS or whatever holds the scope baseline;
- closed tickets for the features already shipped;
- transcripts, if anyone kept them;
- the design files.

Do not clean any of it up first. Contradictions between these documents are information — they tell you where the product drifted, and the skill is built to surface them rather than to smooth them over.

## 3. Build the baseline

Run `pc-from-zero`, feeding sources in the order they were written, oldest first. Same rule as on a new project: a later source refines an earlier one, and reversing the order makes the newer decision look like the contradiction.

Two things to expect, and neither is a problem:

**Citations will be thin.** Where a new project cites a transcript with a timestamp, you will cite "spec page X" or "ticket ABC-123". That is a weaker trace, and it is honest. Do not upgrade it to look better.

**Contradictions will be plentiful.** Let them be recorded rather than resolved. You are not yet in a position to decide which version won — that comes next.

## 4. Verify against reality

This is the step that makes the difference, and the one most likely to be skipped.

For every contradiction the build surfaced, and for every rule that matters, check it against whatever you named as the authority in step 1:

- ask the developer who built it;
- read the code path;
- click through the running system.

Mark each verified statement as confirmed. Mark what you could not verify as an assumption — explicitly, using the same inline markers the skill already uses. A file where the verified and the unverified look alike is worse than no file, because it invites trust it has not earned.

Expect this to take longer than the build itself.

## 5. Fill the gaps forward, not backward

Do not try to reconstruct missing history. Where the context is silent on something, write it as an open question and answer it at the next elicitation, the next refinement, or the next time it actually blocks a story.

Over a few sprints the file fills itself, and everything added from that point forward carries a proper source. The gaps that never get filled were, by definition, never load-bearing.

## 6. Prove it on one epic before trusting it

Take the next epic in the backlog and run `decompose-wbs-epic` against the context you just built.

What you are watching for is not the decomposition. It is the **gaps section**: a flow in the context that no story covers, or a story whose rules the context does not support. On a live project those gaps are the measure of how far your file is from the product.

Fix the file. Then run it again.

---

## Two things that go wrong here

**Backfilling the "why".** It is tempting to write a plausible reason for a decision nobody remembers. Do not. A wrong rationale is worse than a missing one, because it will be quoted back at you in a refinement.

**Declaring it done.** On a new project the Product Context is complete when sources are exhausted. Here it is complete when it stops surprising you — when a week passes without someone pointing at a rule the file does not have. Give it a month.
