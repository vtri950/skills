# Templates — advisory rule vs hard block per harness

Principle is tool-agnostic; only wiring differs. Advisory nudges; hard block enforces. Prefer hard block for gateable checks.

## Advisory (rule text, paste into instructions)

```md
- After any review/debug fix, propose the cheapest deterministic check (lint, hook, or CI) that would have caught it.
- For worker handoffs, require definition-of-done: changed files + verify steps + required artifacts.
- Before adding a scheduled agent, audit existing cadence (15-min ≈ 100 runs/day); prefer incoming signal or API over polling.
```

## Hard block (enforcement wiring)

| Harness | Advisory location | Hard block |
|---------|-------------------|------------|
| Generic agent | `AGENTS.md` routing | CI workflow fails merge on gate RED |
| Pre-commit users | `CODING_STANDARDS.md` note | pre-commit hook exits non-zero on violation |
| Editor agents | workspace instructions file | lint rule (error severity, not warning) |
| Multi-worker setup | plan template `Definition of done:` section | reviewer rejects handoff missing files/steps/artifacts |
| Scheduled agents | schedule registry with cadence + hit-rate | disable schedule when empty-run rate >50%; require signal/API alternative |

## Minimal CI gate skeleton

```yaml
# .github/workflows/retro-gate.yml — rename per check
name: retro-gate
on: [pull_request]
jobs:
  gate:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - run: ./scripts/retro-gate-check.sh
```

```bash
#!/usr/bin/env bash
# scripts/retro-gate-check.sh — exit 1 on violation, 0 on pass
set -euo pipefail
# 1. check the mechanical condition (e.g. required artifact present)
# 2. print file:line on failure for fast fix
```

## Threshold guidance

- Keep gates cheap: seconds, not minutes; local-first.
- One gate per finding. Remove the gate if false positives exceed true catches over two weeks.
- Never gate taste (naming, copy tone, architecture preference).
