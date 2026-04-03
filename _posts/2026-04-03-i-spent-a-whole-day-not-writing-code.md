---
layout: post
title: "I Spent a Whole Day Not Writing Code. It Was the Best Decision I've Made."
date: 2026-04-03
permalink: /blog/i-spent-a-whole-day-not-writing-code/
excerpt: "I started a new vibe coding project and haven't opened a code editor once. Here's what a proper planning phase looks like when you approach it like a PM."
---

I have a confession. I started a new vibe coding project last week and I haven't opened a code editor once.

Not because I'm stuck. Because I made a deliberate choice: this time, I plan first.

If you've been following along, you know my first project taught me some expensive lessons about what happens when you skip the thinking and jump straight to the building. I'm not going to rehash all of that here — you've heard it. But those mistakes are exactly why I'm approaching this new project completely differently. This time I wanted a real plan before touching a single file.

What I didn't expect was how much there was to plan — and how useful Claude would be as a planning partner, not just a coding one.

---

## The Goal

I'm building a direct booking website for a couple of short-term rental properties I own in Stockholm and Mil Palmeras, Spain. Think Airbnb — but mine. No commissions, no algorithm dependency, full control over the guest experience. And eventually, fully agentic: AI agents handling everything from guest messaging to dynamic pricing to SEO blog posts.

It's a personal problem I actually have, which is the only kind of project I'll commit to now.

---

## Starting With the Pitch (Not the Code)

The first thing I did — before user stories, before thinking about tech stack, before anything — was write a product pitch.

Not because I'm pitching to investors. Because the exercise of writing a pitch forces you to answer the hardest question first: *what problem does this actually solve, and for whom?*

The prompt I used wasn't "write me a product pitch." It was a detailed brief — the problem, the solution, the agentic vision, the properties, the target audience — and I asked Claude to write clean prose I could share with a developer or collaborator. The output became a reference document I can paste into any new AI session to instantly re-establish context.

That's a habit I'm stealing from my own PM toolkit: write the brief before you build anything. It sounds obvious. It almost never happens.

---

## The Prompts Are the Product

Here's the thing I keep learning about vibe coding that I didn't fully appreciate in my first project: the quality of your prompts determines the quality of everything downstream. Bad prompt in, chaos out.

So for user stories — which any PM knows can spiral into an endless backlog — I didn't just ask Claude to "write user stories for a booking website." I designed a prompt.

The prompt tells Claude to act as a product manager running an *interactive session*. It doesn't dump all the stories at once. It goes area by area, asks scoping questions before writing anything, confirms after each batch, and builds a running list of what's been completed. It also has this instruction baked in:

> *"Before writing any story, ask whether it's truly needed for that version. The goal is the smallest possible increment that delivers real value."*

That one line changed the whole dynamic. Instead of Claude generating 40 stories and me having to cut them down, it challenged every story before writing it. That's how I want a PM on my team to behave. Turns out you can prompt for that.

---

## This Is Where the PM Instinct Actually Earns Its Keep

Claude put an AI chat assistant in the MVP. Seemed reasonable on the surface — guests ask questions, AI answers them instantly, saves the team time. The stories were well-written, the acceptance criteria were solid. It looked like a perfectly scoped feature.

I caught it.

Does a guest need this *before the first booking can happen*? No. Does it block going live? No. Does it add meaningful complexity to v1? Yes. Out it went — into v2, where it belongs alongside the other guest communication features.

Same thing happened with "Operations." Claude scoped it as a single version covering automated messaging, SEO content agents, dynamic pricing, and Facebook ad campaigns. All in one release. When I actually looked at that list, it was four different problems with four different dependency chains, each one capable of slipping a timeline on its own. I pushed back and split it into three separate versions — Communications, Marketing, Intelligence — each with a single clear goal and a clear definition of done.

This is the part where having a PM background actually helps. Not because I know more than Claude does about writing software, but because I know what "done" looks like and I know that scope creep doesn't announce itself. It shows up dressed as a reasonable feature.

The discipline isn't "don't build it." It's "don't build it *yet*." And the prompt itself is now designed to ask that question before writing a single story.

---

## The Living Roadmap

This is the thing I'm most pleased with from this planning phase.

I created a `ROADMAP.md` file that lives in the GitHub repo. It has the five versions, the scope for each, and — this is the important bit — a **scope decisions log** at the bottom. Every time I defer something or move a feature between versions, I write a one-line entry: what moved, and why.

```
| 2026-04-03 | AI chat assistant moved from v1 → v2 | Not needed to take first booking; adds complexity to MVP |
| 2026-04-03 | Operations split into v2/v3/v4 | Smaller increments, clearer value per release |
```

This solves a problem I had constantly in my first project: re-explaining context to a new AI session. Now I point any LLM at the repo and say "read the roadmap." The decisions are already there. I don't have to remember them. The AI doesn't have to guess.

---

## The GitHub Insight I Didn't See Coming

Speaking of LLMs and context: I was planning to use GitHub Issues to track user stories (standard practice for any team), and I asked myself whether that made sense for a one-person project.

The honest answer is: not for the usual reasons. I don't need to coordinate with a team. I don't need to assign stories or track who's working on what.

But here's why I'm going to do it anyway: I run out of credits. Not in some distant future — regularly, mid-session. When that happens I switch to a different model. And every time I do, I lose context. I have to re-explain the whole project from scratch.

GitHub Issues are persistent, model-agnostic context. I can be in Claude one day, GPT-4 the next, and just say "look at the open issues in milestone v1." The acceptance criteria are already there. The model has everything it needs without me re-typing a single thing.

I'm not setting up Issues yet — that's a right-before-I-start-building task. But knowing *why* I'm doing it makes it feel less like overhead and more like infrastructure.

---

## What I'd Tell You to Try

If you're starting a vibe coding project — especially if you have a PM background and think you can skip this stuff because you know what you're doing — here's what I'd actually recommend:

**Write the pitch first.** Even if it's just for yourself. It forces you to answer whether you're solving a real problem before you commit to building anything.

**Design your prompts, don't just ask questions.** The user stories prompt I built took maybe 30 minutes to write. It's saved me from generating a backlog I'd have spent hours trimming. That's a good trade.

**Build the scope decisions log from day one.** Not a fancy tool — a table in a markdown file. Date, decision, reason. You'll thank yourself in session 12 when you can't remember why something isn't in v1.

**Think about LLM context as infrastructure.** Whatever you put in your repo, an AI can read. Structure your planning documents with that in mind.

The building starts next. I'll let you know how it goes.

---

*Next post: setting up the repo, choosing the stack, and the first thing I'm going to ask Claude to build.*
