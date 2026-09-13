---
name: technical-writing
description: |
  The author's readme-driven development section, verbatim: working backwards from a tutorial, the /technical-writing skill, and the Diátaxis framework.
license: MIT
metadata:
  version: "2.0.0"
  source: "https://x.com/poteto/status/2097732320606507506"
  origin: "pstack Pt.2 — lifted verbatim from lauren / @poteto (light markdown formatting only)"
---

# Working backwards

> Lifted verbatim from [The Complete Guide to pstack Pt. 2](https://x.com/poteto/status/2097732320606507506).

Once you understand the problem, how do you specify the solution?

In my opinion, most harnesses that have plan modes tend to over-specify implementation details and under-specify everything else. That's why in pstack, I cheekily said that "I don't believe in planning". The truth is that I do plan, but I do so through code.

For certain kinds of work, like creating shared code or packages that others will use, I am a big believer in readme driven development. If you're not familiar with it, it's a technique of development that was popular back in the day, where you start with crafting your readme first. This forced you to put on your developer experience hat, where you start with describing the APIs to a hypothetical user, and work backwards to the implementation and architecture.

For example, when I was building Dune, our in-house client framework for desktop apps, I started by first writing a tutorial for it, so I could understand what it would be like to build an app with it. Or at least I tried to. It was real a struggle getting the agent to produce anything good or readable. So I had to first spend some time sharpening my knife, by creating the `/technical-writing` skill.

The first pass of the readme without the `/technical-writing` skill was painful to read because it mixed up different goals. It tried to be a tutorial, a how-to guide, an architectural explanation, and an API reference all in the same document, written with your usual AI slop and mannered prose.

`/technical-writing` uses the Diátaxis framework to separate documentation into four distinct modes:

Tutorial: Learning by doing. A lesson that leads a newcomer through a series of steps to build something visible.

How-to guide: Steps to solve a specific, real-world problem for an experienced user.

Reference: Dry, complete, authoritative technical descriptions of machinery, APIs, and configuration flags.

Explanation: High-level discussion that clarifies and illuminates background, design choices, and tradeoffs.

It also uses `/unslop`, so it produces documentation that is very readable.

Writing a plan this way is very helpful because it also gives your agents a concrete target and goal that it can check its own work against. And of course, it's also much easier to understand what exactly the agent is going to build.

Many of pstack's skills compound here in the design phase. For example:

(1) `/recall my work fixing virtualization bugs and perf issues from the past 7 days. use /how and /why to understand how our current virtualization implementation works.`

(2) `then use /poteto-mode planning and /technical-writing to come up with a new virtualization engine that categorically eliminates flickering and jittering. let's start by writing a tutorial on how i would use this new package to virtualize a React app`

(3) `after you write the plan, /teach me and prove to me why this new approach is superior to our current engine`

The technique here is really about drawing out interesting and rich context that gives your agents the ability to see the problem the same way you do - not just as a small slice:

The first part of the prompt recalls relevant past and present context about how virtualization is implemented in my app.

The second part guides the agent to use that context, such as bugs it has fixed before, to come up with a new design that eliminates those problems entirely.

The final piece is asking your agent to prove to you that this new package is superior. This is where high quality tools like verification skills are important to have.
