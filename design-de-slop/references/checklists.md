# Design De-Slop — Checklists & Templates

## 1. Constraints

Enumerate before generating. Revisit when feedback arrives.

**Template — `constraints.md`:**
```md
# Constraints for [feature]

## Hard constraints (must satisfy)
- Workflows: [e.g., onboarding 3 steps, guest checkout]
- States: [empty, loading, error, permission-denied]
- Type/sizing: [font scale, spacing, density: compact vs airy]
- Business rules: [e.g., sidebar must surface X, Y, Z in <200px]
- Tech: [framework, component library, a11y]

## Soft constraints (tradeoffs)
- Priority order: [what to optimize when constraints conflict]

## Papercuts log (batched, not fixed immediately)
| date | report | constraint changed? | action |
|------|--------|---------------------|--------|
|      |        | yes/no              | log / fix now |
```

**Gate:** If a new request changes a hard constraint → regenerate variants from scratch. Don't spot-patch.

## 2. Steal — Moodboard Checklist

- [ ] Collected 5-10 screenshots of same-problem products
- [ ] For each ref, noted: what to steal (layout, density, interaction, copy tone)
- [ ] Refs attached as image context or URLs in prompt
- [ ] Prompt explicitly cites refs: `Steal [X] from ref A for constraint C2`

Anti-pattern: prompting from memory. Always show, don't describe.

## 3. Variant Generation (Anti-Prototype-Gravity)

- [ ] Output path is isolated: `/showcase`, `prototype.html`, Figma — NOT `src/components`
- [ ] Generated 3-4 variants (different tradeoffs, not color swaps)
- [ ] Each variant satisfies *all* hard constraints
- [ ] Presented side-by-side for human pick (no auto-merge)

**Prompt template:**
```
Constraints: [paste constraints.md hard constraints]
References: [attach 5-10 screenshots, label A..J]
Task: Generate 4 isolated variants in [html/showcase/figma] exploring
different solutions to the same constraints. Vary layout/density/hierarchy,
not just theme. Do not edit src/. Output each variant as standalone file/component.
```

## 4. Subtraction — Subtractive Pass

Agents add by default (copy, icons, dividers, borders, shadows, extra states, try-catch, duplicate utils).

**Checklist — for every element ask `Do I need that?`:**

UI:
- [ ] Copy: can this sentence be 40% shorter or deleted?
- [ ] Icons: does this icon add meaning or just decor?
- [ ] Lines/dividers/borders: does it separate or just add noise?
- [ ] Shadows/gradients: does it signal elevation or just polish?
- [ ] States/affordances: is this affordance used in a real workflow?

Code:
- [ ] Wrappers/try-catch: belt-and-suspenders?
- [ ] Duplicate utils: re-implemented helper?
- [ ] Props/variants: unused API surface?

**Output format:**
```md
Removed:
- [element] — reason: [no constraint requires it / duplicates X / adds noise]
Kept but simplified:
- [element] — from [verbose] to [minimal]
```

## 5. Components & Showcase

- [ ] Created `/showcase` route (or `app/showcase`, `pages/showcase`)
- [ ] Component built there first with props API + states + variants
- [ ] Reuses existing library (no one-off button re-implementation)
- [ ] Views separated from logic (presentational vs container)

**Integration gate:** No PR to main app until showcase approval + subtractive pass clean.

## 6. Preview Deploys (Real Data)

- [ ] Preview URL generated (Vercel/Netlify/Preview env) with real backend
- [ ] Tested with real data shapes: empty, long, error, permission-denied
- [ ] For FE+BE feature: split PRs — BE (tests) / FE (human preview)
- [ ] Shared link for human verification; diff alone is not approval

Failure mode: approving from mock data or code diff.

## 7. Taste — Threshing Prompts

Taste = reflection + solution library built via reps. Use after each variant set.

**Threshing questions (pick 2-3):**
- What feels off within 3 seconds? Name the constraint it violates.
- If you had to delete 30% more, what would go?
- Which variant would you tolerate using daily for a week?
- What would a cropped screenshot of just this component communicate without context?
- Does hierarchy survive at 50% zoom / mobile?

**Loop:**
1. Generate 4 → pick best → 2-sentence critique → new constraint or subtraction → regenerate 2-3
2. Stop when: constraints satisfied + subtraction clean + preview with real data feels right (not just looks right)

## Papercuts.md Workflow

Track minor issues instead of reacting. Template at repo root or `docs/papercuts.md`:

```md
# Papercuts — batched for next redesign

Do not spot-fix. Evaluate each report:
1. Does it change a hard constraint? → update constraints.md, plan redesign.
2. Is it an obvious bug? → fix now.
3. Else → log here, batch for cohesive pass.

| id | date | area | report | freq | constraint impact |
|----|------|------|--------|------|-------------------|
```
