---
layout: post
title: "Why I Use a Framework for AI Coding Now"
date: 2026-04-24
permalink: /blog/ai-coding-framework-multi-agent-workflow/
excerpt: "A single AI agent can write malformed output and not notice. An AI coding framework puts a second agent in the loop to catch it before you act."
tags: [vibe-coding, claude-code, workflow, ai]
---

The verification agent was meant to check whether my plans covered the right requirements. Instead, it flagged something else. Three plan files ended in stray XML:

```
</output>
</content>
</invoke>
```

Tool-call syntax. The kind of tag a Claude agent uses internally to wrap its tool invocations. It had somehow leaked out of the wrapper and into the files the agent was writing. The planner agent — a separate Claude agent upstream — had bled its own scaffolding into its own output and never noticed.

I didn't notice either. I was about to run the execution step.

## What actually happened

I'm building a Swedish personal-assistance portal — the kind of app a family running a care arrangement uses to handle schedules, time reports, and the monthly forms for Försäkringskassan. For this phase, the work is data entry and PDF verification, not new code. The job: walk through the existing app, enter real information for the first time, and check that three generated PDFs (a Försäkringskassan form, a tax form, and a salary slip) contain zero placeholder strings.

To plan that, I used GSD — "Get Shit Done" — a workflow framework for Claude Code. The `/gsd-plan-phase` command runs a small pipeline. A planner agent drafts the work into three `PLAN.md` files. Then a second agent — the checker — reads what the planner wrote and looks for gaps.

The checker did its job. It ran through requirement coverage, decision coverage, threat-model checks, anti-shallow-execution rules. And it also flagged six advisory observations — non-blocking, but worth naming. The first one on its list:

> *"Stray tool-call artefact at file tails. All three plans end with leaked `</output></content></invoke>` lines after the final `<output>` section. Not structurally breaking, but noise."*

Thirty seconds of cleanup. Then the plans were safe to execute.

## An agent doesn't know where its own output ends

That moment is the reason I run a framework now.

A single agent writing to a file doesn't have a clear sense of where "the message I'm sending to the user" stops and "the artefact I'm writing to disk" begins. In isolation, it can produce output that looks perfectly confident and is silently malformed. The agent won't tell you. It can't — the only thing that can tell you is something *outside* the agent that re-reads what came out.

That's what the checker is. It's not smarter than the planner. It just has a different job and a different view of the same file. One agent writes; a second one verifies. The second one catches things the first can't see about itself, because it doesn't carry the same context fog.

This is not a Claude insight. It's a team insight.

## The PM version of the same idea

In product, we've known this forever. Nobody reviews their own pull request. Nobody writes acceptance criteria and also decides whether they passed. Sprint gates don't exist because engineers can't be trusted — they exist because fresh eyes catch different things. Same code, different reader, different failure modes surface.

I'm not inventing anything by putting a checker after a planner. I'm just applying sprint gates to AI agents. What was surprising was how *necessary* it turned out to be. The planner wasn't doing a bad job — the plans were genuinely good. It just couldn't see its own leaked tags. That's the whole story. The thing it couldn't see was completely invisible from the inside.

## What I used to do

I used to have Claude do everything in one long conversation. Discovery, planning, code, tests — all in one context window, one transcript, one voice. It felt efficient. It was the fastest way to "get something working."

It was also how I ended up with [an app that did too much](/blog/ai-mvp-scope-product-judgment/). Single-agent Claude doesn't self-limit. It doesn't stop to verify. It doesn't push back on itself. It's incredibly good at *keeping going*, which is exactly the wrong instinct for product work where stopping to check is half the job.

A framework forces stopping. Each stage writes an artefact to disk — a discussion log, a research doc, a plan file, a verification report. The next stage starts from that artefact, not from the previous conversation. The context resets. The agent reading the plan hasn't seen how the plan was written. It just sees the plan and has one job: judge it.

That reset is doing more work than it sounds like. It's the same reason [I now write reference docs aimed at a collaborator who forgets everything between sessions](/blog/writing-docs-for-ai-coding-assistants/) — context doesn't survive on its own. You have to externalise it.

## Why GSD specifically

GSD is the framework I've landed on because it's built for Claude Code, which is what I use. It names the stages I was already trying to impose on myself manually — discuss, research, plan, verify, execute, verify again — and gives each one a dedicated agent with a focused prompt. The handoffs are files I can read. If the checker flags something, I can see exactly what the planner wrote and decide whether to fix it, overrule it, or replan.

I didn't build it. I just stopped pretending I didn't need it.

## What I'd tell you to try

**Put one agent between you and action.** If you're using Claude Code and you trust its output straight into commits, don't. Add a second step — even manually — where a fresh Claude reads what the first one produced before you act on it. The specific framework doesn't matter as much as the habit.

**Read the checker's advisories, not just its blocks.** The blocking issues are the obvious win. The advisory ones are where the weird stuff hides — in my case, three files ending in HTML that shouldn't have been there, which nobody would have noticed until the executor silently ignored them.

**Stop being impressed that an agent finished.** Finishing is easy. Finishing cleanly is the thing you're paying for.
