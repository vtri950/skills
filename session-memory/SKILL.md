---
name: session-memory
description: >-
  Recall work from previous sessions via claude-mem when installed, else fall
  back to ce-compound docs/solutions. Use when asked "did we already solve
  this?", "how did we do X last time?", or "what happened last week?".
argument-hint: "[question about previous sessions]"
metadata:
  visibility: exported
---

# Session Memory — Recall Previous Sessions

Answer questions about **previous** sessions, not the current conversation.
This skill is an adapter: it uses the `claude-mem` service when the user has
installed it, and falls back to the repo's own `docs/solutions/` (written by
`ce-compound`) when not.

Do not vendor, reimplement, or run the claude-mem installer yourself.
`claude-mem` is a runtime service (Node daemon, SQLite, vector DB, hooks) —
see Decision below. The user installs and owns it; you only query it.

---

## Step 0: Detect

Check the host's current tool list for the claude-mem MCP tools:
`search`, `timeline`, `get_observations`, `get_tool_uses`.

- **Tools present** → follow Layered Workflow below.
- **Tools absent** → follow Fallback below. Mention once (not every turn)
  that persistent memory is available via `npx claude-mem install --ide
  opencode`, then proceed with the fallback. Never run the installer
  unprompted — it provisions accounts, keys, and background services.

---

## Layered Workflow (claude-mem present — ALWAYS follow)

Never fetch full details without filtering first. ~10x token savings.

### 1. Search — get index with IDs (~50-100 tokens/result)

```
search(query="<keywords>", limit=20, project="<project>")
```

Optional filters: `type` (`observations` | `sessions` | `prompts`),
`obs_type` (`bugfix,feature,decision,discovery,change`),
`dateStart` / `dateEnd` (`YYYY-MM-DD`), `offset`, `orderBy`
(`date_desc` default | `date_asc` | `relevance`).

### 2. Timeline — context around interesting results

```
timeline(anchor=<id from step 1>, depth_before=3, depth_after=3, project="<project>")
```

Or anchor automatically: `timeline(query="<keywords>", ...)`.
Returns observations, sessions, and prompts interleaved chronologically.

### 3. Fetch — full details ONLY for filtered IDs

Pick relevant IDs from steps 1-2, discard the rest. Batch in one call:

```
get_observations(ids=[<id>, <id>])
```

Full observations run ~500-1000 tokens each — never fetch unfiltered.

### 4. Raw tool I/O — only when step 3 was not enough

Observations are summaries. When the answer needs literal bytes (exact
diff, exact command output), and only then:

```
get_tool_uses(ids=["<tool_use_id>"])
```

Raw bodies can run to 64 KB each — you should need this layer ~5% of the time.

---

## Fallback (claude-mem absent)

1. Search the repo's `docs/solutions/` (the `ce-compound` archive) with the
   host's file-search tools for keywords from the question.
2. If found, answer from the matching solution file(s) and cite paths.
3. If not found, say so plainly — do not invent previous work.

---

## Privacy

Session memory accumulates raw output. Keep credentials, tokens, auth
headers, and connection strings out of anything shown, written, or
committed — write `<REDACTED>` in their place. If the user marks content
private/do-not-store, respect it for the rest of the session and do not
quote it back from memory results.

---

## Decision record (why an adapter, not a vendored port)

`claude-mem` ([thedotmack/claude-mem](https://github.com/thedotmack/claude-mem),
Apache-2.0) is a managed runtime — installer, background worker, SQLite +
Chroma stores, lifecycle hooks, cloud sync with account sign-in — not a set
of markdown files. Vendoring its internals here would rot on every upstream
release and violate this repo's file-based, zero-dependency rule.

Upstream already supports opencode natively
(`npx claude-mem install --ide opencode`), so there is nothing to port at
the install layer. What needed porting was the *query discipline* — the
search → timeline → fetch → (rarely) raw-I/O layering from upstream's
`mem-search` skill — which is distilled above. If upstream changes its
tools, update this one file; nothing else in the repo is coupled to it.
