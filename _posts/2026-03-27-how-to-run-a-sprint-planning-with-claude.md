---
layout: post
title: "How to Run a Sprint Planning Session with Claude"
date: 2026-03-27
permalink: /blog/sprint-planning-with-claude/
excerpt: "I run sprint planning sessions for a living. One session with Claude cut a feature, rewrote two user stories, and uncovered a gap I never formally declared out of scope."
---

I run sprint planning sessions for a living. Backlog refinement, story review, gap analysis — I do this at work without thinking about it. On my own project, I'd been skipping it. I was building alone. Who was I going to run a session with?

The answer, it turns out, was Claude.

---

We were reviewing user stories for the dashboard. The story said: show a 7-day grid of the current week with coverage status. A reasonable thing to have. I'd built it without questioning it.

Claude flagged it and asked whether the guardian should be able to navigate to other weeks too.

I stopped and typed:

> *"let's review the objectives of the dashboard"*

Claude asked:

> *"What do you want a guardian to walk away knowing after 10 seconds on the dashboard?"*

I had an answer — I just hadn't said it out loud yet. The dashboard was always meant to be more action-oriented. I'd just never thought to tell Claude that, because when you're building alone, the things you know live in your head and stay there.

I asked what the dashboard currently showed. Claude went to the code: a 7-day grid hardcoded to the current week, a FK deadline countdown, a pending approvals count. No week navigation — for that you click through to the schedule.

Then I said:

> *"I think the dashboard should show who is working today, what do I need to do right now, and the upcoming reports that I need to fill up. Showing what's coming for the week doesn't seem to add so much value since that's already covered in the schedule."*

Claude:

> *"The weekly grid is a calendar feature wearing a dashboard hat."*

Feature cut. One sentence. In a conventional sprint planning session, cutting a feature involves a discussion, a rationale, someone updating the backlog, a decision log entry. Here it was two lines of conversation and the story was rewritten.

This is what the dashboard looked like before:

![Current dashboard — 7-day coverage grid]({{ site.baseurl }}/assets/images/dashboard-before.png)
*The current dashboard: a 7-day coverage grid, FK deadline countdown, and pending approvals.*

And this is what it will look like after we make the changes:

![Proposed dashboard — action centre]({{ site.baseurl }}/assets/images/dashboard-after.png)
*The proposed dashboard: who's working today, what needs action, upcoming reports. [Coming soon]*

---

Then I described how the monthly reports actually work.

Assistants log their shifts as they happen. The system aggregates those into a monthly report. The guardian reviews and approves it, generates a PDF, the assistant signs the printed copy, and it gets mailed by post to FK.

Claude:

> *"This changes US-09 and US-16. Both stories describe a guardian filling in daily hours — but that's not what happens. The guardian's job is to review what the assistant already logged. The stories were written for the wrong actor."*

They were. I'd written them early in the project when I was still working out how the workflow would go. The app had evolved. The stories hadn't followed.

In a conventional team, someone would have caught this in refinement — a BA reading the story against the code, or a developer asking a clarifying question before picking it up. When you're building alone, there's no one to ask. The gap just sits there until it causes a problem.

---

Then Claude asked who does the actual shift logging — does the assistant have their own login?

> *"The assistants should have their own app where they can view their schedule, clock in and clock out."*

Claude:

> *"That's a whole missing side of the product. The guardian can't approve what was never logged. Is that v0.1 or v0.2?"*

This was the biggest aha moment of the session.

The assistant app was always part of the plan. I just never wrote it down. When you're building alone, unwritten scope doesn't slow you down — you just don't build it yet and move on. In a conventional project, this would have been a formal descoping decision. A story written, reviewed, and explicitly moved to the next release with a rationale. Here it existed only in my head, which meant it didn't exist anywhere that mattered.

The session put it on paper for the first time.

---

Then came a question that sounds like something a PM asks a senior engineer on the first day of a sprint.

> *"could both these checks be applied as we log the schedule so it's more proactive or would that be overkill for MVP"*

This kind of question usually gets a long answer, a whiteboard, and a follow-up meeting. Claude:

> *"Not overkill — but the two layers need to do different jobs. A block on the schedule as you're mid-entry is disruptive. A warning on the schedule tells you early, while you can still do something. The PDF block stays as a hard stop. Warn when there's time to fix it. Block when it's too late to ignore."*

Decision made. Two layers: amber warning on the schedule when an assistant is approaching their limit, hard block at PDF generation if the limit is actually breached.

What happened there isn't just Claude answering a question. It's Claude holding three things at once — the product goal, the implementation tradeoff, and the user experience — and coming back with a recommendation and a reason. That's not a search or a task execution. That's what a senior engineer does when they've thought about a problem for five minutes.

The difference is that you have to know to ask. "Would this be overkill?" is a question most solo builders never ask, because there's no one to ask it to. Once you have someone to ask, the question becomes obvious.

---

One session. A feature cut, two stories rewritten, a gap surfaced, and an architecture decision made in two exchanges. That's a decent hour's work for a team of one.
