---
layout: post
title: "How to Run a Sprint Planning Session with Claude"
date: 2026-03-27
permalink: /blog/sprint-planning-with-claude/
excerpt: "I run sprint planning sessions for a living. Then I ran one with Claude to review 16 user stories. Here's what the session actually looked like."
---

I have sixteen user stories for my app. They were written after most of the app was already built — which is not how it's supposed to work, and I know that. I do this for a living.

Last week I decided to review them. Not rewrite from scratch, just go through them group by group and check if they still matched what the app actually does. I shared the document with Claude, pointed at the test results from a run I'd done the week before, and said: one group at a time.

It started with registration.

Claude summarised the first story, flagged that the test run had confirmed all three scenarios, then asked: *"In your use case, are you a different person from the patient? And do you want the app to eventually support both scenarios explicitly — or is the current model good enough for v0.1?"*

That's the question a good business analyst asks in a refinement session. Not a code question. A product question. It came from Claude reading the database schema and noticing that "patient" wasn't a user role — it was a profile record attached to a guardian account. A distinction I hadn't documented because I built it and had just absorbed it.

I answered: same model for v1, keep it simple. It noted the decision and moved on. No debate. No "but have you considered." Just: noted, next group.

The quality of the question came directly from the context I'd given it upfront — the spec, the test results, the code. It couldn't have asked that question from the document alone.

---

We got to authentication. Claude summarised the login story, then said there was no user story for password reset. Then — because it had read the code, not just the document — it described the full feature that was already built. A forgot-password link, a token-based reset email, a one-time-use token that expires in 24 hours. The whole thing implemented and undocumented.

That happens in real refinement sessions. Someone looks at the spec and says: this has been live for months, why isn't it here? In a team, that gap belongs to someone. In my case, that someone was me.

Working one group at a time meant that question had room to surface. If I'd shared all sixteen stories and asked for a general review, that gap would have been a bullet point at the bottom.

---

Two groups in and I'm describing this as if it was a normal meeting.

It was a normal meeting. I facilitate this kind of session at work. Someone presents the stories, the team asks questions, scope decisions get made, gaps get surfaced. It takes an hour if everyone's prepared, longer if they aren't.

The difference with Claude is that it comes prepared. It had read the document, the test results, and the codebase before I asked the first question. When I made a decision, it stopped asking about it. When the session ended, it didn't need five minutes of wrap-up.

I'm not saying it replaced a team. A team catches things Claude doesn't — the edge case someone ran into months ago, the thing a user said in feedback, the legal question nobody's looked into properly. None of that is in any document I gave it.

But for what I had — a document, a test result, a codebase — it ran the session.

---

We still haven't finished the review. The reports section has seven sub-stories just for PDF output.

Claude will get to them.
