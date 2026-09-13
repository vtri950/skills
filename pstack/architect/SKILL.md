---
name: architect
description: |
  The author's design and execution sections, verbatim: the prototyping playbook, the /architect skill, the planning playbook, the four workflow examples, and the closing.
license: MIT
metadata:
  version: "2.0.0"
  source: "https://x.com/poteto/status/2097732320606507506"
  origin: "pstack Pt.2 — lifted verbatim from lauren / @poteto (light markdown formatting only)"
---

# Measure a hundred times, cut once

> Lifted verbatim from [The Complete Guide to pstack Pt. 2](https://x.com/poteto/status/2097732320606507506). This skill holds the article's prototyping, architecture, planning, and workflow sections.

When planning, two of the most common mistakes I see are:

Accepting the agent's first design.

Overcooking the plan without empirical evidence.

When humans wrote code, we often collaborated with each other over design documents. These were docs that talked about high level architecture, alternatives considered, tradeoffs, and any unusual implementation notes. It was very common to go through multiple iterations of these docs before you landed on a settled design.

With agents, while we can skip the ceremony of the design doc, I often see the mistake of accepting the first thing the agent gives back to you. With pstack, we can instead take the "measure twice, cut once" approach to its limit, using parallel agents.

We do this by using the prototyping playbook.

In pstack, playbooks aren't skills, but reference files inside of `/poteto-mode`. These playbooks are conditionally loaded (for token efficiency) depending on the type of task you're working on. These 23 playbooks (as of 0.15.0) each contain a workflow that I use when I'm doing a task.

Unlike skills, playbooks are automatically used by the agent as part of `/poteto-mode`. For example:

`/poteto-mode prototype a few options for the new dropdown menu`

`/poteto-mode fix this bug`

`/poteto-mode eval this skill change`

Prototyping is one of my favorite pstack playbooks. It gives you many attempts at a goal and helps the agent reason about the best option. This is useful not just for visual prototyping, but also prototyping different solutions for features, bug fixes, and so on.

`/poteto-mode prototype a few options for <feature request>. use /control-app* and take videos/screenshots for me to review and choose from`

\* note: `/control-app` is the verification skill we created in Part 1

When prototyping visual changes, the agent builds throwaway sketches in your app or in a scratch directory. If it is testing a UI interaction, it puts two or three variations behind a simple switcher. Then it drives the interaction with the `/control-app` skill, takes screenshots of each variant, and measures the actual timing or layout.

Prototyping is planning, but with code. It allows agents the freedom to explore the problem space, and to give them a chance to surprise you with something you wouldn't have thought of yourself. Prototypes allow agents to answer their own questions with empirical evidence instead of waiting for my input.

## Architecting bigger changes

As an engineer in the agentic era, it's more important to spend my time on architecture, choosing the right data structures, and thinking about how the systems I build will work together. My agents fill in the implementation details.

Another useful skill that pstack ships with is `/architect`. It structures design into distinct, disciplined phases:

Ground the problem. The agent runs `/how` and `/why` over the affected systems to build an accurate mental model of existing ownership and constraints.

Sketch. The agent enters an architecture arena. It spawns independent candidate runners in parallel, often across different model families. Each runner receives the grounding brief and drafts a complete design package: the caller's usage sketch, the core type definitions, public function signatures, and a concise rationale. These are usually done by sketching out just the type signatures, that derive from how we want call sites to look like. Each runner must evaluate interface depth, examine failure modes on weak models, and screen against our catalog of design red flags.

Cross-judge and Synthesize. A cross-judge agent using a different model than the main agent evaluates the candidates against a strict rubric.

Implement against the sketch. The agent replaces the sketch's placeholder bodies with real logic. If the agent discovers during implementation that a function needs unexpected parameters or extra state, it surfaces the discrepancy.

Scrap when the design is wrong. If during implementation we find that the sketches were wrong, the agent throws it all away and starts over.

The point here is to give the agent a self contained mini-loop where it can synthesize multiple competing designs from different model families into one optimal approach, and take care to be rigorous and not be afraid to throw its design away if it turns out that the architecture it came up with is wrong based on empirical proof. If the same workaround appears across unrelated call sites, or if the types require escape hatches like `any` or forced casts, that is empirical proof that the architecture is wrong.

`/architect this new <feature request>`

The big lesson here is that it's far more effective to plan with code using `/poteto-mode` prototyping and `/architect`.

It's also why I never bother with reviewing abstract plans adversarially. The agents start hallucinating theoretical risks, and inventing complex edge cases to protect against problems that will never happen. Don't overcook your plans when they're still abstract: let the agent answer open questions on its own through prototyping and verifying its own work.

## Okay but I really want a planning doc

While pstack doesn't come with a planning skill, it does ship with a multi-phase planning playbook. I typically use this after the agent has come up with a design I'm happy with, as a way to create a tactical execution plan.

`/poteto-mode turn this design into a plan`

Every single task in the plan is structured around proof and verification. The playbook tells agents that tests alone are not sufficient verification. It's verified only when it has actually run the code and verified that it works.

Every plan is checked by an automated script that validates its structure and formatting. Once approved, the plan executes item by item. Each PR is small, self-contained, and easily reviewed.

For really large projects (like one that might take me a whole week), I may sometimes decide to commit the plans temporarily to the codebase so that other agents are aware of the work in progress. But I typically delete them when I'm done so I don't leave the codebase in a state of confusion. I don't find it valuable to keep plans around permanently.

## The workflow in practice

To see how all of these pieces fit together, let us walk through three concrete examples of how I prompt these workflows.

Example 1: Researching an ambiguous bug

When an issue appears in production and the root cause is unclear:

`/poteto-mode investigate why background workers periodically fail with timeout errors. give me a breakdown of what we know, what data you used, and your best hypotheses.`

The agent explores the code, checks metrics and historical commits in parallel, and gives you its best educated guesses on where the problem might lie.

Example 2: Designing a new service boundary

When introducing a new subsystem that other modules will depend on:

`/poteto-mode we need to add rate limiting for external webhooks. /architect this first, and answer any open questions with prototypes. let me review before proceeding.`

The agent grounds the existing webhook architecture, spins up competing design runners across multiple models, benchmarks with throwaway prototypes, and produces a clean, verified interface.

Example 3: Executing a multi-PR migration

When executing a complex refactor across many files:

`/poteto-mode create a plan to migrate our entire UI library to StyleX. break the migration into small, verifiable PRs. each PR must have its visual regression tests and live verification steps. i want the final result to be 100% identical compared to the original - bugs included`

The agent breaks the work into independent steps, writes an auditable checklist, and prepares each unit so that it can be built, verified, and landed safely.

Example 4: Fix stuff people report on Slack

If you've ever seen me on Slack in one of our issues or feedback channels, you'll probably have seen these classics:

\# thread already has sufficient context

`/poteto-mode do it`

`/poteto-mode repro this with /control-app. if it repros on main, fix it and show me a video as proof`

Many of the skills I've talked about here are already automatically used by `/poteto-mode`, so the vast majority of times you can just use `/poteto-mode` and move on with your life!

## The art of planning

Plan Mode is often used as a way to convince yourself that the agent is going to do the right thing. But the reality is that abstract plans only give you the illusion of progress. A long and lengthy plan makes it look like you and your agent were very productive, but it's probably lacking in substance.

pstack gives you tools to combine thorough investigation, empirical evidence, and rigorous verification. When you plan this way, engineering with agents stops feeling like a gamble. It becomes predictable and repeatable.

Thanks for reading, and stay tuned for Part 3!
