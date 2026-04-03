---
layout: post
title: "How to Run a Backlog Refinement With Claude"
date: 2026-04-03
permalink: /blog/backlog-refinement-with-claude/
excerpt: "I built the guardian side of this app without running a proper backlog refinement. The assistant side is where I tried to get it right."
---

(Most teams still call this a grooming session. Same thing.)

---

I built the guardian side of this app the wrong way.

I had a rough idea of what it should do, so I opened a code editor and started building. Features appeared because I was curious about them — a self-booking system, activity tags on entries, a whole Hours tab in the nav that I barely opened once it was done. The sprint planning session I wrote about [earlier](https://rosekaron.github.io/blog/sprint-planning-with-claude/) was where I started catching the damage. A feature cut, two stories rewritten against the wrong actor, a whole side of the product that existed only in my head.

There's a distinction in product development between discovery and delivery. Discovery is figuring out what to build — validating that it's the right thing before a line of code gets written. Delivery is building it. Most teams do both. I'd been skipping straight to delivery every time.

When it came time to build the assistant view — the app my assistants use to clock in and out — I wanted to do discovery first. No prototyping until I knew what I was building. No stories written from memory.

The problem with that approach is that discovery needs someone in the room.

---

At work, my job is to stay close to the people the product is for — users, stakeholders, anyone near the problem — and make sure that what gets built reflects what I find. The idea is continuous discovery: not a single research phase at the start, but an ongoing conversation with users that runs alongside the build. The goal is to stay close enough to the problem that you're never more than a conversation away from knowing whether you're building the right thing.

On this project, I was doing it alone. There was no one to talk to.

So I talked to Claude.

It isn't that Claude is experienced or has domain knowledge. It's that Claude is a good speaking partner. And when you're building alone, a good speaking partner is what you're missing most.

---

The first story that needed talking through was one I almost got wrong entirely.

There's a real issue in the Swedish personal assistance industry. Care companies have been caught falsifying hours — logging shifts that didn't happen, billing Försäkringskassan for work that was never done. It's documented. Prosecutions have happened. I'd been thinking about building a confirmation step: the assistant signs off on their own hours before the guardian submits the report. An audit trail. Proof the work was real.

This is a classic product trap: falling in love with the solution before you've validated the problem. I had a real problem, a clean solution, and hadn't stopped to ask whether the problem belonged to my user.

I explained it to Claude. We talked through who the fraud was actually happening with.

The app's MVP user is egna arbetsgivare — private individuals who employ their own assistants directly, without an agency. Usually a family. The assistant is someone they know. The fraud is institutional — companies manipulating reports at scale. That incentive structure doesn't exist in a family arrangement.

I knew this. But I hadn't said it out loud yet. Talking it through was how I confirmed what I already suspected: the problem was real, the solution was reasonable, and this was not the right user for it. Not MVP.

That's the pattern. I come in with the user knowledge. I describe the situation. The conversation is where I work out what it means for the build.

---

Not everything needed a decision. Two things just needed closing.

BUG-05 had been open for weeks. The kollektivavtal field — collective agreement — showed blank in assistant profiles. I knew egna arbetsgivare almost never have collective agreements. Blank is correct. The "bug" was the right behavior. I said this to Claude, we confirmed it, closed it: won't fix.

OQ-01 was an open question about whether waiting time and standby time should count toward weekly hour limits at full rate or reduced. The precise legal answer needed a specialist. I knew the risk of getting the default wrong: undercount and we've created a labor law exposure; overcount and we report conservatively, which Försäkringskassan prefers. I talked through the risk. We agreed on the safe default: count everything at 1:1.

Every open question that doesn't get resolved is time the team spends uncertain. Solo, the cost is different. Open questions don't block anyone. They just sit. Both of these had been sitting because there was no one to say them to.

---

The session ended with a spec: twenty-three user stories, eight design decisions, two closed items that didn't need building.

The way I work hasn't changed. I talk to people. I represent what I learn in what gets built. The difference here is that I was building alone — no team, no stakeholders in the room, no one to say things to.

Claude didn't bring expertise I didn't have. It brought the conversation. And it turns out that's most of what I needed.

The assistant view isn't built yet. That comes next.

---

*Rose Karon writes about parenting, care, and figuring things out one conversation at a time.*
