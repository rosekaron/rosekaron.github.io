---
layout: post
title: "How to Run a Sprint Planning Session with Claude"
date: 2026-03-27
permalink: /blog/sprint-planning-with-claude/
excerpt: "I run sprint planning sessions for a living. Then I ran one with Claude to review 16 user stories. The dashboard conversation alone changed what I'm building."
---

We were mid-session. We'd just finished running acceptance tests on the app and I said, without much ceremony: let's go through the user stories too. One group at a time.

I do this at work. Someone owns the backlog, the team reads through it, questions come up, decisions get made, gaps get added to the list. It usually takes an hour. It usually requires a room.

This took two minutes to start.

---

Registration was first. Claude read through the story, flagged which scenarios had passed in the test run, then asked whether I was a different person from the patient I'm managing care for — the app had fields for both but the user stories said nothing about which scenario applied.

I typed back:

> *"let's keep the same for v1"*

Claude noted it and moved on. No discussion. One line in, one line out.

---

We got to the dashboard. The story said: show a 7-day grid of the current week with coverage status. Claude flagged it, then asked:

> *"Current week only — or should the guardian be able to navigate to next week too?"*

I stopped and typed:

> *"let's review the objectives of the dashboard"*

So Claude asked:

> *"What do you want a guardian to walk away knowing after 10 seconds on the dashboard?"*

Not a code question. A product question. And I didn't have a clean answer.

I asked what the dashboard currently showed. Claude went to the code and came back: a 7-day grid hardcoded to the current week, a FK deadline countdown, and a pending approvals count. No week navigation — if you want another week, you click through to the schedule.

That was useful to know. The grid existed because it seemed like a good thing to have. Not because someone had asked what the dashboard was actually for.

I typed:

> *"I think the dashboard should show who is working today, what do I need to do right now and the upcoming reports that I need to fill up. Showing what's coming for the week doesn't seem to add so much value since that's already covered in the schedule."*

Claude's response:

> *"The weekly grid is a calendar feature wearing a dashboard hat."*

That was the moment. One sentence and the story was wrong — not broken, just aimed at the wrong thing. The dashboard needed to be an action centre, not a preview. Who's here today. What needs approving. What reports are overdue.

---

Then I described how the reports actually work.

Assistants log their shifts as they happen. The system aggregates those into a monthly report. The guardian reviews and approves it, generates a PDF, the assistant signs the printed copy, and it gets mailed to FK by post.

Claude came back:

> *"This changes US-09 and US-16. Both stories describe a guardian filling in daily hours — but that's not what happens. The guardian's job is to review what the assistant already logged. The stories were written for the wrong actor."*

They were. I'd written them early in the project when I was still figuring out how the workflow would go. The app had moved on. The stories hadn't.

Then Claude asked who does the shift logging — does the assistant have their own login?

> *"The assistants should have their own app where they can view their schedule, clock in and clock out."*

Claude flagged it immediately:

> *"That's a whole missing side of the product. The guardian can't approve what was never logged. Is that v0.1 or v0.2?"*

We haven't answered that yet.

---

What started as reviewing a single user story — a 7-day grid on a dashboard — ended with two stories rewritten, a product decision pending, and a clearer picture of what the app actually needs to do.

That's what a grooming session is supposed to do.

A grooming session works when people come prepared and decisions get made quickly. That's the whole thing. Most of the overhead — the scheduling, the catching up, the circling back — is just the cost of getting people to that state. If the context is already there, you can skip straight to the questions.

What it won't do is replace the person who says *I remember a user asking about this six months ago* or *legal flagged something similar last quarter.* That knowledge doesn't live in any document. But for what does — specs, test results, code — it runs the session.
