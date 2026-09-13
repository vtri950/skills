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
| [how](./how/) | "how does X work" — walkthroughs, ownership/layering questions |
| [why](./why/) | "why is it this way" — rationale from git, PRs, tickets, docs, chat, observability |
| [teach](./teach/) | "explain it plainly" — runs `how` + `why`, weaves one clear account |
| [recall](./recall/) | "where did I leave off" — rebuild context from chat history + shared record |

## Design — shape before code

| Skill | Use when |
|-------|----------|
| [architect](./architect/) | non-trivial work — sketch types/signatures/structure first, stay in the loop during implementation |
| [arena](./arena/) | one attempt would lock in the wrong shape — N parallel candidates, graft winners |
| [figure-it-out](./figure-it-out/) | large migration / multi-part change with no narrower playbook — auditable hypothesis loop |

## Verify — prove it works

| Skill | Use when |
|-------|----------|
| [create-verification-skill](./create-verification-skill/) | repo has no scripted way to prove behavior — generate a project-local driver skill |
| [maintain-verification-skill](./maintain-verification-skill/) | periodic audit keeping the verify skill + feature map honest |
| [swarm](./swarm/) | parallel coverage — fan out N workers, drain, one report |

## Review gates — pre-ship checks

| Skill | Use when |
|-------|----------|
| [interrogate](./interrogate/) | adversarial review — independent angles challenge the change |
| [blast-radius](./blast-radius/) | "what could this break" — impact beyond the diff, proven by running code |
| [tdd](./tdd/) | explicitly asked TDD, or the bug has an obvious cheap local test target |
| [show-me-your-work](./show-me-your-work/) | long/unattended runs — TSV decision trail (what, why, evidence, result) |

## Docs

| Skill | Use when |
|-------|----------|
| [technical-writing](./technical-writing/) | docs, RFCs, readmes, PR descriptions — Diátaxis + style standard |
| [unslop](./unslop/) | cut AI tells from any writing |

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
