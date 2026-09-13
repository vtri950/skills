# pstack — curated self-contained collection

SDLC subset of the upstream pstack plugin plus everything `poteto-mode`
needs to run, vendored unchanged (`SKILL.md`, `references/`, `scripts/`,
`agents/` preserved as-is). Sources of truth: `../plugins/pstack` and
`../plugins/cursor-team-kit` — re-copy from there to pick up updates.

Deliberately excluded: `bro`, `automate-me`, `typescript-best-practices`,
and the 2.3M `docs/` guide (prose, not load-bearing).

## Mode — the orchestrator

| Skill | Use when |
|-------|----------|
| [mode/poteto-mode](./mode/poteto-mode/) | working in poteto's style — trigger→skill routing, task→playbook matching (23 playbooks: feature, bug-fix, prototype, refactor, shipping, babysit…), principles enforcement |
| [mode/setup-pstack](./mode/setup-pstack/) | configure per-role model choices (overrides skill defaults) |

## Research — understand before touching code

| Skill | Use when |
|-------|----------|
| [research/how](./research/how/) | "how does X work" — walkthroughs, ownership/layering questions |
| [research/why](./research/why/) | "why is it this way" — rationale from git, PRs, tickets, docs, chat, observability |
| [research/teach](./research/teach/) | "explain it plainly" — runs `how` + `why`, weaves one clear account |
| [research/recall](./research/recall/) | "where did I leave off" — rebuild context from chat history + shared record |
| [research/reflect](./research/reflect/) | "reflect" — review transcript with parallel subagents, route learnings to skill edits |

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
| [verify/control-cli](./verify/control-cli/) | drive/inspect CLI and TUI surfaces like a user (from cursor-team-kit) |
| [verify/control-ui](./verify/control-ui/) | drive/inspect browser/Electron/web UI surfaces like a user (from cursor-team-kit) |

## Review gates — pre-ship checks

| Skill | Use when |
|-------|----------|
| [review/interrogate](./review/interrogate/) | adversarial review — independent angles challenge the change |
| [review/blast-radius](./review/blast-radius/) | "what could this break" — impact beyond the diff, proven by running code |
| [review/no-comments](./review/no-comments/) | pre-review comment hygiene — Comment Sicko kills narrating comments, keeps only non-obvious whys |
| [review/deslop](./review/deslop/) | pre-commit gate — de-slop the diff (from cursor-team-kit) |
| [review/tdd](./review/tdd/) | explicitly asked TDD, or the bug has an obvious cheap local test target |
| [review/show-me-your-work](./review/show-me-your-work/) | long/unattended runs — TSV decision trail (what, why, evidence, result) |

## Docs

| Skill | Use when |
|-------|----------|
| [docs/technical-writing](./docs/technical-writing/) | docs, RFCs, readmes, PR descriptions — Diátaxis + style standard |
| [docs/unslop](./docs/unslop/) | cut AI tells from any writing |

## Principles — the ground `poteto-mode` stands on

Every multi-step task under `poteto-mode` starts by reading these and citing
which principle drove each decision. All 21 vendored unchanged under
`principles/`:

Core: `laziness-protocol`, `foundational-thinking`,
`redesign-from-first-principles`, `subtract-before-you-add`,
`minimize-reader-load`, `outcome-oriented-execution`, `experience-first`,
`exhaust-the-design-space`, `build-the-lever`.
Architecture: `model-the-domain`, `boundary-discipline`,
`type-system-discipline`, `make-operations-idempotent`,
`migrate-callers-then-delete-legacy-apis`,
`separate-before-serializing-shared-state`.
Verification: `prove-it-works`, `fix-root-causes`,
`sequence-verifiable-units`.
Delegation: `guard-the-context-window`, `never-block-on-the-human`.
Meta: `encode-lessons-in-structure`.

## Agents

`agents/` holds the two subagent definitions the skills spawn by name —
`poteto-agent.md` (default delegate inside playbook steps) and
`comment-sicko.md` (spawned by `review/no-comments`). Your harness must
register these names or map them to its own subagent types.

## Dependency map — why each piece is here

`mode/poteto-mode` references, by name, everything below. Nothing here is
decorative:

- triggers route to: `research/how`, `design/architect`, `verify/swarm`,
  `design/arena`, `review/interrogate`, `docs/unslop`,
  `docs/technical-writing`, `review/show-me-your-work`,
  `design/figure-it-out`, `research/reflect`, `review/no-comments`,
  `research/why`, `verify/control-cli`, `verify/control-ui`,
  `review/deslop`
- Principles section mandates reading + citing the `principles/` leaf skills
- Subagents spawn as `poteto-agent` / `Comment Sicko` → `agents/`
- Model routing defaults live inline; `mode/setup-pstack` overrides per role

## Harness notes (files preserved unchanged — adapt at install time)

- Cursor built-ins referenced but not vendored: `create-skill`
  (skill authoring), `AskQuestion` (blocking questions), Cursor's own
  babysit skill (`poteto-mode` explicitly prefers its own Babysit playbook).
- `mode/poteto-mode` playbooks reference Graphite (stacks,
  merge-when-ready) in Shipping/Babysit — git-native harnesses should map
  these to their own landing flow.
- Default models (`grok-4.6-fast-xhigh`, `claude-fable-5-thinking-max`,
  `gpt-5.6-sol-max`) are inline; run `mode/setup-pstack` to re-point roles.

## Also here

- `SDLC-GIST.md` — takeaway notes (judgments + prompts) from lauren's
  ([@poteto](https://x.com/poteto))
  [Pt.1](https://x.com/poteto/article/2094457600259842065) +
  [Pt.2](https://x.com/poteto/status/2097732320606507506) posts that motivate
  this curation. Notes, not skills.
