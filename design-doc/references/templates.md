# Templates — drop-in doc skeleton

## Metadata block (every doc)

```md
- **Author**: <name> (<email>)
- **Created**: <YYYY-MM-DD>
- **Source**: <authoritative URL>
- **Status**: Draft | In review | Approved
  - <approver> signed off, <YYYY-MM-DD>
```

## One-pager skeleton

```md
# <Short, distinctive title>

## Objective
<One sentence, plain language.>

## Background
<Why now. Baseline → delta with numbers.>

## Goals
- <User/team impact, not internals.>

## Non-goals
- <Tempting out-of-scope item + why excluded.>

## Open issues
- <Problem> / Options: <a, b> / Next step: <owner + action>
```

## Full-doc additions (append only what the selector keeps)

```md
## Alternatives considered
- <Rejected option> — appealing because <x>; rejected because <y>.

## SLOs
- <Percentile> <metric>: <=<bound> (measured via <method>; pages <who> at <threshold>)

## Timeline
- **Milestone 1 (<date>)**: <stakeholder-visible artifact, dummy data OK>
- **Milestone 2 (<date>)**: <real data / enforcement / production>
```

## Open-issue entry

```md
### Open: <title>
<Problem in 2-3 sentences, with cost-of-deciding numbers if known.>
**Options**: <a> / <b>
**Next step**: <who does what by when>
```

On resolve, prepend `**Decision**: <choice + rationale>.` and move under `## Resolved issues`, keeping the full entry.
