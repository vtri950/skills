---
name: design-doc
description: |
  Scope, draft, and drive review of software design docs — should-you-write gate, investment sizing, penalty-for-being-wrong filter, section checklist, measurable SLOs, alternatives and open issues, signoff trail. Use when user says design doc, technical spec, RFC, needs architecture review, or asks what belongs in a design document.
license: MIT
metadata:
  version: "1.0.0"
  source: "https://refactoringenglish.com/excerpts/write-an-effective-design-doc"
---

# Design Doc: Scope It, Draft It, Drive It Through Review

Writing code is cheap; recovering from the wrong architecture is not. A design doc forces hard decisions before implementation and coordinates them across teammates. This skill scopes the doc to what matters, drafts only the sections that earn their place, and drives it to signoff.

**Core loop:** gate → size → filter → draft → number → review.

**Boundary:** this skill scopes and reviews the doc. The implementation-plan format (U-IDs, test scenarios, task breakdown) belongs to your planner workflow — do not re-plan here. Point the doc at the plan; don't duplicate it.

## When to use

- User is starting anything that outlives a single session: multi-person work, cross-team surface, production system, ambiguous requirements
- Agent is asked to write a design doc, technical spec, or RFC
- A draft exists but review stalls on trivia while hard decisions go unexamined
- User asks what belongs in a design doc or how much process a change deserves

## Workflow — run in order

### 1. Gate: should you write one?

Ask these six. Zero yes → skip the doc, say so in one line. One yes → one-pager (objective + background + goals/non-goals + open issues). Two or more → full doc.

1. Will multiple people coordinate work to implement it?
2. Will it take more than ~3 months of full-time work?
3. Will it run in production for years?
4. Does it involve cross-team collaboration?
5. Are goals or requirements ambiguous?
6. Are there catastrophic risks preventable at design time (security, legal, data loss)?

### 2. Size the investment

One-pager → 50-page multi-team signoff. State the chosen size up front; zero is valid. Match cost to risk, deadline, and culture — never to template completeness.

### 3. Filter: penalty for being wrong

For each candidate decision ask: `if we get this wrong, is recovery a rewrite or an afternoon?`

- Doc it: language, storage backend, service boundaries, public contracts, security posture, legal exposure — wrong means stuck.
- Skip it: pagination size, copy wording, button placement — wrong means a few hours and user feedback settles it. Never spend review cycles arguing these.

### 4. Draft from the checklist, subset only

Pick sections from `references/checklist.md` — never all of them. Defaults that earn their place most often: metadata, one-sentence objective, background with numbers, goals/non-goals, alternatives considered, open issues. Add SLOs, security, privacy, legal, interfaces, deps, timeline only when step 3 says the penalty is real.

Draft rules:

- Objective is one sentence, plain language, on the first page. A reader with zero context must understand the doc from page one alone.
- Background carries numbers, not adjectives: baseline, delta, share of cost (e.g. loads 100ms → 600ms; lookups 80% of load; 95% of lookups hit 3% of rows).
- Goals state user/team impact, never implementation internals. Non-goals name the tempting out-of-scope items explicitly.
- Diagrams show data flow, components, and trust crossings. Link the editable source (code or drawing file) so the diagram can be revised, never a frozen image.
- Alternatives considered: brief lines on strong rejects only — what was appealing, why it lost. A healthy alternatives section rivals the decision in length.
- Timeline milestones deliver stakeholder-visible artifacts in series (dummy-data UI before plumbing), so misunderstood requirements surface early.

### 5. Number the vague

Convert every adjective goal into an SLO: uptime/availability %, latency percentile + bound, scale volume. Then state how each is measured in production (what pages, what thresholds). Unmeasurable SLO → open issue, not a goal.

### 6. Drive review to signoff

- Metadata block on every doc: author, created date, authoritative URL, approver + signoff date. See `references/templates.md`.
- Open issues appendix: problem, options seen, immediate next step. Never merge with known-unknowns undocumented.
- Resolved issues: move the entry, keep the full discussion, record the decision first.
- Close the loop: every approver named in metadata signs with a date. No signoff → still a draft.

## Prompt templates

**Scoping:**
> Run the should-you-write gate: [change]. Score the six questions, recommend skip / one-pager / full doc, and state the investment size. List which decisions pass the penalty-for-being-wrong filter and which don't.

**Drafting:**
> Draft [one-pager/full] design doc for [change] using sections [list from checklist]. Objective in one sentence on page one; background with measured numbers; goals as impact; alternatives as brief rejects; open issues with next steps. Subset only.

**Review prep:**
> Audit this draft: flag vague goals without SLOs, missing metadata/signoff, frozen diagrams, alternatives thinner than the decision, and trivia crowding out hard choices. Output table: finding | section | fix.

## AGENTS.md pointer to pair with this skill

```md
- For design docs, invoke design-doc skill first. Gate writing, subset sections by penalty-for-being-wrong, require numbered background + measurable SLOs + signoff metadata.
```

## Source

Distilled from a design-doc writing guide (write-gate questions, penalty filter, section catalog, signoff discipline) plus multi-agent field notes (alternatives section rivals the decision; purpose plus trade-offs plus failure modes). Full URL in frontmatter `metadata.source`.
