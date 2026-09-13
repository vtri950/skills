---
name: ground-research
description: |
  Ground yourself and the agent before building — restate the problem in the agent's own words, split how (runtime mechanics) from why (history/intent), and pull past transcripts for continuity. Use at the start of ambiguous bugs, unfamiliar subsystems, or refactors where no human holds the full mental model. Precedes ce-brainstorm/ce-plan; feeds ce-debug with grounded hypotheses.
license: MIT
metadata:
  version: "1.0.0"
  source: "https://x.com/poteto/status/2097732320606507506"
  origin: "pstack Pt.2 — supervising someone smarter than you (lauren / @poteto)"
---

# Ground-Research: Supervise Someone Smarter Than You

Frontier models code better than you on narrow slices. They fail two ways: (1) they misunderstand your intent (under/poorly specified), (2) they lack context to do it correctly. Both are fixed by priming high-quality context — not by micromanaging implementation.

## 1. Restate first (indirect prompt)

Don't lead with your hypothesis — it caps what the agent can find and drags it down wrong paths. Force compression of noisy input into a structured problem statement first:

> `read this thread/ticket. restate in your own words, in plain english, what you think the underlying issue is — before doing anything else.`

Why it works: (a) compresses noise, (b) exposes misreads while cheap — correct before code, (c) avoids anchoring on your possibly-wrong assumptions.

Same move after a fix: `teach me why you implemented it this way and not <other way>. what tradeoffs did you make and why?` If it can't justify with evidence, it didn't ground.

## 2. Split how from why

- **How = runtime mechanics.** Trace how the subsystem actually executes. If it spans dirs/services, fan out parallel fast explorers, then synthesize. Ask: `how is <X> implemented? show call path with file:line.`
- **Why = motivation/intent.** Code never says why. Query history in parallel: git log/blame, PR review comments, tickets (Linear etc.), design docs (Notion etc.), Slack threads, monitors (Datadog), errors (Sentry), analytics events. Ask: `why are we still on <odd decision>? cite the ticket/PR/commit that decided it.`

Models confidently state ungrounded claims — the act of teaching (with citations) grounds them too, not just you.

## 3. Recall past transcripts

Multi-chat projects lose all grounding each new session. Past transcripts are the gold mine:

> `recall the work I did on <X> in the past 7 days, then read this bug report.`

Carry forward: prior hypotheses, dead ends, perf numbers, subsystem briefs. In `ce-compound` terms: check `docs/solutions/` first, then session history.

## Anti-patterns

- Accepting the agent's first confident statement without file:line or ticket/PR citation
- Asking how and why in one undifferentiated "investigate" (you get mechanics with invented motives)
- Rebuilding context from zero each chat instead of recalling

## Done checklist

```
[ ] Problem restated in agent's own words, misreads corrected pre-code
[ ] How-path traced with file:line evidence
[ ] Why-claims cite history (commit/PR/ticket/doc), not vibes
[ ] Relevant past work recalled, not rebuilt
[ ] Agent can teach the tradeoff back to you
```
