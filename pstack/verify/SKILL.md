---
name: verify
description: |
  Prove the app actually works by driving it like a user — video, screenshots, traces — not just "tests pass." Use to build the one-time driver CLI + feature map for your app, to verify every change with proof, and to maintain both daily.
license: MIT
metadata:
  version: "1.0.0"
  source: "https://x.com/poteto/article/2094457600259842065"
  origin: "pstack Pt.1 — verification skill / control-app (lauren / @poteto)"
---

# /verify — prove it runs

Tests alone are not verification. Verified = the agent drove the real app and showed proof. Three jobs, in order:

## 1. Build the driver (once per app)

A tiny CLI that uses your app like a user: open screens, click, log in, seed data, take screenshots / video / perf traces. Web or Electron → Chrome DevTools Protocol. iOS → simulator. No rich runtime → build a dev sidecar (or treat undebuggability as a stack smell).

House rules for the CLI: JSON output, `--dry-run` on anything destructive, errors that say what to do **next**, rich `--help`.

> `create a control CLI for this app: goto, click, screenshot, video, trace, seed. JSON output.`

## 2. Map the features (once, then daily)

`references/features/` — one index + one short file per feature: what it is, how a user reaches it, what auth/data it needs. Have the agent write it by clicking through the app. Refresh daily. A stale map silently rots every agent that reads it.

## 3. Verify every change (always)

End work prompts with the proof requirement:

> `verify your changes with the driver and show me a video and screenshots as proof`

Perf work: `trace before, fix, trace after — N runs, show the numbers.`
Scale (the /swarm pattern): fan out many agents running the same driver to fuzz the app or confirm a win with a real sample size.

**Done = proof attached.** No video, screenshot, or trace → not done.
