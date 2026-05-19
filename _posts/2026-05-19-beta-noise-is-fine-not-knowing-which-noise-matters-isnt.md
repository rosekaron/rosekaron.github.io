---
layout: post
title: "Beta Noise Is Fine. Not Knowing Which Noise Matters Isn't."
date: 2026-05-19
permalink: /blog/pm-builds-n8n-error-monitoring-tool/
excerpt: "Running agentic features in beta means expecting errors. The problem we had wasn't the errors — it was having no way to tell which ones needed urgent action and which ones could wait for the next sprint."
tags: [vibe-coding, n8n, product-management, ai]
---

When you're running agentic features in beta, errors are expected. Workflows fail. Credentials need refreshing. Edge cases surface that nobody anticipated. This is normal, and treating every error as a crisis would create more noise than the errors themselves.

The problem we had wasn't the errors. It was that all errors looked identical.

Our n8n workflows post to a shared Teams channel — `[N8N] Error Reporting` — when something breaks. The channel was designed to create visibility. What it created instead was a wall where a transient one-time blip and a workflow silently failing to update customer data looked exactly the same. No SLAs in a beta product means no alerting thresholds. Nothing to tell you which message in that wall actually required someone's attention today versus which one could wait until next sprint.

---

## The Signal Design Problem

The Lead Scoring workflow had an OAuth2 credential failure. A developer spotted it, raised a fix PR — good, that's the system working. But the same errors kept appearing for days while the PR waited. Nobody flagged it because by then the channel already had noise, and one more error message in a busy channel reads as background, not urgency.

That's not negligence. When you're actively shipping a beta product with many moving parts, you need the tooling to tell you what to look at. "Everything in the channel" is not a useful instruction when the channel has dozens of entries a day.

---

## The Two-Track Problem

The plan before building the monitoring agent was reasonable: review the error channel periodically, triage the non-critical issues, add them to Co-Work, and handle them in the regular development cycle. Non-critical failures in a beta product don't need hotfixes — they need to be tracked and scheduled like any other work.

The gap is the other track. Critical issues — a credential expiring that silently blocks data enrichment, a core workflow failing repeatedly across multiple days — can't wait for the next periodic review. They need to be surfaced and treated as urgent, separately from the non-critical queue.

The problem is that without tooling, both tracks look the same. Everything arrives in the same channel with the same formatting. Deciding which track an error belongs on requires reading it, recognising the workflow, knowing its criticality, and checking whether it's appeared before. That's a non-trivial amount of context to carry into every channel check — and it's exactly the kind of judgment that becomes unreliable under normal sprint load.

The pitch was about building the triage layer that does this automatically: surface what's critical immediately, consolidate the rest into a morning digest for the regular development cycle.

---

## Writing the Pitch Before Touching Any Tool

I use a Shape Up-adjacent approach for technical projects I initiate: pitch first, tools later. Appetite — how much time is this worth? — then solution, then risks and explicit no-gos.

For this one: one week. Two n8n workflows. One reads the error channel every hour and fires an alert when patterns cross a threshold — same error appearing five or more times in a window, repeating across three consecutive hourly windows, or any auth/credential keyword. One sends a morning digest email consolidating overnight errors into a prioritised list for the regular review cycle.

The no-gos were as explicit as the requirements: no auto-remediation, no changes to existing workflows, no snooze mechanism. No-gos are the most useful part of any pitch — they make scope defensible when someone asks "why didn't you also build X?" and they're the part most often skipped.

Writing the brief before opening any tool is a habit I've carried from years of PM work into how I now initiate my own builds. [It forces clarity about what problem you're actually solving](/blog/verify-claude-account-working-directory/) before you're already three nodes deep in a workflow and committed to a design.

---

## What Building It Yourself Actually Looks Like

I'm building this with Claude Code and a planning workflow that runs parallel AI research agents before committing to a design. The research step is the part I would have skipped a few months ago. Now it's the first thing I do.

Here's what it caught: the existing error handler — the workflow that posts to Teams every time a workflow breaks — only fires on production. On staging, it hits a conditional branch and does nothing. Dead end, by design.

That's not a footnote. It means the monitoring workflow I'm building would be tested against a Teams channel with zero messages in it during development. Every hourly run returns empty. The rolling log stays blank. I'd spend the build week convinced the workflow was functioning when it was reading silence.

The research step surfaced this before I wrote the first node. I wouldn't have caught it in a kickoff conversation. I might have caught it on day three, staring at an empty data table. The difference is thirty minutes of structured research before planning versus potentially half the build week debugging the wrong assumption.

---

## The Instinct That's Transferable

There's a PM habit that fires when I'm scoping something and realise I don't know how the existing system behaves. The instinct is to ask someone who does. What's changed is that I can run that question through AI research first — quickly, before committing to a design — and get enough signal to know what questions are worth asking humans.

This isn't AI replacing judgment. It's AI making judgment cheaper to apply earlier. The same instinct, faster.

The monitoring tool is useful. But the more transferable thing is the shape: notice a gap, name the two tracks that are currently conflated, pitch a solution with explicit no-gos, research before planning, build with AI. Each step is learnable individually. Combined, they're what "PM who builds" looks like when the gap is in your own team's operations and you're the one who spotted it.

---

## What I'd Tell You to Try

**Name the tracks before scoping the solution.** In this case: urgent issues that need immediate action, and non-critical issues that belong in the development cycle. If you can't name the tracks, you can't build the triage layer.

**Write the pitch before you open any tool.** Appetite, solution, risks, no-gos. The no-gos do the most work: they make tradeoffs visible before you're in the middle of a build and it's expensive to change course.

**Run research before planning.** If you're building something that touches existing systems, have AI research the system behavior before you commit to a design. It's the step most likely to surface the assumption you didn't know you were making — the one that would have cost days instead of minutes.
