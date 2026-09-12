---
name: ac-validation
description: "Review Acceptance Criteria for one or more user stories against completeness and quality standards. Checks happy path, alternative flows, edge cases, error states, and criterion quality, and returns findings only — never rewrites the AC. Use before refinement, or on AC inherited from someone else."
---

# ac-validation

## What it does

Reviews the Acceptance Criteria of a story, or a set of stories, against completeness and quality standards. Returns a structured list of findings so the BA decides what to fix. **It does not rewrite the AC.**

That separation is the point. A skill that both judges and rewrites gives you no way to tell which of its changes you actually agreed with.

## Input

- A user story with its AC — pasted, or fetched from wherever requirements live.
- Optional scope note: "check coverage only", "focus on edge cases".

If the story statement is missing or vague, say so before checking anything else. AC cannot be validated against a story that does not state who wants what.

## Process

### Step 1 — read the story and its AC

Extract the story statement (who, what, why) and the full list of criteria. Where a Product Context is available, read it too: a criterion can be complete in form and still contradict a decided rule.

### Step 2 — check coverage

| Coverage area | Check |
|---|---|
| Happy path | The main successful flow is described end to end, including what happens next |
| Alternative flows | Each branch of a splitting trigger is covered |
| Negative and error states | Invalid input, rejected actions, boundary violations |
| Edge cases | Empty states, maximum values, an action already completed, back-navigation |
| Role variations | Where behaviour branches by role, each branch is covered |

Mark each area ✅ covered, ⚠️ partial, or ❌ missing. Where an area genuinely does not apply, mark it N/A rather than ❌.

### Step 3 — check quality

| Criterion | Description |
|---|---|
| **Atomic** | One requirement per item, not several bundled with "and" or "or" |
| **Testable** | Can be verified with a clear pass or fail outcome |
| **Unambiguous** | No "should", "may", "appropriately", "fast" without a measurable value |
| **Complete** | Enough detail to implement and test without a follow-up question |
| **Consistent** | Does not contradict another criterion, the story statement, or the Product Context |

Flag every criterion that fails one or more.

### Step 4 — deliver the report

```
## AC Validation — <story title>

### Coverage
✅ Happy path — covered
⚠️ Alternative flows — partial: <X> covered, <Y> missing
❌ Error states — not covered

### Missing scenarios
- <case not covered, described in one line>

### Quality issues
- Criterion 3: ambiguous — "responds quickly" has no measurable threshold
- Criterion 5: bundled — two separate requirements, split them

### Contradictions
- Criterion 7 allows <X>; Product Context §<n> states <Y>

### Suggested additions
- <short description of what to add, not the finished criterion>
```

### Step 5 — summarise across stories

When reviewing more than one story, add a table:

| Story | Coverage | Quality issues | Status |
|---|---|---|---|
| <name> | ✅ / ⚠️ / ❌ | <count> | Pass / Needs work |

## Rules

- **Do not rewrite AC.** Flag issues and describe what is missing; the wording stays with the BA.
- **Do not invent scenarios** that are not grounded in the story statement or the Product Context. A missing case is only missing if the flow can actually reach it.
- **Do not flag an intentional exclusion as a gap.** Something listed under Out of Scope is a decision, not an omission.
- **Do not treat a non-functional requirement as a missing functional criterion.** Performance and load belong elsewhere.
- If a rule or flow is unclear, write `TBD` and say what would resolve it. Do not infer.

## Note on running this in the same chat as ears-ac

Running validation immediately after writing the AC in one session tends to produce agreement rather than review — the same context that produced the criteria also judges them. A fresh chat, or a pass over someone else's AC, finds more.
