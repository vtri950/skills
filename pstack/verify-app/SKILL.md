---
name: verify-app
description: |
  Build and maintain an agent-operable verification loop for your app — a small CLI that drives the app like a user plus a Feature Map of how to reach every feature. Use when agents keep saying "done" without proof, when verification costs more tokens than the fix, when fresh chats can't find features, or before setting up auto-repro / parallel agents. Pairs with ce-work (execution) and ce-debug (diagnosis) as the loop they verify against.
license: MIT
metadata:
  version: "1.0.0"
  source: "https://x.com/poteto/article/2094457600259842065"
  origin: "pstack Pt.1 — Verification is all you need (lauren / @poteto)"
---

# Verify-App: Give Agents a Loop They Can Close Themselves

If an agent can't verify its own work, you are the bottleneck. Tests alone are not verification — verified means the agent actually ran the app and proved the behavior with traces, screenshots, or video.

**Core loop:** drive app with CLI → observe machine-readable output → fix → re-drive → show proof.

## When to use

- Agent says done but you end up manually clicking through to check
- Every investigation starts with "how do I even run / log in / seed this app?"
- You want parallel agents, auto-repro of user reports, or perf confirmation with sample sizes
- `ce-work` / `ce-debug` runs need a concrete verification target

## Build 1: The control CLI ("Build the Lever")

Prefer a small tool over more markdown. A CLI call costs fewer tokens than a throwaway script and is reproducible and testable.

Pick the richest runtime you have:
- Web/Electron → Chrome DevTools Protocol (navigate, click, screenshot, trace, console)
- iOS → simulator control. No rich runtime? Build a dev sidecar or script `lldb` / test harness — or treat lack of debuggability as a stack smell.

Seed the dev experience first, or the CLI is useless:
- one-command consistent dev env bring-up
- seeded dev DB, test users, auth bypass, test/staging API endpoints

Agent-friendly CLI rules:
- Composable, deep modules (a few powerful commands, not 50 flags)
- Subcommands to disclose functionality gradually
- `--dry-run` on anything destructive
- Errors tell the agent what to do instead, not just what failed
- Rich `--help`, machine-readable output (JSON) by default

Example shape (adapt to your stack):

```bash
app goto <feature> --json        # navigate via Feature Map entry
app act click <selector> --dry-run
app shot --video 10s --json      # screenshot / short video as proof
app trace --perf 30s --json      # perf trace of status quo vs fix
app db seed --scenario checkout
```

Get the basics error-free before anything advanced. This CLI is critical infra, not a helper script.

## Build 2: The Feature Map (materialized memory)

A compact, searchable map of every feature, what it does, and how to reach it as a user. Saves context on every run; shared memory for all contributors (human + agent).

```
references/features/README.md        # index: feature → link
references/features/<feature>.md     # what it is, how to reach it, edge states
```

Each entry: purpose in one line, user path to reach it, auth/data preconditions, CLI commands that drive it. Have the agent catalog the app to bootstrap it — don't hand-write.

Your codebase is the ultimate memory; the Feature Map is its token-cheap projection.

## Maintain it like infra

- Re-run a maintain pass at least daily (and let agents patch the map as they work — catch the rest in the daily pass)
- Broken CLI / stale map = all downstream agents degrade. Consider oncall-level ownership on teams.
- Prompt pattern for features: `build <feature + context>. verify with the control CLI and show me video + screenshots as proof`
- Perf pattern: `trace status quo with control CLI, fix, then confirm the win with N repeated runs`
- Scale pattern: run verification in cloud/parallel agents (not local worktrees — 10-way local parallelism is the practical ceiling) for fuzzing and sample sizes; keep the coordinator's context clean by delegating runs
- Automation pattern: pipe user reports (e.g. Slack) into a routine that tries to reproduce via CLI + Feature Map first; only then decide to auto-fix

## Done checklist

```
[ ] Agent can bring up env, seed data, log in without asking
[ ] Every major feature reachable via CLI + documented in Feature Map
[ ] Errors guide next action; output is JSON; --dry-run exists
[ ] Fix prompts return video/screenshot/trace proof, not just "tests pass"
[ ] Map + CLI refreshed daily / auto-patched during work
```
