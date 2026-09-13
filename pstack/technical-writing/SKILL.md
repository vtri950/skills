---
name: technical-writing
description: |
  Write docs a stranger can actually use. Use when agent docs come back as unreadable slop mixing tutorial, how-to, reference, and explanation in one file — or README-first before building shared code, so the caller's tutorial becomes the reviewable target.
license: MIT
metadata:
  version: "1.0.0"
  source: "https://x.com/poteto/status/2097732320606507506"
  origin: "pstack Pt.2 — /technical-writing, Diátaxis (lauren / @poteto)"
---

# /technical-writing — one doc, one job

First-pass agent docs fail the same way: they try to be a tutorial, a how-to, an architectural explanation, and an API reference in one file, in mannered AI prose. Split them (Diátaxis):

- **Tutorial** — learning by doing. A newcomer builds something visible, step by step.
- **How-to** — steps for one real problem. Experienced user, no teaching.
- **Reference** — dry, complete, authoritative. APIs, flags, config.
- **Explanation** — background, design choices, tradeoffs. Illuminates, doesn't instruct.

Type this:

> `write a tutorial for <X>. tutorial only — no reference material inside.`

If a doc mixes modes, send it back naming the intruding mode ("this tutorial contains reference material — move it out").

**README-first:** before building shared code or packages, write the caller's tutorial first. It forces the developer-experience view and gives the agent a concrete target it can check its own work against — and gives you something you can actually review.
