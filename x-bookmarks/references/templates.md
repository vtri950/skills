# Templates

## Triage table (running, one row per bookmark)

```md
| URL | verdict | artifact | reason |
|-----|---------|----------|--------|
| <source-url> | keep | skill `foo/` | weekly UI polish pain, tiny, no overlap |
| <source-url> | skip (S1 duplicate) | none | overlaps ce-plan/work |
```

## Gist skeleton (`<topic>.md`, 1 file)

```md
# <Topic> — distilled from <source link>

> One-line claim + numbers.

## Core idea
## Architecture (diagram as text if any)
## Directory structure / config (copy-paste)
## Prompts (verbatim, numbered: ingest / bootstrap / project / daily)
## Ready-made templates (table + clone commands)
## Limitations + when NOT to use
## What was skipped and why
```

## Skill skeleton

```
<name>/SKILL.md            # frontmatter + when-to-use + workflow in order + prompt templates + source link
<name>/references/checklist.md   # grep patterns + pass/fail table
<name>/references/templates.md   # drop-in configs per harness
```

Frontmatter: `name`, `description` (triggers: phrases user says), `license: MIT`, `metadata.version/source`.

## Copilot-optimize starter (portable subset, no API knobs)

- `.github/copilot-instructions.md` — stable prefix, <40 lines, static, no timestamps/file lists. Conditional routing, e.g. `For files >350 lines: call bulk_read…; for boilerplate from reference: call code_write…; for edits: diff only + ≤5 bullets`.
- `.github/instructions/*.instructions.md` — per-glob (`data/**`, `src/**/*.ts`), keeps prefix byte-stable per task type (= `defer_loading` equivalent).
- `.github/prompts/*.prompt.md` — bound output (`changed files + diff + 3-line summary, no full rewrites`).
- MCP: keep one server, disable rest for bulk tasks in Chat > Tools. Never put `ls`/date/file lists in instructions (breaks prefix cache).

## PR checklist

- [ ] Branched off `origin/main` (`feat/add-<name>`), isolated diff
- [ ] README table + layout + attribution updated, agent-agnostic wording
- [ ] No author/handle/platform refs in code/docs (grep: zero matches)
- [ ] Fresh test + one failure case run; tool gates pass locally
- [ ] `gh pr create` → URL returned; one skill per PR
