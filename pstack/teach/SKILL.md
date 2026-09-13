---
name: teach
description: |
  Make the agent explain something back to you in plain, intuitive language before it writes code. Use to check the agent's understanding, to learn an unfamiliar subsystem yourself, or after a change to demand its tradeoff account.
license: MIT
metadata:
  version: "1.0.0"
  source: "https://x.com/poteto/status/2097732320606507506"
  origin: "pstack Pt.2 — /teach (lauren / @poteto)"
---

# /teach — explain it back to me

The understanding check. Two moments to type it:

**1. Before code** (catches misreads while they're cheap):

> `/poteto-mode read this slack thread. restate in your own words and in plain english what you think the underlying issue is`

Don't state your own hypothesis first — it anchors the agent and caps what it can find. If it fixates on a red herring, correct it now, not after a diff exists.

**2. After a change** (demands the tradeoff account):

> `/teach me why you implemented it this way and not <other way>. what were the tradeoffs you made and why?`

Under the hood this calls `/how` (mechanics) then `/why` (motives) and explains both simply.

## The rule

An explanation with no `file:line` and no history link is not an explanation — send it back. The side effect is the point: teaching forces the agent to actually read the code and check history instead of stating things confidently from vibes. It grounds the agent, not just you.
