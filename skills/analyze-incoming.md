---
name: analyze-incoming
description: "Explain an incoming item and route it to the next action. Use when the BA pastes a client change-log comment, a client message, a transcript snippet, a PRD section, or a newly proposed feature and asks what it means and what to do. Cross-checks the item against the current Product Context, states the real decision at stake, and routes it (apply directly · ask the client · bring to call · no action · or whatever fits). When the route is a client question, it drafts that one question. Does NOT edit the Product Context or write the changelog itself — those are separate, explicitly-requested steps handled by find-and-replace and a changelog skill, if you have one."
---

# analyze-incoming — explain & route

Take one incoming item and do two things: explain what it actually is and the real decision behind it, then route it to the next action. This is the front-door reasoning step that *precedes* any commit to the Product Context or changelog — it grounds the item, makes sense of it, and decides where it goes.

## What this skill is and is not

- **Is:** the analysis and routing step. Ground the item against the project, explain the decision, decide the next action.
- **Is not:** the Product Context commit step (→ `find-and-replace`) or the changelog write step. It does not edit the Product Context or the changelog. It *may* draft a single client question when that is the route, because that's a small inline action — assembling a full prioritized question set for a session is a separate, heavier elicitation-prep activity.

## Inputs

One or more incoming items, any type: client change-log comment, client Slack/board message, transcript snippet, PRD section, newly proposed feature. Plus read access to the current Product Context.

## Procedure

### 1. Ground the item

Targeted cross-check against the current Product Context, read as a whole: what does the project already say on this exact point? Is this genuinely new, already covered, a change to something decided, or a contradiction? Don't analyze in a vacuum — a wrong read here sends the item down the wrong route.

### 2. Explain the item and the decision behind it

State what the item actually is and the real problem or decision at stake — clearly and directly, in normal working style. Don't just restate the item back verbatim; surface the meaning the BA needs to act on. Keep it as long as the item requires and no longer.

### 3. Route to the next action

Classify the item and say it crisply as "this is X → do Y". The routes below are the common ones; they aren't a closed set — if a different action fits the item better, use it and name it.

| Route | When | Hand-off |
|---|---|---|
| **Apply directly** | A decision already exists, or it's a pure narrowing / cleanup — no client input needed | Name the commit target: Product Context update · changelog · board comment · requirement/AC edit. Also say whether it can be applied as-is or should still be confirmed with the client (the recurring "apply now vs bring to call" call). The commit itself happens via the matching skill on the BA's command. |
| **Ask the client** | The item raises something undecided or ambiguous; the client must rule | Draft the question itself — short, addressed to the right person per the project's Stakeholders file. Flag it if it carries scope risk. |
| **Bring to call** | Needs live discussion, multiple parties, or trade-offs | Note what to put on the agenda and why. |
| **No action** | Already covered, out of scope, or a note that only needs acknowledging | Say which, and that nothing changes. |

An item can carry more than one route (e.g. apply part directly, ask the client about the rest) — split it and route each part.

### 4. Stop at the route

End at the routing decision (plus the drafted question where the route is "ask the client"). Don't emit find/replace blocks and don't write changelog entries. If the BA then explicitly asks for edits, that hands off to `find-and-replace`.

## Anti-hallucination

- Don't invent project facts — cross-check against the Product Context, or mark "not defined in sources, needs confirming."
- Don't pick a winner on a real conflict — route it to the client and draft that question.

## What NOT to do

- Don't edit the Product Context or write the changelog from this skill — analyze and route only. (Drafting a single client question is allowed; producing Product Context edits or changelog entries is not.)
- Don't restate the item verbatim instead of explaining it — lead with the decision at stake.

## Example (abridged)

Item: *"Client Slack message — 'can we also let people book a room for someone else, like an assistant booking for their manager?'"*

```
This is a new capability, not covered by the Product Context. §4.1 has the
holder always be the Employee who creates the booking — booking on behalf of
someone else isn't modeled at all: no delegate concept, no "book for" field.

This is a scope question, not a narrowing — it changes who the holder can be
and likely touches cancellation rights (§4.3) and notifications (§4.5) too.

Route: Ask the client. Scope risk — flag it.

Draft question: "Should an Employee be able to book a room on behalf of
someone else (e.g. an assistant booking for their manager)? If yes: who
becomes the holder — the person who created the booking or the person it's
for — and who can then cancel or move it?"
```

## Output

In-chat only: a clear explanation of the item and the decision behind it, plus the route per item ("this is X → do Y", with apply-now vs confirm-with-client noted). Where the route is "ask the client", include the drafted question. No files, no Product Context or changelog commits.
