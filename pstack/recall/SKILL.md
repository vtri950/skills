---
name: recall
description: |
  The author's /recall, verbatim: pull recent context from chat history so fresh agents don't start from zero.
license: MIT
metadata:
  version: "2.0.0"
  source: "https://x.com/poteto/status/2097732320606507506"
  origin: "pstack Pt.2 — lifted verbatim from lauren / @poteto (light markdown formatting only)"
---

# Learning from history

> Lifted verbatim from [The Complete Guide to pstack Pt. 2](https://x.com/poteto/status/2097732320606507506).

Many of my projects span multiple conversations. For example, a few months ago I was working on fixing virtualization bugs and perf issues that people were reporting in Cursor. I realized that every time I started a new chat I had to basically start over with building up the rich context my agent had before when it was solving a similar problem.

What I realized is that your past transcripts are often a gold mine for rich context. pstack ships with the `/recall` skill to pull your recent context from chat history, so even fresh agents have the right context they need to get back to a good state.

`/recall the work i did yesterday on virtualization and then read this bug report on slack`

Using `/teach`, `/recall`, `/how`, and `/why` are how I keep my own mental models of the codebase up to date, compressed into a form I can easily understand and remember. And, it helps agents too!
