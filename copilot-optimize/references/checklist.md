# Checklist — copilot-optimize audit

Copy-paste output table when report-only.

## A. Stability (prefix)

- [ ] `copilot-instructions.md` exists and <40 lines?
- [ ] No date / timestamp / `git log` / full `ls` output inside?
- [ ] No absolute paths (`/Users/...`)?
- [ ] Per-path split used (`applyTo:`) instead of one 200-line global?
- [ ] `.vscode/mcp.json` server names frozen?

## B. Anti-patterns (grep)

Search case-insensitive in `.github/`, `AGENTS.md`, `CLAUDE.md`:

- `verify twice|double-check|triple-check`
- `maximally thorough|exhaustive search|CRITICAL.*MUST|ENFORCED`
- `think step by step in a scratchpad|mandatory \d+-step`
- `always .* never .*` pairs that conflict — read manually
- retired model IDs, `thinking.*budget.*\d+`

Each hit: `file:line | pattern | suggested rewrite`.

## C. Routing

- [ ] Cheap-first rule written (Ask+mini for reads, Agent+frontier for edits)?
- [ ] `edit-task.prompt.md` bounds output (diff + ≤5 bullets)?
- [ ] `read-task.prompt.md` uses ask mode?
- [ ] Chat Tools menu pruned for bulk tasks?
- [ ] Large files (>350 lines) listed with bulk path?

## Verdict

- PASS: all checked
- FIXABLE: list files to write
- SKIP: needs Claude API (breakpoint, TTL, Batch, hillclimb)
