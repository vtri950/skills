# skills

A collection of skills for AI coding assistants — each skill teaches an assistant a specific workflow or capability.

## Available skills

| Skill | Description |
|-------|-------------|
| [visual-docs](./visual-docs/) | Author and convert documentation to rich MDX files viewable locally with [planview](https://github.com/vtri950/planview) |
| [design-de-slop](./design-de-slop/) | De-slop AI-generated design — constraints-first, subtractive review, variant exploration for landing pages/apps/TUIs |
| [copilot-optimize](./copilot-optimize/) | Optimize any GitHub Copilot setup: stable prefix, prompt audit, cheap-first routing |
| [x-bookmarks](./x-bookmarks/) | Distill X bookmarks by taste — fetch, triage, route to gist/skill/tool, generalize across harnesses |
| [retro-gate](./retro-gate/) | Turn repeat review findings into deterministic checks — lint, hook, or CI gate plus handoff/schedule invariants |
| [design-doc](./design-doc/) | Scope, draft, and drive review of software design docs — write-gate, penalty filter, section checklist, signoff trail |

### pstack SDLC (`pstack/`)

Curated SDLC subset of the upstream pstack plugin — 16 skills vendored unchanged (see `pstack/README.md` for the per-family index). Excludes `poteto-mode`, `principle-*`, and non-SDLC utilities.

| Skill | Description |
|-------|-------------|
| [research/how](./pstack/research/how/) · [research/why](./pstack/research/why/) · [research/teach](./pstack/research/teach/) · [research/recall](./pstack/research/recall/) | Research — how it works, why it's this way, plain explanations, resume context |
| [design/architect](./pstack/design/architect/) · [design/arena](./pstack/design/arena/) · [design/figure-it-out](./pstack/design/figure-it-out/) | Design — sketch before code, parallel candidates, large migrations |
| [verify/create-verification-skill](./pstack/verify/create-verification-skill/) · [verify/maintain-verification-skill](./pstack/verify/maintain-verification-skill/) · [verify/swarm](./pstack/verify/swarm/) | Verify — project-local driver skills, audits, parallel workers |
| [review/interrogate](./pstack/review/interrogate/) · [review/blast-radius](./pstack/review/blast-radius/) · [review/tdd](./pstack/review/tdd/) · [review/show-me-your-work](./pstack/review/show-me-your-work/) | Review gates — adversarial review, impact analysis, TDD, decision trails |
| [docs/technical-writing](./pstack/docs/technical-writing/) · [docs/unslop](./pstack/docs/unslop/) | Docs — Diátaxis standard, de-slop prose |

Plus [`pstack/SDLC-GIST.md`](./pstack/SDLC-GIST.md) — takeaway notes from the Pt.1/Pt.2 posts.

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

### Review visualization (`humanlayer/`)

Curated review-visualization subset of the upstream skills collection — 1 skill vendored unchanged (see `humanlayer/README.md` for the index).

| Skill | Description |
|-------|-------------|
| [show-me](./humanlayer/show-me/) | Explain the current topic visually — smallest diagram or code-shape sketch (pseudocode, call tree, component tree, file tree, mermaid, `diff`) |

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
├── x-bookmarks/                 # X bookmarks → collection by taste (fetch → triage → route → generalize)
├── retro-gate/                  # retro findings → deterministic gates (lint → hook → CI) + handoff/schedule invariants
├── design-doc/                  # design docs — write-gate → size → penalty filter → subset draft → SLOs → signoff
├── pstack/                      # curated pstack SDLC subset (16 skills, unchanged)
│   ├── research/how|why|teach|recall/
│   ├── design/architect|arena|figure-it-out/
│   ├── verify/create-verification-skill|maintain-verification-skill|swarm/
│   ├── review/interrogate|blast-radius|tdd|show-me-your-work/
│   ├── docs/technical-writing|unslop/
│   └── SDLC-GIST.md                        # takeaway notes
└── compound-engineering/        # EveryInc source — 6 skills segregated by origin
    ├── ce-brainstorm/
    ├── ce-plan/
    ├── ce-work/
    ├── ce-compound/
    ├── ce-code-review/
    └── ce-debug/
└── humanlayer/                 # review-visualization source — skills segregated by origin
    └── show-me/                # smallest visual for the current topic (diagrams, code-shape sketches)
```

Add future sources as sibling dirs (e.g. `acme-corp/`, `my-skills/`) to keep origins segregated.

## Attribution

`compound-engineering/*` (`ce-brainstorm`, `ce-plan`, `ce-work`, `ce-compound`, `ce-code-review`, `ce-debug`) are sourced from [EveryInc/compound-engineering-plugin](https://github.com/EveryInc/compound-engineering-plugin) (MIT, © Every Inc.) at `v3.24.0`. Original `SKILL.md` and `references/` are preserved unchanged under `compound-engineering/`.

`pstack/*` (16 SDLC skills) are vendored from the upstream pstack plugin copy in this workspace at `../plugins/pstack/skills/`. `SKILL.md`, `references/`, and `scripts/` are preserved unchanged under `pstack/`. Excluded upstream: `poteto-mode`, all `principle-*`, `bro`, `no-comments`, `reflect`, `automate-me`, `setup-pstack`, `typescript-best-practices`. `pstack/SDLC-GIST.md` is original notes on a two-part SDLC series on agent workflows.

`humanlayer/show-me` is sourced from the upstream skills collection (MIT) at `plugins/show-me/skills/show-me`. Original `SKILL.md` and `agents/` are preserved unchanged under `humanlayer/`.

## How these skills compound

```
ideate (optional) → ce-brainstorm → ce-plan → ce-work → ce-compound
                                  ↘ ce-code-review / ce-debug (on-demand quality gates)
```

Each cycle's `ce-compound` output (`docs/solutions/`) grounds the next `ce-brainstorm`/`ce-plan` run.
