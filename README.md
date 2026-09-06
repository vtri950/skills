# skills

A collection of skills for AI coding assistants — each skill teaches an assistant a specific workflow or capability.

## Available skills

| Skill | Description |
|-------|-------------|
| [visual-docs](./visual-docs/) | Author and convert documentation to rich MDX files viewable locally with [planview](https://github.com/vtri950/planview) |

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

## How these skills compound

```
ideate (optional) → ce-brainstorm → ce-plan → ce-work → ce-compound
                                  ↘ ce-code-review / ce-debug (on-demand quality gates)
```

Each cycle's `ce-compound` output (`docs/solutions/`) grounds the next `ce-brainstorm`/`ce-plan` run.
