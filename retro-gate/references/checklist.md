# Checklist — gateable vs judgment-call

## 1. Grep patterns (fuzzy language to convert or drop)

Search review notes / steering files for:

```
\b(be careful|verify twice|maximally thorough|MUST ALWAYS|never forget|stay quiet)\b
\b(should|ensure|properly|correctly)\b.{0,40}\b(check|validate|verify)\b
```

Each hit → ask: `can a script decide this?` If yes → write gate. If no → rewrite as conditional routing (`for X: do Y`) or delete.

## 2. Gateable candidates

- [ ] Schema-first violation (missing field, wrong type, unversioned contract)
- [ ] Plan-first trace missing (`Plan:` ref, issue link, or plan path absent)
- [ ] Required artifact absent (screenshot/video/log for UI/perf change)
- [ ] Empty-schedule waste (>50% runs with nothing to do)
- [ ] Browser loop where API/signal exists

## 3. Pass/fail table (run before handoff)

| Case | Expected | Result |
|------|----------|--------|
| Original failure input | gate RED | |
| Fixed input | gate GREEN | |
| Unrelated path | gate silent (no fire) | |
| Missing source / invented failure | BLOCKER — do not ship gate | |

Ship only on RED + GREEN + silent. One red failure of the matrix reverts the gate.
