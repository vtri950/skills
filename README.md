# skills

A collection of skills for AI coding assistants — each skill teaches an assistant a specific workflow or capability.

## Available skills

| Skill | Description |
|-------|-------------|
| [visual-docs](./visual-docs/) | Author and convert documentation to rich MDX files viewable locally with [planview](https://github.com/vtri950/planview) |
| [design-de-slop](./design-de-slop/) | De-slop AI-generated design — constraints-first, subtractive review, variant exploration for landing pages/apps/TUIs |
| [copilot-optimize](./copilot-optimize/) | Optimize any GitHub Copilot setup: stable prefix, prompt audit, cheap-first routing |

### pstack SDLC (`pstack/`)

Source: lauren ([@poteto](https://x.com/poteto)) — [Complete Guide to pstack Pt.1](https://x.com/poteto/article/2094457600259842065) + [Pt.2](https://x.com/poteto/status/2097732320606507506) (MIT, distilled — tool-agnostic, no pstack dependency)

| Skill | Description |
|-------|-------------|
| [verify-app](./pstack/verify-app/) | Agent-operable verification loop — control CLI that drives the app like a user + Feature Map, maintained like infra |
| [ground-research](./pstack/ground-research/) | Ground before building — restate in own words, split how (mechanics) from why (history/intent), recall past transcripts |
| [prototype-architect](./pstack/prototype-architect/) | Plan through code — README/tutorial-first, parallel prototypes with proof, cross-model architect arena, verifiable plans |

Plus [`pstack/SDLC-GIST.md`](./pstack/SDLC-GIST.md) — judgments + workflow prompts worth keeping outside skills.

### Compound Engineering (`compound-engineering/`)

Source: [EveryInc/compound-engineering-plugin](https://github.com/EveryInc/compound-engineering-plugin) `v3.24.0` (MIT)

| Skill | Description |
|-------|-------------|
| [ce-brainstorm](./compound-engineering/ce-brainstorm/) | Explore vague or ambitious ideas into a right-sized requirements-only unified plan. One question at a time, named gap lenses. |
| [ce-plan](./compound-engineering/ce-plan/) | Enrich requirements into implementation-ready plans with U-IDs, test scenarios and confidence checks (WHAT, not HOW). |
| [ce-work](./compound-engineering/ce-work/) | Execute an implementation-ready plan end-to-end — figure out HOW with code in front of you, then verify locally. |
| [ce-compound](./compound-engineering/ce-compound/) | Capture verified learnings into `docs/solutions/` so the next brainstorm/plan starts smarter. The compounding step. |
| [ce-code-review](./compound-engineering/ce-code-review/) | Structured multi-agent review (personas, confidence-gated findings) against the plan and `CODING_STANDARDS.md`. Report-only. |
| [ce-debug](./compound-engineering/ce-debug/) | Diagnosis loop for bugs/failing behavior — causal chain, predictions, then optional fix and PR handoff. |

## How to install a skill

Copy the skill directory into your assistant's skills folder and reference it in your configuration. For example:

```bash
cp -r visual-docs ~/.your-assistant/skills/
cp -r compound-engineering/ce-plan ~/.your-assistant/skills/
# or all 6 at once:
cp -r compound-engineering/* ~/.your-assistant/skills/
```

Refer to your AI assistant's documentation for the exact installation path.

## Repository layout

```
skills/                          # repo root
├── visual-docs/                 # standalone skill
├── design-de-slop/              # anti-slop design workflow (constraints → variants → subtraction)
├── copilot-optimize/            # Copilot cost/quality optimizer
├── pstack/                      # poteto source — 3 SDLC skills + SDLC-GIST.md
│   ├── verify-app/
│   ├── ground-research/
│   └── prototype-architect/
└── compound-engineering/        # EveryInc source — 6 skills segregated by origin
    ├── ce-brainstorm/
    ├── ce-plan/
    ├── ce-work/
    ├── ce-compound/
    ├── ce-code-review/
    └── ce-debug/
```

Add future sources as sibling dirs (e.g. `acme-corp/`, `my-skills/`) to keep origins segregated.

## Attribution

`compound-engineering/*` (`ce-brainstorm`, `ce-plan`, `ce-work`, `ce-compound`, `ce-code-review`, `ce-debug`) are sourced from [EveryInc/compound-engineering-plugin](https://github.com/EveryInc/compound-engineering-plugin) (MIT, © Every Inc.) at `v3.24.0`. Original `SKILL.md` and `references/` are preserved unchanged under `compound-engineering/`.

`pstack/*` (`verify-app`, `ground-research`, `prototype-architect`) are distilled from lauren's ([@poteto](https://x.com/poteto)) [Complete Guide to pstack Pt.1](https://x.com/poteto/article/2094457600259842065) + [Pt.2](https://x.com/poteto/status/2097732320606507506). Rewritten tool-agnostic (no pstack/`/poteto-mode` dependency); see `pstack/SDLC-GIST.md` for non-skill judgments.

## How these skills compound

```
ideate (optional) → ce-brainstorm → ce-plan → ce-work → ce-compound
                                  ↘ ce-code-review / ce-debug (on-demand quality gates)
```

Each cycle's `ce-compound` output (`docs/solutions/`) grounds the next `ce-brainstorm`/`ce-plan` run.
