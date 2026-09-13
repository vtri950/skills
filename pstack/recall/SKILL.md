---
name: recall
description: |
  Pull what you and your agents already figured out in earlier chats into this one. Use at the start of any session that continues past work, so a fresh agent doesn't rebuild context from zero.
license: MIT
metadata:
  version: "1.0.0"
  source: "https://x.com/poteto/status/2097732320606507506"
  origin: "pstack Pt.2 — /recall (lauren / @poteto)"
---

# /recall — don't start from zero

Every new chat wipes the rich context your last agent built. Past transcripts are the gold mine. Type this:

> `/recall the work i did yesterday on virtualization and then read this bug report on slack`

## What the agent does

1. Summarizes the relevant past work: what was tried, what failed, numbers measured, subsystem briefs.
2. States what's still open vs settled.
3. Applies it to the new task in the same turn.

## Be specific about scope

`yesterday`, `past 7 days`, `on virtualization` — "recall everything" returns mush. Check your durable learnings (`docs/solutions/`) first; `/recall` is for the session-level detail those don't hold.
