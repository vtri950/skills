# skills

A collection of skills for AI coding assistants — each skill teaches an assistant a specific workflow or capability.

## Available skills

| Skill | Description |
|-------|-------------|
| [visual-docs](./visual-docs/) | Author and convert documentation to rich MDX files viewable locally with [planview](https://github.com/vtri950/planview) |
| [design-de-slop](./design-de-slop/) | De-slop AI-generated design — constraints-first, subtractive review, variant exploration for landing pages/apps/TUIs |
| [copilot-optimize](./copilot-optimize/) | Optimize any GitHub Copilot setup: stable prefix, prompt audit, cheap-first routing |

### pstack (`pstack/`)

Self-contained collection: curated SDLC skills + everything `poteto-mode` needs to run, vendored unchanged (see `pstack/README.md` for index, dependency map, harness notes).

| Skill | Description |
|-------|-------------|
| [mode/poteto-mode](./pstack/mode/poteto-mode/) · [mode/setup-pstack](./pstack/mode/setup-pstack/) | Orchestrator — trigger→skill routing, 23 playbooks, principles enforcement; per-role model config |
| [research/how](./pstack/research/how/) · [research/why](./pstack/research/why/) · [research/teach](./pstack/research/teach/) · [research/recall](./pstack/research/recall/) · [research/reflect](./pstack/research/reflect/) | Research — how/why, plain explanations, resume context, transcript-driven skill edits |
| [design/architect](./pstack/design/architect/) · [design/arena](./pstack/design/arena/) · [design/figure-it-out](./pstack/design/figure-it-out/) | Design — sketch before code, parallel candidates, large migrations |
| [verify/create-verification-skill](./pstack/verify/create-verification-skill/) · [verify/maintain-verification-skill](./pstack/verify/maintain-verification-skill/) · [verify/swarm](./pstack/verify/swarm/) · [verify/control-cli](./pstack/verify/control-cli/) · [verify/control-ui](./pstack/verify/control-ui/) | Verify — project-local driver skills, audits, parallel workers, CLI/UI drivers |
| [review/interrogate](./pstack/review/interrogate/) · [review/blast-radius](./pstack/review/blast-radius/) · [review/no-comments](./pstack/review/no-comments/) · [review/deslop](./pstack/review/deslop/) · [review/tdd](./pstack/review/tdd/) · [review/show-me-your-work](./pstack/review/show-me-your-work/) | Review gates — adversarial review, impact analysis, comment hygiene, pre-commit gate, TDD, decision trails |
| [docs/technical-writing](./pstack/docs/technical-writing/) · [docs/unslop](./pstack/docs/unslop/) | Docs — Diátaxis standard, de-slop prose |
| `principles/` (21 skills) · `agents/` (poteto-agent, comment-sicko) | Ground — decision principles `poteto-mode` cites; subagent definitions skills spawn by name |

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
├── pstack/                      # self-contained pstack collection (vendored unchanged)
│   ├── mode/poteto-mode|setup-pstack/      # orchestrator + model config
│   ├── research/how|why|teach|recall|reflect/
│   ├── design/architect|arena|figure-it-out/
│   ├── verify/create-verification-skill|maintain-verification-skill|swarm|control-cli|control-ui/
│   ├── review/interrogate|blast-radius|no-comments|deslop|tdd|show-me-your-work/
│   ├── docs/technical-writing|unslop/
│   ├── principles/ (21 skills) + agents/   # ground + subagent defs
│   └── SDLC-GIST.md                        # takeaway notes
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

`pstack/*` is vendored from the upstream plugin copies in this workspace — `mode/`, `research/`, `design/`, `verify/create-verification-skill`, `verify/maintain-verification-skill`, `verify/swarm`, `review/interrogate`, `review/blast-radius`, `review/no-comments`, `review/tdd`, `review/show-me-your-work`, `docs/`, `principles/`, `agents/` from `../plugins/pstack/skills/` (+ `../plugins/pstack/agents/`), and `verify/control-cli`, `verify/control-ui`, `review/deslop` from `../plugins/cursor-team-kit/skills/`. `SKILL.md`, `references/`, `scripts/`, and agent defs are preserved unchanged under `pstack/`. Excluded upstream: `bro`, `automate-me`, `typescript-best-practices`, `docs/` guide. `pstack/SDLC-GIST.md` is original notes on lauren's ([@poteto](https://x.com/poteto)) [Pt.1](https://x.com/poteto/article/2094457600259842065) + [Pt.2](https://x.com/poteto/status/2097732320606507506) posts.

## How these skills compound

```
ideate (optional) → ce-brainstorm → ce-plan → ce-work → ce-compound
                                  ↘ ce-code-review / ce-debug (on-demand quality gates)
```

Each cycle's `ce-compound` output (`docs/solutions/`) grounds the next `ce-brainstorm`/`ce-plan` run.
