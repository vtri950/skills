---
name: retro-gate
description: |
  Turn repeat review/debug findings into deterministic enforcement — lint rule, pre-commit hook, or CI gate. Use when agent repeats the same mistake, a retro surfaces a fuzzy coding-standards rule, a multi-agent handoff arrives without acceptance criteria, or scheduled agents poll wastefully.
license: MIT
metadata:
  version: "1.0.0"
  source: "https://x.com/mattpocockuk/status/2099859946053533933, https://x.com/dedene/status/2100165534268453203"
---

# Retro-Gate: Make the Error Impossible Next Time

Reviewers and retros produce fuzzy advice (`be careful`, `verify output`). Fuzzy advice gets forgotten in two weeks. This skill converts only the mechanical subset into deterministic checks, plus two supervision invariants for multi-agent runs.

**Core loop:** collect failure → classify → smallest gate → verify red/green → keep-if-better.

## When to use

- Same reviewer finding appears twice (review-noise repeat)
- Post-fix retro asks `what would have prevented this?` and answer is mechanical
- Worker handoff lacks acceptance criteria (missing screenshots, perf numbers, repro steps)
- Scheduled agent wakes often with nothing to do (~100 runs/day at 15-min cadence)

## Workflow — run in order

### 1. Collect one failure

Read the primary source: review findings, debug postmortem, or worker transcript. Quote the exact failure. Do not generalize from memory.

### 2. Classify: gate or judgment?

Ask: `could a script decide pass/fail without taste?`

- Gate it: schema-first violations, missing required fields, untested paths, absent artifacts (screenshot/video/log), polling schedule with >50% empty runs.
- Leave as judgment: naming taste, architecture trade-offs, product direction, tone. These stay as review guidance, never as gates.

If nothing is gateable → stop. No gate is a valid outcome.

### 3. Propose the smallest enforcement

Prefer in this order, cheapest first:

1. Linter / type check (in-editor, fastest signal)
2. Pre-commit hook / filesystem check (local, blocks commit)
3. CI workflow (shared, blocks merge)

One failure → one gate. Never bundle three rules into one PR.

### 4. Add supervision invariants (conditional routing)

Pair with these two always-on pointers (see `references/templates.md` for harness wiring):

- Handoff must include definition-of-done: changed files + repro/verify steps + required artifacts (screenshots or perf numbers for UI/perf work).
- Audit schedules before adding new ones: 15-min cadence ≈ 100 runs/day. Prefer incoming signal or API polling over browser loops; a `stay quiet` instruction does not cancel runs.

### 5. Verify red/green before handoff

- Fresh test: gate fails on the original failure (red).
- Failure case: gate passes on fixed input (green) and does not fire on unrelated paths.
- Missing source = blocker. Never invent a failure to justify a gate.

### 6. Keep-if-better, else revert

Measure: runs that hit the gate, false-positive rate, time added. Keep the gate only if repeat errors drop without new friction. If worse → revert the gate, keep the note. Log the decision in the retro trail.

## Prompt templates

**Retro triage:**
> List this session's review/debug findings. For each, mark gateable (script-decidable) or judgment-call. For gateable ones, propose the smallest check: lint, hook, or CI. Output table: finding | gateable? | proposed check | where it runs.

**Gate proposal:**
> Turn this finding into a deterministic check: [finding]. Propose lint rule, pre-commit hook, or CI workflow — cheapest that catches it. Include red case (fails before fix) and green case (passes after fix). No fuzzy language.

**Handoff DoD:**
> Reject worker handoffs missing: changed files, verify steps, required artifacts. Ask for the missing piece before review.

## AGENTS.md pointers to pair with this skill

```md
- After any review/debug fix, ask what deterministic check would have caught it; propose lint, hook, or CI — cheapest first.
- For worker handoffs: require definition-of-done (files + verify steps + artifacts); for schedules: audit cadence first, prefer signal/API over polling.
```

## Source

Distilled from bookmark batch 2026-09-16: retro-to-deterministic-checks thread + multi-agent ops notes (9h live-build observations: 5-min supervision P0, verification driver + feature map, schedule-audit math). Full URLs in frontmatter `metadata.source`.
