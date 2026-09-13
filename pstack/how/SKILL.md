---
name: how
description: |
  The author's /how, verbatim: trace runtime mechanics of a subsystem, with parallel explorers when it spans directories or services.
license: MIT
metadata:
  version: "2.0.0"
  source: "https://x.com/poteto/status/2097732320606507506"
  origin: "pstack Pt.2 — lifted verbatim from lauren / @poteto (light markdown formatting only)"
---

# /how

> Lifted verbatim from [The Complete Guide to pstack Pt. 2](https://x.com/poteto/status/2097732320606507506). Part of `/teach` — see the `teach` skill for the full section this comes from.

Under the hood, `/teach` calls out to `/how` and `/why`.

`/how` traces runtime mechanics. When you ask `/how`, the agent assesses the complexity of the subsystem. If the subsystem spans multiple directories or services, it spawns parallel explorer agents on fast, efficient models like Grok.

`/how is virtualization implemented?`
