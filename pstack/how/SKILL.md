---
name: how
description: |
  Trace how something in the codebase actually works at runtime. Use when you catch yourself or the agent guessing about mechanics — ask "how is X implemented?" and get the real execution path with file:line evidence.
license: MIT
metadata:
  version: "1.0.0"
  source: "https://x.com/poteto/status/2097732320606507506"
  origin: "pstack Pt.2 — /how (lauren / @poteto)"
---

# /how — show me how it works

Mechanics, not motives. Type this:

> `/how is virtualization implemented?`

## What the agent does

1. Finds the code that implements X — actual code, not docs, not memory.
2. If X spans multiple directories or services, fans out parallel explorer agents and synthesizes.
3. Answers as a click-through path: entry point → key functions → exit, every step with `file:line`.
4. States what it did **not** read, so you know the limits.

## Good vs bad

Good: `List.tsx:42 → useVirtualizer.ts:87 → ...` — a path you can follow.
Bad: a confident paragraph with zero file references. Reject it and send the agent back to read.

## Pairs with

- `/why` — same question, opposite half: motives and history.
- `/teach` — explains the `/how` result back in plain language.
