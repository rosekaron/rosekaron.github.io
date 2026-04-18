---
layout: post
title: "What a PM Does When the AI Can Code"
date: 2026-04-18
permalink: /blog/pm-skills-ai-coding-assistants/
excerpt: "When you work with AI coding assistants, the skill that doesn't automate is checking the frame — two catches from one session that would have shipped broken."
tags: [vibe-coding, product-management, claude-code, ai]
---

A developer checks the code. A PM checks the frame.

That's the line that keeps rewriting itself in my head lately. A developer asks: does this function do what it says? A PM asks: are we even working in the right branch? Is this the field I asked for, or a plausible-sounding field Claude made up? Are the numbers in the commit message the numbers I actually approved?

These are different questions, and they stay different even when the AI is good at writing code. Especially then.

---

Over a single session this week I caught Claude twice on things that wouldn't have flagged as bugs. The code compiled. Tests would have passed. Each individual step was internally consistent. Both would have shipped broken. Neither was a hallucination in the classic sense — they were both frame drifts, where the AI was coherent about the content but operating on a frame I had to reality-check.

## Catch 1: The database column that doesn't exist

We were planning a schema change for the Swedish assistance-portal app I'm building. Claude proposed adding a column called `fk_decision_hours_per_day` — the daily hour allowance from a Försäkringskassan beslut — to sit next to the existing `weeklyHours` field. Made sense on the surface. A daily cap would enable downstream compliance checks.

I stopped at the column name.

> *"what is that? is that new?"*

That was my whole message. No framework, no context, just a question about a phrase that hadn't appeared in any earlier session.

Claude's reply was honest:

> *"It's a new column I'm proposing. I flagged this because `weeklyHours` and the proposed field look like they might be measuring the same thing."*

Good. Except the reply was about whether the two fields were redundant. The actual question was upstream: **does `fk_decision_hours_per_day` correspond to a real field on a real FK beslut?**

So I asked Claude to go and research. Forty minutes and four parallel research agents later, the answer was clean: Swedish FK decisions state hours *per week*, per 51 kap 9§ socialförsäkringsbalken. There is no daily-cap field. The column Claude had been about to propose didn't correspond to anything in reality. Claude then retracted:

> *"Drop `fk_decision_hours_per_day` from the roadmap. No such field is stated on FK beslut."*

If I hadn't asked "what is that?" — if I'd accepted the proposal on its surface — the column would have been added to the schema, the compliance rule engine would have read from it, and the guardian filing out FK forms next month would have been lining up against a constraint that doesn't exist in Swedish law.

The mistake wasn't a hallucination. It was a plausible extrapolation that hadn't been grounded. Claude couldn't tell the difference. I could — but only because I asked, not because the code looked wrong.

## Catch 2: The branch that stopped meaning what it used to mean

The second catch was less technical and felt weirder.

We'd just finished v1.0 of the app — shipped behind a git tag and an archive-branch named `milestone/v1.0-mvp`. All good. Next milestone, v1.0.1, was spinning up. I watched Claude make five planning commits: requirements, roadmap, research, schema comments. Each commit message cheerfully said "for milestone v1.0.1." Each one landed on the branch `milestone/v1.0-mvp`.

I looked at the screen:

> *"why am i seeing milestone/v1.0-mvp here? arent we working on version 1.0.1?"*

Claude was briefly silent — I can tell when it's re-reading its own context — and then came back with *"You caught something real."* We cut a new `milestone/v1.0.1` branch from the current commits. Nothing was lost. The commits were still fine content. But the container they were in had quietly stopped matching what the content said.

This one bothered me more than the first. Not because the fix was hard — the fix was two git commands — but because there was nothing wrong with any individual commit. Each one passed any check you could run against it. The drift lived in the relationship between the commits and their branch, and that relationship isn't a thing code-level checks see.

## The PM pattern: frame checks vs content checks

Both catches are the same shape. The AI was internally consistent in each case. It wasn't confused. It wasn't contradicting itself. It was operating on a frame — a set of assumptions about *what we're doing* — that had drifted a step away from reality, and then proceeding confidently from inside that drifted frame.

A content check asks: *is what you wrote correct?*
A frame check asks: *is what you wrote landing where we agreed it would land, on the thing we agreed it was about?*

AI coding assistants are getting very good at content checks. Most of the time, Claude writes code that does what it says it does. Tests will reveal when it doesn't. Linters catch style drift. Type systems catch contract mismatch. These are all content checks, increasingly automatable.

Frame checks don't automate. They require someone who remembers: we agreed this is v1.0.1, not v1.0. We agreed hours are weekly, not daily. We agreed this is the assistant's concern, not the employer's. They require someone holding the state of the world outside the code in their head and comparing it against what's appearing on the screen.

That's the PM job now. [Not just catching Claude over-engineering](/blog/ai-mvp-scope-product-judgment/) — that's part of it, but it's one kind of frame check. Not writing better prompts — that's table stakes. The job is the running comparison between *what Claude thinks we're doing* and *what we actually agreed we're doing*, and catching the drift early enough that the fix is two git commands instead of a rollback.

## Why this isn't paranoia about AI

Nothing in the session went wrong because Claude is bad. Claude is actually very good. The schema proposal was a reasonable extrapolation from context. The branch choice was a reasonable default from where the session started. In both cases, if I'd been pairing with a human colleague operating under the same assumptions, they would have done the same thing — and I would have had the same job of calling the frame check.

What's different is that Claude doesn't *feel* the frame drift. A human pair would notice "wait, are we still on the archive branch?" mid-commit. Claude doesn't have that kind of running awareness — not yet, and probably not in the way a person does. It has content consistency and frame indifference. Those are a very specific pair of traits, and knowing how they combine is what I mean by the PM job when the AI can code.

## What I'd Tell You to Try

**When something is proposed with a name, ask what it is.** Plausible naming is cheap. If Claude proposes a column, a branch, a file, an enum value — and you don't remember agreeing to its existence — ask "what is that? is that new?" That one question has a wildly disproportionate hit rate.

**Pay attention to the container, not just the content.** Is the commit on the right branch? In the right file? Under the right milestone? If the container stops matching what you're doing, the content underneath will slowly migrate to match the container, not the other way around.

**Catch frame drift early; the fix is almost always cheap.** Two git commands. Drop one column. Revert one line. Late frame drift is a rollback. Early frame drift is a correction. The return on a ten-second "wait, what?" is enormous.

---

The [post I published earlier today](/blog/writing-docs-for-ai-coding-assistants/) was about the response to one of these catches — how to document things so the next session doesn't re-derive them from scratch. This one is about the upstream skill: the noticing itself. Both are getting more central to how I work.
