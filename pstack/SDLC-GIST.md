# pstack Pt.1 + Pt.2 — SDLC notes worth keeping (not skills)

> My own distilled notes. The `pstack/` skills themselves are the author's own words, lifted verbatim — this gist is the separate takeaway sheet.
Source: lauren (@poteto) — "The Complete Guide to pstack" Pt.1 (verification) + Pt.2 (research/planning/prototyping/architecture).
Distilled to judgments and workflow knowledge that don't warrant a skill file but change how you run SDLC with agents.

## The two theses

1. **Verification is all you need (Pt.1).** If an agent can't verify its own work, you stay the bottleneck all day. A good verification setup 100–1000x's the whole team — including non-engineers.
2. **Supervise someone smarter than you (Pt.2).** Frontier models out-code you on slices but fail as: (a) misunderstood intent, (b) missing context. Your job shifts to priming context + architecture + data structures; agents fill implementation.

## Judgments (genuinely useful, easy to lose)

- **Build the Lever: tools > markdown.** A small CLI the agent calls beats pages of instructions — fewer tokens, reproducible, testable.
- **Your stack is your ceiling.** Web/Electron (CDP) and simulator-backed mobile give agents eyes and hands. If the stack is hard to drive/debug, agent productivity caps early — choosing (or building) debuggability is worth real money.
- **Tests ≠ verified.** Verified = the agent ran the app and showed trace/screenshot/video proof. Write every plan task as "run X, observe Y," not "add test Z."
- **Cloud agents > local worktrees** for parallelism. Worktrees top out (~10 local agents, disk/CPU); snapshotted cloud machines scale to hundreds and keep the coordinator's context clean.
- **Coordinator pattern.** Main bot supervises, cloud agents execute. Keeps the planner's window clean and lets you use different model families per subtask.
- **Volume matters now — but only with guardrails.** PR count was vanity pre-agents; with verification + maps + small PRs it becomes the gardener's lever (refactor foundations while hundreds land daily).
- **Abstract plans are illusion of progress.** Long plan docs feel productive but lack substance. Never adversarially review an abstract plan — agents invent theoretical risks and gold-plate against phantoms. Answer open questions with prototypes instead.
- **Scrap signals are empirical, not aesthetic.** Same workaround in unrelated call sites, or types needing `any`/forced casts → architecture is wrong. Throw it away, don't patch.
- **Plans rot — delete them.** Commit big plans temporarily so parallel agents see WIP, delete on landing. Codebase is the memory; Feature Map is its cheap projection (preferred over generic vector-DB memory).
- **Maintain verification like infra.** Stale maps/tooling silently degrade every downstream agent. Daily refresh; consider oncall ownership on teams.

## Copy-paste workflow prompts (Pt.2 examples, de-pstacked)

- Ambiguous bug: `investigate why <symptom>. give me what we know, what data you used, and best hypotheses.`
- New boundary: `we need <subsystem>. ground current architecture first, answer open questions with prototypes. let me review before proceeding.`
- Big migration: `plan migration of <X> into small verifiable PRs. each PR has visual-regression + live verification steps. final result 100% identical to original — bugs included.`
- Slack report, context-rich: `do it` / `repro this with the control CLI. if it repros on main, fix it and show video as proof.`

## Small references kept for precision

- **Diataxis (via /technical-writing):** Tutorial (learn by doing) / How-to (one real problem) / Reference (dry API) / Explanation (why/tradeoffs). One doc, one mode — kills AI slop that blends all four.
- **Agent-friendly CLI properties:** composable deep modules, subcommands for gradual disclosure, `--dry-run` on destructive ops, errors that say what to do next, rich `--help`, JSON output.
- **Feature Map shape:** `references/features/README.md` (index) + one file per feature (purpose, user path, preconditions, driving commands). Agent-bootstrapped, daily-refreshed.
- **Cross-model judging:** sketch authors and the judge must differ; weak-model failure modes are part of the rubric.
