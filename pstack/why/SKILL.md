---
name: why
description: |
  Find out why the code is the way it is — the decision, ticket, or incident behind it. Use when you hit an odd choice ("why are we still on X?") and want the cited history instead of a plausible-sounding story.
license: MIT
metadata:
  version: "1.0.0"
  source: "https://x.com/poteto/status/2097732320606507506"
  origin: "pstack Pt.2 — /why (lauren / @poteto)"
---

# /why — tell me why it is this way

Code tells you what happens. It never tells you why someone wrote it that way. Type this:

> `/why are we still stuck on an old version of node.js?`

## What the agent does

Searches these in parallel and cites what it finds — with links, not vibes:

- git history and blame, PR review comments
- tickets (Linear/Jira), design docs (Notion, etc.)
- Slack conversations, Datadog monitors, Sentry errors, analytics events

## Good vs bad

Good: "Pinned in PR #1234 (link) because…; Slack thread (link) shows…"
Bad: a plausible story with no links. Reject it.

One honest answer is always allowed: **"no record found."** That beats an invented motive — and tells you the decision is now yours to make deliberately.
