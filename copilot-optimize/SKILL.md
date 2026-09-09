---
name: copilot-optimize
description: |
  Optimize any GitHub Copilot setup for lower cost and equal-or-better quality.
  Use when user says optimize copilot, reduce tokens, fix instructions, audit prompts,
  or speed up Copilot Chat / Agent mode. Scans .github/copilot-instructions.md,
  .github/instructions/, .github/prompts/, .github/agents/, AGENTS.md, and
  .vscode/mcp.json, then applies three fixes: stable cached prefix, prompt
  anti-pattern removal, and cheap-first model/mode routing.
license: MIT
metadata:
  version: "1.0.0"
---

# Copilot Optimize: cut cost without losing quality

Three levers only. Apply in order. Report-only unless user says `fix`.

Copilot has no API knobs (no cache breakpoint, TTL, effort param, Batch API).
You optimize what you control: instruction stability, instruction quality, tool list, model/mode routing.

## Phase 0 — Inventory (always)

Find these files if they exist, else note missing:

1. `.github/copilot-instructions.md`
2. `.github/instructions/*.instructions.md`
3. `.github/prompts/*.prompt.md`
4. `.github/agents/*.agent.md`
5. `AGENTS.md`, `CLAUDE.md`, `.cursor/rules/*`
6. `.vscode/mcp.json`, `.vscode/settings.json`

List large files: `wc -l` top 10. Note any file >350 lines — these are bulk-read candidates.

## Phase 1 — Stable prefix (cache equivalent)

Copilot caches server-side on exact prefix. Keep it stable:

- [ ] One short global file (`.github/copilot-instructions.md`, <40 lines). Static text only.
- [ ] Per-path files (`.github/instructions/*.instructions.md` with `applyTo:`) instead of one giant file.
- [ ] Order: static rules first, volatile content last or not at all.
- [ ] FAIL if instructions contain: current date/timestamp, `git log` output, full file listings, absolute user paths.
- [ ] FAIL if tool list changes per request (reordered MCP servers, renamed tools).

Fix: move volatile lists into tool output or prompt input. Freeze `.vscode/mcp.json` server names.

## Phase 2 — Prompt audit (anti-patterns)

Scan all files from Phase 0. Flag and remove:

1. **Verification rituals:** `double-check`, `verify twice`, `validate three times`
2. **Thoroughness boosters:** `be maximally thorough`, `CRITICAL.*MUST ALWAYS`, `ENFORCED.*not advisory`, ALL-CAPS commands
3. **Mandatory procedures:** fixed N-step scratchpads, `think step by step in a scratchpad`, `always do X before Y` rituals
4. **Stale examples:** few-shots that demo long reasoning chains for simple tasks
5. **Contradictory rules:** two rules that conflict (e.g. `never edit directly` vs `always fix immediately`)
6. **Dated config:** old thinking budgets, retired model names

Rewrite rule: conditional + scoped, not emphatic. Example:

Before:
> MANDATORY (ENFORCED): Do not read files >350 lines directly. MUST use bulk_read.

After:
> For files >350 lines: call `bulk_read` with `question` + `paths`, do not open directly. For edits, read with offset/limit.

## Phase 3 — Cheap-first routing (effort equivalent)

No `effort` param in Copilot. Route by mode + model picker + prompt frontmatter:

- Ask + cheap model (`gpt-4o-mini`, `gemini-flash`): Q&A, summaries, bulk reads
- Agent + frontier (`claude-sonnet`, `gpt-5`): edits, debugging, architecture

Enforce in files:

1. `.github/copilot-instructions.md` states the routing in 4 lines (see `references/templates.md`).
2. `.github/prompts/edit-task.prompt.md` has `mode: agent` + `model:` + bounded output (`diff + ≤5 bullets, no full-file rewrites`).
3. `.github/prompts/read-task.prompt.md` has `mode: ask` + cheap model.
4. MCP: keep only needed servers in `.vscode/mcp.json`. Tell user to disable unused tools in Chat > Tools menu for bulk tasks (defer-loading equivalent).

Never delegate: targeted edits needing line numbers, debugging, arch decisions.

## Phase 4 — Apply + validate

If user said `fix` / `apply`:

1. Write `.github/copilot-instructions.md` (create dir if missing).
2. Split oversized globals into `.github/instructions/<area>.instructions.md` with `applyTo` globs.
3. Add the two prompt files from templates.
4. Leave `.vscode/mcp.json` server names unchanged; only remove dead servers.
5. Validate: 5 reads + 5 edits in Copilot Chat. Pass = 0 direct opens of >350-line files, responses are diff + bullets.

If report-only: output table `file | line | anti-pattern | rewrite`.

## What NOT to promise

Explicit cache breakpoints, `max_tokens: 0` pre-warm, 1-hr TTL, Batch API, auto hillclimb search. Those need Claude API access. In Copilot, approximate with stability + routing above.

## Install on any repo

```bash
# portable Agent Skill layout — works in Copilot, Claude, Cursor
cp -r <this-dir> <target-repo>/.github/skills/copilot-optimize/
# or legacy global path per assistant docs
```

Then in Copilot Chat: `optimize my copilot setup` or `run copilot-optimize fix`.
