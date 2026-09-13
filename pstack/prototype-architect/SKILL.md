---
name: prototype-architect
description: |
  Plan through code, not abstract docs — README/tutorial-first design, parallel throwaway prototypes with empirical proof, cross-model architecture arena, and verifiable execution plans. Use for new subsystems, shared packages, refactors, and migrations. Alternative to ce-plan's research-then-write path when the design space needs evidence; produces the plan ce-work executes.
license: MIT
metadata:
  version: "1.0.0"
  source: "https://x.com/poteto/status/2097732320606507506"
  origin: "pstack Pt.2 — planning, prototyping, architecture (lauren / @poteto)"
---

# Prototype-Architect: Measure a Hundred Times, Cut Once

Abstract plans give the illusion of progress — long docs, no substance. Agents left in the abstract hallucinate theoretical risks and over-engineer against problems that never happen. Plan with code instead.

## 1. README-first (work backwards)

For shared code / packages / service boundaries, write the caller's tutorial *before* the implementation. Start from developer experience:

> `write a tutorial for how I would use this new <package> to <job>. don't implement yet.`

Split docs by mode (Diataxis) so they stop blending into slop: Tutorial (learn by doing) / How-to (solve one real problem) / Reference (dry API/flags) / Explanation (background, tradeoffs). One doc, one mode.

The tutorial is the concrete target the agent checks its own work against — and the thing you can actually review.

Compound prompt shape:

```
(1) recall my work on <X> from past 7 days. use how/why to brief current implementation.
(2) draft the new <X> that categorically eliminates <known bugs>. start with the user tutorial.
(3) teach me why the new approach beats the current one, with proof.
```

## 2. Prototype arena (don't accept the first design)

Two mistakes: accepting the first design, and overcooking the plan without evidence. Instead, run parallel throwaway sketches:

- Build 2–3 variants in a scratch dir or behind a switcher — never directly in the real path on first pass (kills prototype gravity; cf. design-de-slop for UI)
- Drive each with your verification loop (see verify-app): screenshots/video, timings, layout/perf numbers
- Present variants + evidence side by side; pick, don't merge into a Frankenstein

> `prototype a few options for <feature>. drive each with the control CLI, take videos/screenshots + timings for me to review.`

Prototypes let agents answer their own open questions empirically instead of waiting on you.

## 3. Architect arena (for bigger changes)

Structure the design as a mini-loop:

1. **Ground** — how/why brief over affected systems (see ground-research).
2. **Sketch** — spawn independent candidate runners, ideally across model families. Each delivers: caller usage sketch, core type definitions, public signatures, concise rationale. Design from call sites inward; check interface depth and failure modes on weak models against a red-flag list.
3. **Cross-judge + synthesize** — a judge using a *different* model than the authors scores candidates on a strict rubric; synthesize the winner.
4. **Implement against the sketch** — fill bodies. New params / extra state surfacing mid-build = discrepancy to report, not silently absorb.
5. **Scrap when wrong** — same workaround in unrelated call sites, or types needing `any`/casts = empirical proof the architecture is wrong. Throw it away and re-sketch.

> `/architect this <feature> first, answer open questions with prototypes. let me review before proceeding.`

## 4. Verifiable execution plan (only after design settles)

Convert the winning design into a tactical plan where every task is structured around proof — tests alone are insufficient; each item states how it is *run and observed* (command + expected evidence). Validate plan shape by script/lint if you can. Break large work into small self-contained PRs; for week-scale migrations, commit plans temporarily so parallel agents see WIP, then delete when done — permanent plans rot.

> `turn this design into a plan. break into small verifiable PRs, each with live verification steps.`

## Anti-patterns

- Reviewing abstract plans adversarially (breeds hallucinated risks)
- One-shot design accepted without competing variants
- Plans specifying implementation details while under-specifying UX, verification, and tradeoffs
- Keeping execution plans in the repo forever

## Done checklist

```
[ ] Caller tutorial written and readable before implementation
[ ] 2+ prototypes built in scratch, evidence compared, winner picked
[ ] Architecture sketched types-first, cross-judged, discrepancies surfaced
[ ] Scrap signals checked (any/casts, repeated workarounds)
[ ] Execution plan: every task has run-it-and-prove-it steps, small PRs
```
