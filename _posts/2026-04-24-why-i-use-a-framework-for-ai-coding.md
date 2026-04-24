---
layout: post
title: "Why I Use a Framework for AI Coding Now"
date: 2026-04-24
permalink: /blog/ai-coding-framework-multi-agent-workflow/
excerpt: "A single AI agent can't verify its own plans. A framework puts a second pair of eyes on the quiet drift you'd miss on a skim."
tags: [vibe-coding, claude-code, workflow, ai]
---

The verification agent flagged four categories of drift before I was allowed to execute anything.

1. **Requirement coverage.** All three requirement IDs — DATA-01, DATA-02, DATA-03 — had to be mapped to at least one plan. They were.
2. **Secret hygiene.** No real bank digits, no real Swedish personal numbers, no real addresses embedded in plan bodies. Real values only flow in at runtime; the plans themselves are committed to git, and I don't want those values in git. The checker verified nothing leaked.
3. **Decision fidelity.** Seven locked decisions from the context doc — D-01 through D-07 — had to appear somewhere in the task list. They did.
4. **Cosmetic noise.** Three plan files ended in stray tool-call XML the planner had leaked into its own output. Not breaking — the executor would have ignored it. Noisy.

The first three would have silently broken something if missed. The fourth was the only one I'd have caught on a skim — and it's the least important of the four.

## What the planner couldn't see about itself

I'm building a Swedish personal-assistance portal — the kind of app a family running a care arrangement uses to handle schedules, time reports, and the monthly forms for Försäkringskassan. For this phase, I was planning a data-entry step: walk through the app, enter real information for the first time, then generate three PDFs and check each one for placeholder strings.

The planner agent drafted the phase into three `PLAN.md` files. The checker agent — a separate Claude instance with a different prompt and a fresh context — read those files and ran through its verification checklist.

Three of the four things the checker flagged were invisible from inside the planner's view. The planner knew what it had written. It couldn't tell whether what it had written still matched the requirement document it had never re-read in full, or whether seven specific decisions from the context doc had survived translation into tasks. It couldn't spot its own stray closing tags — those tags are scaffolding it uses every second.

That's not a Claude failure. It's a context-window failure that any single-agent setup will have. The planner's job was to plan. Checking that the plan matches the inputs is a different job, and it needs different eyes.

## The PM version of the same idea

In product, we've known this forever. Nobody reviews their own pull request. Nobody writes acceptance criteria and also decides whether they passed. Sprint gates don't exist because engineers can't be trusted — they exist because fresh eyes catch different things. Same artefact, different reader, different failure modes surface.

I'm not inventing anything by putting a checker after a planner. I'm just applying sprint gates to AI agents. What was surprising was how much it turned out to catch — and how boring most of the catches were. Not dramatic tag leaks. Coverage gaps, drifted decisions, quiet mismatches between the plan and the document it was supposed to be implementing.

Nobody gets paid to catch boring things. That's exactly why agents don't catch them without being told to.

## What I used to do

I used to have Claude do everything in one long conversation. Discovery, planning, code, tests — all in one context window, one transcript, one voice. It felt efficient. It was the fastest way to "get something working."

It was also how I ended up with [an app that did too much](/blog/ai-mvp-scope-product-judgment/). Single-agent Claude doesn't self-limit. It doesn't stop to verify. It doesn't push back on itself. It's incredibly good at *keeping going*, which is exactly the wrong instinct for product work where stopping to check is half the job.

A framework forces stopping. Each stage writes an artefact to disk — a discussion log, a research doc, a plan file, a verification report. The next stage starts from that artefact, not from the previous conversation. The context resets. The agent reading the plan hasn't seen how the plan was written. It just sees the plan and has one job: judge it.

That reset is doing more work than it sounds like. It's the same reason [I now write reference docs aimed at a collaborator who forgets everything between sessions](/blog/writing-docs-for-ai-coding-assistants/) — context doesn't survive on its own. You have to externalise it.

## Why GSD specifically

GSD — Get Shit Done — is the framework I've landed on because it's built for Claude Code, which is what I use. It names the stages I was already trying to impose on myself manually — discuss, research, plan, verify, execute, verify again — and gives each one a dedicated agent with a focused prompt. The handoffs are files I can read. If the checker flags a coverage gap, I can see exactly which requirement is missing and which plan should have covered it.

I didn't build it. I just stopped pretending I didn't need it.

## What I'd tell you to try

**Put one agent between you and action.** If you're using Claude Code and you trust its output straight into commits, don't. Add a second step — even manually — where a fresh Claude reads what the first one produced before you act on it. The specific framework doesn't matter as much as the habit.

**Make the checker look at the boring things.** Requirement coverage. Secret hygiene. Whether the plan actually addresses the decisions in the context doc. Not whether the plan "looks right" — that's the thing the planner already thought it did.

**Don't be impressed by the dramatic catches.** The tag leaks are fun to spot. The coverage gap is the one that would have hurt.
