---
name: teach
description: |
  The author's research sections, verbatim: supervising someone smarter than you, the indirect prompt, and the /teach skill (with /how and /why).
license: MIT
metadata:
  version: "2.0.0"
  source: "https://x.com/poteto/status/2097732320606507506"
  origin: "pstack Pt.2 — lifted verbatim from lauren / @poteto (light markdown formatting only)"
---

# Research, planning, prototyping, and architecture with pstack

> Lifted verbatim from [The Complete Guide to pstack Pt. 2](https://x.com/poteto/status/2097732320606507506). This skill holds the article's opening and research sections. Design and execution sections live in `technical-writing` and `architect`.

In my last post, I showed you why verification is the foundation of everything I do with agents, and how to get started creating your own verification skills. The key lesson was that if an agent can't verify its own work, nothing else matters. You remain the bottleneck, and your whole day will be spent babysitting your agents.

But once you have verification working, the next question is: how do you actually figure out what to build?

In this post, I am going to walk you through how I do research, planning, prototyping, and architecture with pstack. This is the exact workflow that allows me to ship thousands of PRs a month into production while keeping code quality extraordinarily high.

## The art of supervising someone smarter than you

Back in the old days of 2024, to make any change in a system, you first needed to read enough of it to build a mental model of what's going on. Depending on the size and complexity of the codebase, this may have taken you anywhere from hours, to even days and months. With a small change, you could get away with maybe only a local understanding of a small subsystem. If you were refactoring the core however, you'd probably need to have a mental model of how the whole thing works in order to do the refactor correctly and effectively.

Agents obviously remove this barrier. You can make changes to systems very easily by just prompting your agent and it will do it, regardless of how much or little you know about the code. But keeping the quality of the code and user experience high is still difficult, especially if you're not already a domain expert who knows what to look for and ask.

Even though frontier models have gotten very capable, there are still 2 failure modes that I constantly observe:

They are unable to fully understand your intent because they're under/poorly specified.

They don't have enough context on how to do the work correctly.

Both of these problems are related. Using agents well comes down to how well you're able to prime the agent's context window with high quality context. You can certainly write code that works without doing this, but I find that the outcomes and quality are much better when I've done the work to provide my agents with everything they need to do a high quality job.

## In your own words

Frontier models are very capable coders. While with older models I might have prompted very specifically what I wanted it to do, almost micromanaging them, the latest models are able to write code better than you or I can. So there's a fine balance I want to strike with telling the agent what I want it to achieve, while giving it the freedom to solve it in ways I might not have thought of.

This is the art of supervising someone smarter than you, on a codebase you haven't written yourself, and where humans can no longer fit the entire mental model of the codebase in their head.

One technique I like to use is the indirect prompt. Instead of telling the agent exactly what I want, I try to draw it out of the agent instead - in its own words.

For example, when someone reports an issue in Slack, I will often ask the agent to read the thread and restate the problem in its own words before doing anything else.

For example, I might say:

`/poteto-mode read this slack thread. restate in your own words and in plain english what you think the underlying issue is`

This accomplishes three things:

First, it forces the agent to compress a noisy conversation into a structured problem statement. Second, it lets me catch misunderstandings immediately. If the agent fixates on a red herring in the thread, I can correct it quickly before it starts writing any code.

And third, I haven't potentially led it down the wrong path by stating my own assumptions and hypotheses which could be incorrect or limit what the agent could otherwise achieve.

## Building up a mental model

Asking the agent to restate itself in a way that you can understand is an important part of working with someone that is smarter than you. That was the inspiration for `/teach`, a skill that helps your agent explain things to you in an intuitive way. I use it whenever I need to make sure my agent is doing something that makes sense to me.

Under the hood, `/teach` calls out to `/how` and `/why`.

`/how` traces runtime mechanics. When you ask `/how`, the agent assesses the complexity of the subsystem. If the subsystem spans multiple directories or services, it spawns parallel explorer agents on fast, efficient models like Grok.

`/how is virtualization implemented?`

`/why` investigates motivation and intent. Code tells you what happens. It rarely tells you why someone wrote it that way. When you run `/why`, pstack queries historical evidence across multiple sources in parallel: Git history and PR review comments, Linear tickets, Notion design docs, Slack conversations, Datadog monitors, Sentry errors, code lineage, and analytics warehouse events.

`/why are we still stuck an old version of node.js?`

I use `/teach` whenever I want the agent to restate something so I can better understand and trust its work.

`/teach me why you implemented it this way and not <other way>. what were the tradeoffs you made and why?`

In practice, I have also found that the research done by the `/teach` skill is not just useful to humans, but for agents as well. Even with the latest frontier models, (this also depends on the quality of the harness), in general I find that they still often state things confidently without backing it up with data or actually reading the code needed to build up a mental model of how it works. So this act of teaching you what it's going to do and why ends up helping the agent too.
