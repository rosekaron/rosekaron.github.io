---
layout: post
title: "How to Run a Sprint Planning Session with Claude"
date: 2026-03-27
permalink: /blog/sprint-planning-with-claude/
excerpt: "I run sprint planning sessions for a living. Then I ran one with Claude to review 16 user stories. Here's what the session actually looked like."
---

We were mid-session. We'd just finished running acceptance tests on the app and I said, without much ceremony: let's go through the user stories too. One group at a time.

I do this at work. Someone owns the backlog, the team reads through it, questions come up, decisions get made, gaps get added to the list. It usually takes an hour. It usually requires a room.

This took two minutes to start.

---

Registration was first. Claude read through the story, flagged which scenarios had passed in the test run, then stopped and asked:

> *"In your use case, are you a different person from the patient? And do you want the app to eventually support both scenarios explicitly — or is the current model good enough for v0.1?"*

In Sweden, sometimes the patient employs their own assistants directly. Sometimes a family member does it for them. The app had fields for both — a guardian profile, a patient profile — but the user stories didn't say anything about which situation applied.

I hadn't documented it because I built it and just knew.

I typed back:

> *"let's keep the same for v1"*

Claude noted it and moved to the next group. No discussion. No "should we park this for a follow-up." In a real meeting that question sometimes opens fifteen minutes of conversation about scope. Here it was one line in, one line out.

---

Authentication was next. We were a few lines into it when I typed, not really as part of the review — just because I realised I didn't know:

> *"how does the user reset his password?"*

Claude went to the code. Not the user stories document. The code. And came back with a full description of a feature that was already built: a forgot-password link on the login page, a token-based reset email, a one-time-use link that expires in 24 hours. Implemented. Tested. Not a single user story written for it.

In a sprint grooming session, that moment usually comes from someone on the team saying *wait, didn't we build this already?* It surfaces because someone remembers. Here it surfaced because I asked a random question and Claude looked in the right place.

The gap wasn't that the feature was missing. The gap was that no one had written it down — and I was the only person on this team.

---

We stopped two groups in. The reports section has seven sub-stories just for the PDF output, and we haven't touched it. I interrupted the review to write about the review because I think it's important to share what I learned here.

A grooming session works when people come prepared and decisions get made quickly. That's the whole thing. Most of the overhead — the scheduling, the catching up, the circling back — is just the cost of getting people to that state. If the context is already there, you can skip straight to the questions.

What it won't do is replace the person who says I remember a user asking about this six months ago or legal flagged something similar last quarter. That knowledge doesn't live in any document. But maybe, that's a topic that I can write about in the future.
