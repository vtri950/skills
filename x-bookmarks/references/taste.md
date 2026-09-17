# Taste filter — keep vs skip

Derived from the "2 of 19 kept" session. All keeps must pass every K-rule; one failed S-rule is enough to skip.

## Keep (all required)

- **K1 weekly trigger:** invokes at least weekly in real work.
- **K2 felt pain:** removes a pain felt *this week*, not hypothetical.
- **K3 unique:** not covered by `ce-brainstorm/plan/work/compound/code-review/debug` or `websearch`+`webfetch`.
- **K4 tiny:** zero-deps, composable, triggers only on its job.

Kept examples: `make-interfaces-feel-better` (post-frontend polish, zero-deps), `humanizer` (docs/posts rewrite pass), `design-de-slop` (7-step UI process), `copilot-optimize` (stable prefix + audit + cheap-first routing), `session-memory` adapter (layered query discipline, not daemon).

## Skip (any one suffices)

- **S1 duplicate:** `addyosmani/agent-skills`, `mattpocock/skills`, `oh-my-hermes` → duplicate `ce-plan/work/review`. Two competing processes = bloat.
- **S2 prompt-sized:** `i-have-adhd` → 3 lines in `AGENTS.md` ("action first, numbered steps, no preamble"), not a skill.
- **S3 heavy-ops:** `codebase-memory-mcp`, `SkillClaw`, `Minions`, `Composio`, `Browser Harness` → MCP/server/browser/auth cost. Only when repo thrash / parallel-job chaos / real app actions hurt *this week*.
- **S4 covered:** `Agent-Reach`, `youtube-full`, `Defuddle` → `websearch`+`webfetch` covers 90%. Only for daily social/video research.
- **S5 niche-bloat:** `OpenMontage`, `Resemble Detect`, 818-skill cybersecurity packs → niche, nukes context. Pull on-demand.
- **S6 daemon:** `claude-mem` (Node + SQLite + Chroma + hooks + cloud sign-in) → never vendor (rots). Ship thin adapter: detect-if-installed, else fallback to `ce-compound` `docs/solutions/`, plus privacy rules.
- **S7 pattern-only:** `loopy` (iterate→measure→keep-if-better→stop) → steal the loop into `ce-debug`/`ce-work`, don't install catalog.
- **S8 hype:** star counts (`73k`, `224k`) as selection signal. Ignore.

## Wording hygiene (hard rule, no exceptions)

Shipped artifacts must never mention platform links or refs — no post/status URLs, no handles, no author names, no "bookmark batch" / "thread" / "tweet" provenance. This covers `SKILL.md` (including frontmatter and source sections), `references/`, README rows, PR bodies, and gists. Terms like `review-noise`, `schema-first`, `plan-first` over attributions. Non-platform source links (articles, repos, docs) may stay in frontmatter `metadata.source` only. Provenance beyond that lives in working notes, never in the artifact. Verify with grep for `x.com`, `twitter.com`, `@handle`, `bookmark`, `thread`, `tweet` before handoff — any hit outside this skill's own pipeline docs is a blocker.
