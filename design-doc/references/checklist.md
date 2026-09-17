# Checklist — section selector + review gates

## 1. Section selector (subset only — never all)

| Section | Include when | Skip when |
|---------|--------------|-----------|
| Metadata | always (author, date, URL, signoff) | never |
| Objective (1 sentence) | always, page one | never |
| Background | always — with measured numbers | only if trivially obvious |
| Related docs | prior iterations / partner specs exist | greenfield solo work |
| Goals / Non-goals | always (impact, not internals) | never |
| Scenarios | behavior unclear from goals alone | pure infra change |
| Diagrams | >1 component or any trust crossing | single-file change |
| Glossary | internal names a newcomer won't know | all terms standard |
| Constraints | budget/client/infra shapes choices | unconstrained |
| SLOs | any performance/availability claim | no such claim |
| Monitoring / alerting | SLOs exist | no SLOs |
| Timeline | multi-milestone work | ships in one pass |
| Interfaces | user/API/file surface changes | internal-only |
| Dependencies / infra | language/storage/hosting hard to reverse | easily swapped |
| Security | attack surface or trust boundary exists | no untrusted input — still note why |
| Privacy | sensitive data handled/retained | no sensitive data |
| Legal | regulated domain or risky reuse | neither |
| Logging | long-lived service | throwaway script |
| Open / Resolved issues | always (empty only if truly none) | never |
| Alternatives considered | any credible reject exists | decision was the only option |

## 2. Vague-language grep (draft must fail these)

```
\b(performant|fast|scalable|robust|user-friendly|seamless|high-quality)\b  → demand SLO numbers
\b(should|ensure|properly|correctly)\b.{0,40}\b(handle|validate|support)\b   → demand measurable criterion
```

## 3. Pass/fail table (run before review)

| Case | Expected | Result |
|------|----------|--------|
| Zero gate-yes | no doc, one-line reason | |
| Objective >1 sentence or jargon-locked | FAIL — rewrite | |
| Background without numbers | FAIL — measure first | |
| Goal states implementation, not impact | FAIL — restate | |
| Alternatives thinner than decision with credible rejects known | FAIL — expand | |
| Diagram without editable source link | FAIL — link source | |
| Missing metadata/signoff block | FAIL — add | |
| Open issue without next step | FAIL — assign step | |
| Trivia (pagination, copy, spacing) in review scope | FAIL — cut | |
