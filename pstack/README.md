# pstack — curated SDLC collection

SDLC-relevant subset of the upstream pstack plugin, vendored unchanged
(`SKILL.md`, `references/`, `scripts/` preserved as-is).
Source of truth: `../plugins/pstack` — re-copy from there to pick up updates.

Deliberately excluded: `poteto-mode` (orchestrator/style mode — overlaps the
`compound-engineering` loop already in this repo; see note below),
all `principle-*` micro-skills, `bro`, `no-comments`, `reflect`,
`automate-me`, `setup-pstack`, `typescript-best-practices`.

## Research — understand before touching code

| Skill | Use when |
|-------|----------|
| [research/how](./research/how/) | "how does X work" — walkthroughs, ownership/layering questions |
| [research/why](./research/why/) | "why is it this way" — rationale from git, PRs, tickets, docs, chat, observability |
| [research/teach](./research/teach/) | "explain it plainly" — runs `how` + `why`, weaves one clear account |
| [research/recall](./research/recall/) | "where did I leave off" — rebuild context from chat history + shared record |

## Design — shape before code

| Skill | Use when |
|-------|----------|
| [design/architect](./design/architect/) | non-trivial work — sketch types/signatures/structure first, stay in the loop during implementation |
| [design/arena](./design/arena/) | one attempt would lock in the wrong shape — N parallel candidates, graft winners |
| [design/figure-it-out](./design/figure-it-out/) | large migration / multi-part change with no narrower playbook — auditable hypothesis loop |

## Verify — prove it works

| Skill | Use when |
|-------|----------|
| [verify/create-verification-skill](./verify/create-verification-skill/) | repo has no scripted way to prove behavior — generate a project-local driver skill |
| [verify/maintain-verification-skill](./verify/maintain-verification-skill/) | periodic audit keeping the verify skill + feature map honest |
| [verify/swarm](./verify/swarm/) | parallel coverage — fan out N workers, drain, one report |

## Review gates — pre-ship checks

| Skill | Use when |
|-------|----------|
| [review/interrogate](./review/interrogate/) | adversarial review — independent angles challenge the change |
| [review/blast-radius](./review/blast-radius/) | "what could this break" — impact beyond the diff, proven by running code |
| [review/tdd](./review/tdd/) | explicitly asked TDD, or the bug has an obvious cheap local test target |
| [review/show-me-your-work](./review/show-me-your-work/) | long/unattended runs — TSV decision trail (what, why, evidence, result) |

## Docs

| Skill | Use when |
|-------|----------|
| [docs/technical-writing](./docs/technical-writing/) | docs, RFCs, readmes, PR descriptions — Diátaxis + style standard |
| [docs/unslop](./docs/unslop/) | cut AI tells from any writing |

## Also here

- `SDLC-GIST.md` — takeaway notes (judgments + prompts) from lauren's
  ([@poteto](https://x.com/poteto))
  [Pt.1](https://x.com/poteto/article/2094457600259842065) +
  [Pt.2](https://x.com/poteto/status/2097732320606507506) posts that motivate
  this curation. Notes, not skills.

## Note on `poteto-mode` (excluded)

The individual skills above are the tools; `poteto-mode` is the foreman —
an agent style + auto-router that picks which skill/playbook fits the task
(feature, bug-fix, prototype, refactor, ship…) and enforces concise,
unslopped, verified work, so you stop thinking about which skill to invoke.
Skipped here because this repo's `compound-engineering` loop
(`ce-brainstorm → ce-plan → ce-work`) already plays orchestrator, and
`poteto-mode` is the heaviest, most harness-coupled piece. Revisit if you
want self-routing instead of manual skill selection.
