# Templates — drop into any repo

## 1. `.github/copilot-instructions.md`

```md
---
applyTo: "**"
---
# Copilot instructions
Use bulk workers for large I/O, frontier for edits.

- For files >350 lines: call `bulk_read` with `question` + `paths`, do not open directly.
- For boilerplate/tests from a reference: call `code_write` with `spec + reference + target`.
- For edits: read with offset/limit, return diff only + ≤5 bullet summary.
- Piped `cat <file> | grep <x>` is fine. Bare `cat` on large files is not.
```

Adjust `350`, tool names, large-file list per repo.

## 2. `.github/instructions/data.instructions.md`

```md
---
applyTo: "src/data/**"
---
Use bulk_read for reads. Targeted reads only with offset/limit for line numbers.
```

## 3. `.github/prompts/read-task.prompt.md`

```md
---
mode: ask
model: gpt-4o-mini
---
Summarize via bulk tools. Bullets only, no full-file dumps. Include file:line refs.
```

## 4. `.github/prompts/edit-task.prompt.md`

```md
---
mode: agent
model: Claude Sonnet
---
Return: changed files + unified diff + 3-line summary. No full-file rewrites.
```

## 5. `.vscode/mcp.json` (keep names stable)

```json
{
  "servers": {
    "shunt-mog": {
      "command": "node",
      "args": ["tools/shunt/mcp-server.mjs"],
      "cwd": "${workspaceFolder}"
    }
  }
}
```

Rename `shunt-mog` / script path per repo, but never rename per request.
