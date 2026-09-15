---
name: x-bookmarks
description: |
  Distill X bookmarks into your collection by taste — fetch, triage for pragmatic reuse, route to gist vs skill vs tool vs AGENTS.md pointer, generalize across harnesses, add in proper format. Use when user says X bookmarks, bookmark workflow, extract value from X/thread/post, add to my collection/skills, or drops an x.com link for reuse.
license: MIT
metadata:
  version: "1.0.0"
---

# X Bookmarks → Collection

Turns a pile of X bookmarks into a small collection that compounds. Distilled from two weeks of sessions: gist-from-thread, skill-vs-gist decisions, pragmatic filtering (2 of 19 kept), Claude-specific → universal generalization, and risk-gate / shunt-router tool builds.

**Core loop:** fetch → taste-triage → route → generalize → collect → verify.

## 0. Batch setup

1. Get the bookmark list (URLs + why saved). Process one URL at a time, keep a running triage table:
   `| URL | verdict: keep/skip | artifact: gist/skill/tool/AGENTS/none | reason |`
2. Rule from experience: from ~19 candidates, expect **2 keeps**. Rest is bloat or duplicates. If everything looks like a keep, the filter is broken.

## 1. Fetch — get the real content

1. `webfetch` the X URL as markdown first.
2. If prompts/steps come back empty or content lives in images/diagrams, pull the full-article URL hidden in page data / embed API before writing anything.
3. Read linked articles/repos referenced by the thread (don't summarize from the tweet alone).
4. Keep `Source:` link + local file refs for everything you produce.

## 2. Taste-triage — is it worth keeping?

Ask in this order (these are the questions that actually filtered well):

1. `Is there anything useful / productivity-enhancing here?` — strip hype first.
2. `What are the useful points?` — list claims with numbers, not adjectives.
3. `Can these rules run in another harness? Practically implementable or just theory?` — split portable 80% from vendor-locked 20%.

**Keep filter** (`references/taste.md`): all must be true.

- `frequency x pain x uniqueness` — triggers at least weekly, removes a pain felt *this week*.
- Tiny, zero-deps, composable. Triggers only on its job (e.g. after frontend work, on docs output).
- Not a duplicate of `ce-brainstorm/plan/work/compound/code-review/debug`.
- No heavy MCP / server / browser / auth maintenance unless that pain is hurting *this week*. `websearch` + `webfetch` already covers ~90%.
- Ignore star counts (`73k`, `224k`) — hype-inflated, not signal.
- Daemon ≠ skill: Node daemon (SQLite + Chroma + hooks + cloud sign-in) → thin **adapter** skill with detect-if-installed + fallback, never vendored. Pattern ≠ install: iterate→measure→keep-if-better loops get encoded into `ce-debug`/`ce-work`, not installed. 3-line prompt tweak → `AGENTS.md`, not a skill.

If it fails the filter → `skip` with one-line reason. Do not gist it "just in case".

## 3. Route — gist vs skill vs tool vs AGENTS.md

Different layer, different artifact. Pick one (at most two):

| Artifact | When | Format |
|----------|------|--------|
| **gist** (`vtri950`, public or secret) | Reference guide, future reliance, article summary, universal routing notes | 1 md file: core idea + arch diagram as text + dir structure + copy-paste prompts + templates table with clone commands + limitations + time expectations |
| **skill** (this repo) | 7-step *process*, repeatable, on-demand workflow | `SKILL.md` + `references/` (+ `templates.md`/`checklist.md`) + README row. On-demand (invoked per task), not always-on |
| **tool / primitive** (`risk-gate/`, `tools/shunt/`) | Deterministic enforcement, guardrail, router, auditor | Small bash/JS + CI wiring. Enforcement via hooks, not prompt. Cheap model (`gpt-4o-mini`/`haiku`/`gemini-flash`, temp `0.2`), tunable threshold (`SHUNT_MIN_LINES=350`) |
| **AGENTS.md pointer** | 2-line invariant applying to code + design | Conditional routing (`for files >350 lines: call bulk_read…`), never caps (`MUST ALWAYS`, `verify twice`, `maximally thorough`, fixed scratchpad) |

Examples from history: `second-brain-compiler.md` gist (reliance version + full manual linked inside); `design-de-slop`, `copilot-optimize`, `session-memory` adapter, `make-interfaces-feel-better`, `humanizer` skills; `risk-gate` action + `coverage/docs/skills-isolation/plan-link` gates + `skills-auditor`; `tools/shunt` `bulk-read`/`code-write`/MCP server + hard `>350-line` block.

**Skill-vs-gist-vs-AGENTS rule:** full process → skill. Always-on rule → `AGENTS.md` (bloats context otherwise). Manual reference agent never sees unless pasted → gist. If design happens 1x/month, gist + manual port beats skill maintenance.

## 4. Generalize — never leave it Claude-specific

Principle is tool-agnostic; only the wiring differs. Hard block beats prompt (`CLAUDE.md` instructions alone were ignored until the hook landed).

| Agent | MCP / worker | Rule (advisory) | Hard block (enforcement) |
|-------|--------------|-----------------|--------------------------|
| Claude | `bulk_read`/`code_write` MCP | `.claude/skills/` | `settings.json` `PreToolUse` |
| Copilot | `.vscode/mcp.json` (`servers`) | `copilot-instructions.md` + `instructions/*.instructions.md` + `prompts/*.prompt.md` | tool restriction / custom chat mode |
| Kiro | same MCP | `steering/` | `.kiro/hooks/*.json` |
| Codex | `config.toml` `mcp_servers.cheap-worker` | `AGENTS.md` routing | `deny read_file lines>350` |
| Cursor | `.cursor/mcp.json` (`mcpServers`) | `.cursor/rules/*.mdc` (`alwaysApply: true`) | `hooks.json` `beforeReadFile` + `beforeShellExecution` (deny + exit 2, allow `offset/limit` + piped `grep`) |

Never delegate edits (needs `offset/limit`) or reasoning/thread-safety bugs to cheap workers. Keep threshold tunable.

## 5. Collect — proper format

**Skills repo** (`/Users/vidit/side_projects/skills`):

1. Branch off `origin/main`: `feat/add-<name>`. Isolate — never bundle unrelated unmerged work.
2. Vendored skills: byte-identical + MIT attributed, segregated by origin (`compound-engineering/`, `pstack/`, `<source>/`). Ports/adapters: original `SKILL.md` with layered discipline (e.g. search→timeline→fetch), detect-if-installed, fallback to `ce-compound` `docs/solutions/`, privacy rules.
3. README: table row + layout tree + attribution. Agent-agnostic language (no Claude-Code specifics).
4. Neutral wording: no author names, handles, or platform links in code/docs — generic `review-noise` / `schema-first` / `plan-first` terms. PR body same.
5. Commit, push, `gh pr create`, return URL. One skill per PR; stack with base = prior PR branch only if dependent.

**Gists:** one file per gist in `vtri950`, public unless prompts/secrets. Include source link + what was skipped + why.

## 6. Verify before handoff

- Skills: fresh test + one failure case (missing source/owner). Missing source = blocker, never invent.
- Tools: run gates locally (`risk-gate.sh`, `coverage/docs/skills-isolation/plan-link`), confirm pass/fail matrix. CI pitfalls seen: `yq` rejects `// empty` (use `[]?` alone); fallback order must match config order; plan gate needs `Plan:`/`Closes #`/plan path or `chore` label + `GITHUB_TOKEN` passed to the step or label exemption silently fails.
- Report: `kept N / skipped M` + per-keep artifact path + PR/gist URL + what was deliberately *not* kept.

## Source hierarchy (trust order)

Live tracker/source > exported snapshot > old summary > remembered detail. On conflict, stop and re-read — never claim current/done/approved from memory.

## References

- `references/taste.md` — full keep/skip filter with examples of each skip reason.
- `references/templates.md` — triage table, gist skeleton, skill skeleton, PR checklist.
