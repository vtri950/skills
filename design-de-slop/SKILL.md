---
name: design-de-slop
description: |
  De-slop AI-generated design for landing pages, apps, and TUIs — constraints-first workflow, subtractive review, variant exploration. Use when user says design with AI, hates slop, whack-a-mole design, prototype gravity, needs design critique, component showcase, or asks to up their design game.
license: MIT
metadata:
  version: "1.0.0"
  source: "https://x.com/reactiverobot/status/2092638003789439075"
---

# Design De-Slop: How to Design with AI and Not Hate It

Engineers + AI default to slop: additive, local spot-fixes, grafted onto the codebase, visually polished but incomprehensible. This skill enforces the 7 de-slopping moves from Matt Dailey (Ref.tools) / Christopher Alexander's *Notes on the Synthesis of Form*.

**Core loop:** constraints → steal → 3-4 variants in isolation → subtract → componentize → preview with real data → thresh taste.

## When to use

- User is building or iterating a UI (landing, app, dashboard, TUI, sidebar, component)
- Agent output looks "fine but kind of bad" - extra copy, icons, borders, patchwork buttons
- User is tempted to prompt `make X more prominent` repeatedly
- Any `/design`, `/ui`, or design review request

## Workflow — Run in order, skip only if user says so

### 1. Always Consider the Whole (Constraints-First)

Do NOT jump to solutions. AI exacerbates whack-a-mole design.

1. Read `references/checklists.md#1-constraints` and build the constraint list with the user.
2. Constraints = font/sizing rules, workflows you must support, business-logic states, density, info hierarchy. You decide the constraints — don't let feedback decide them implicitly.
3. Generate an **array of solutions** that satisfy *all* constraints, not one patch.
4. When feedback arrives, ask: `Does this add, remove, or change a constraint?` If yes → return to step 1. If no → treat as papercut.
5. Papercuts: maintain `papercuts.md` (or `docs/papercuts.md`). Move fast on obvious fixes, log minor annoyances, batch them for the next cohesive redesign. Never be overly reactive.

Failure mode to block: skipping constraint enumeration → disjoint patchwork that randomly prioritizes interactions.

### 2. Steal Stuff (Before You Generate)

Most UX already solved. Every cracked designer starts with screenshots.

1. Ask user for 5-10 screenshots of products solving similar problems or communicating similar ideas. If none, propose 3 reference categories to search (use `websearch` if allowed).
2. Use screenshots as direct context for the agent. Prompt pattern: `Steal layout density from ref A, interaction from ref B, compose for constraint C.`
3. Require this before variant generation — it is the cheapest quality boost.

### 3. Iterate in a Design Tool, Not the Codebase (Kill Prototype Gravity)

Prototype gravity = first version in `src/` feels cheaper to refine → you never explore alternatives; also forces grafting onto real app constraints.

1. Generate **3-4 variants** in isolation: Figma (GOAT), Cursor Design Mode, Claude Design, or standalone `*.html` prototypes / `/showcase` sandbox. Never iterate directly in `src/components` on first pass.
2. Tool must give fine control + minimal extra context + fast iteration. Confirm path with user: `figma | html-prototype | /showcase`.
3. Present variants side-by-side for selection. Do not merge variants into one Frankenstein.

### 4. Remove Stuff (Subtractive Pass)

Agents love to add. Your job is to remove. Applies to code *and* UI.

1. After any generation, run the subtractive pass from `references/checklists.md#4-subtraction`.
2. For **every element** ask: `Do I actually need that?` (copy, lines, icons, borders, shadows, extra states, try-catch, duplicate utils)
3. Output a `Removed:` list with rationale. If nothing to remove, state why.
4. Enforce minimalism over "polished but bad."

### 5. Use Components and Libraries

Separate views and logic. Create reusable components → visual cohesion vs patchwork.

1. Build component in `/showcase` (or `app/showcase`, `pages/showcase` — adapt to framework) *before* wiring to main app.
2. Define props API, states, and variants there. Confirm reusability.
3. Only after showcase approval, integrate into feature.

### 6. Use Preview Deploys with Real Data

Best way to evaluate design = real data + real backend. Mock data lies.

1. After showcase approval, wire to backend and deploy a preview URL (Vercel/Netlify/Cloudflare preview, etc.).
2. Hold it with real data — expect polish/re-work even if agent built exactly what you asked.
3. For large FE+BE features: split PRs. BE changes verified via unit/integration tests. FE changes require human verification + shareable preview link.

### 7. Explore Your Taste (Threshing)

Taste = reflecting on your own reaction, building a solution library via reps.

1. Run the threshing loop: throw design in the middle, beat it with critique until good. Use `references/checklists.md#7-taste` prompts.
2. Product engineers are great at `doesn't work` but weak at `how to fix` due to small solution library. Fix = reps + reflection, not Brooklyn-loft mystique.
3. After each variant, force a 2-sentence critique: `What feels off? What constraint does it violate?`
4. Iterate until threshold: `good enough` is when constraints satisfied + subtractive pass clean + real-data preview feels right.

## Quick-Apply Checklist (for agent self-review)

Before handing off any UI:

```
[ ] Constraints listed explicitly and shared with user
[ ] 3-4 variants generated outside src/ (or rationale why 1 was enough)
[ ] References/screenshots used as context
[ ] Subtractive pass done with Removed: list
[ ] Built in /showcase before main app integration
[ ] Preview deploy plan with real data (or split FE/BE PRs)
[ ] 2-sentence taste critique included
```

## Prompt Templates

**Constraint elicitation:**
> List all constraints for [feature]: workflows, states, sizing/type rules, density, hierarchy. If feedback arrives, does it change a constraint or is it a papercut for papercuts.md?

**Variant generation:**
> Using refs [A,B,C], generate 4 isolated variants in [figma/html/showcase] that satisfy constraints [C1..Cn]. Do not touch src/. Present side-by-side.

**Subtractive:**
> For each element in this design, do we need it? Remove unnecessary copy, icons, dividers, borders. Output Removed: list.

**AGENTS.md one-liner to pair with this skill:**
```md
- For any UI/design task, invoke design-de-slop skill first. Do not build v1 directly in src/ (prototype gravity). Default to subtractive review after every generation.
```

## Source

Distilled from Matt Dailey (@reactiverobot) — "How I Design with AI. As an engineer who is not a designer and hates slop." (Aug 26 2026) — https://x.com/reactiverobot/status/2092638003789439075 — 7 moves + Christopher Alexander's *Notes on the Synthesis of Form*.
