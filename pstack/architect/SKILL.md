---
name: architect
description: |
  Design bigger changes in the open with evidence — ground, sketch competing candidates, cross-judge, prototype open questions, and scrap bad designs fast. Use for new subsystems, shared packages, refactors, and migrations instead of accepting the agent's first design or debating abstract plans.
license: MIT
metadata:
  version: "1.0.0"
  source: "https://x.com/poteto/status/2097732320606507506"
  origin: "pstack Pt.2 — /architect + prototyping playbook (lauren / @poteto)"
---

# /architect — design with evidence, scrap without mercy

For new subsystems, shared packages, refactors, migrations. Five phases, in order:

## 1. Ground

Run `/how` + `/why` over the affected code first. No design from vibes.

## 2. Sketch competing candidates

Produce 2+ designs as **types + signatures + a caller usage sketch** — not bodies. Design from the call site inward. Use different models (or fresh sessions) per candidate so they genuinely differ.

## 3. Cross-judge

A **different** model/session scores the candidates on a fixed rubric — interface depth, failure modes, your red-flag list — and picks or merges a winner.

## 4. Prototype the open questions

Anything the sketches disagree on gets a throwaway prototype in a scratch dir, driven with `/verify` (screenshots, timings). Evidence decides, not debate.

> `prototype a few options for <feature>. drive each with the driver, take videos/screenshots + timings for me to review.`

Never adversarially review abstract plans — agents invent phantom risks and gold-plate against problems that never happen.

## 5. Implement, ready to scrap

Fill in the bodies. Two scrap signals, non-negotiable:

- types needing `any` or forced casts
- the same workaround appearing in unrelated call sites

Either one = the architecture is wrong. Throw it away and re-sketch.

---

**Prompts to copy:**

> `/architect <feature>. answer open questions with prototypes. let me review before proceeding.`

After the design settles:

> `turn this design into a plan of small PRs, each with live verification steps.`

Commit big plans temporarily so parallel agents see the WIP; delete on landing — permanent plans rot.
