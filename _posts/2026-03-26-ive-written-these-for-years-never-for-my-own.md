---
layout: post
title: "I've Written These for Years. Never for My Own."
date: 2026-03-26
categories: blog
permalink: /blog/acceptance-testing-vibe-coded-app/
excerpt: "I use AI to write user stories at work. I skipped them for my own app. The basics matter more when there's no team to catch what you miss."
---

*Part 9 of 11 — **Vibe Coding for Non-Developers***

---

At work, I use AI to speed up discovery.

We invite an AI notetaker into grooming and planning sessions. It listens, extracts the requirements, and outputs structured user stories — hooked directly into JIRA and Confluence. By the time the meeting ends, the stories are already drafted. It's one of those workflows that once you have it, you can't imagine not having it.

I didn't apply any of that here.

Not because I forgot. Because I told myself it wasn't necessary. I was building alone. No team to align. No sprint to plan. No stakeholder asking for a ticket. So I skipped the basics and went straight to building.

At some point, my software development instincts kicked in. I wanted test cases — proper acceptance criteria I could run against the app before calling it done, and reuse later for integration testing as the app grows. So I sat down with Claude and asked it to help me build them.

I opened a blank file. Sixteen features built. Zero stories written.

---

## What Writing the User Stories Exposed

They came quickly — I know the format. Thirty minutes and I had sixteen stories across login, dashboard, assistants, reports, settings, FK compliance.

Then I read through them and noticed: registration was missing. Not in the code — registration existed. In my thinking. I'd documented what the app does once you're inside and skipped the part where you get in. Two stories added: guardian self-registration and assistant invite-based setup, with edge cases for duplicate emails, expired invite links, weak passwords.

That's what the exercise exposed — not a bug, but a gap in how I'd been thinking about my own app. When you're on a team, someone in the room catches that. When you're building alone, there's no one to ask "what about registration?" until you're already deep in development.

Writing the user stories mid-build told me something I should have known at the start: I skipped ideation. I should have done this before I wrote a single line of code.

---

## Acceptance Testing a PDF: Seven Test Cases for One Button

The PDF test cases made the gap even more visible.

The app generates a PDF at the end of the reporting flow — printed, signed, mailed to FK. I'd been testing it by checking that it downloaded. That's not a test. That's checking that a file exists.

The actual questions: does the patient name in the PDF match what's in Settings? Does the month on the report match the work month — not today's date, not the approval date? Does the total equal the sum of the daily entries? Does the right assistant's name appear on the right report and not bleed into another?

Seven test cases for one button. And this one — the last one — can't be automated at all:

```
### US-12g — PDF signature field is present and printable

> ✋ Manual — must not be skipped
> 1. Download the PDF for an approved report
> 2. Open it in Preview or a PDF viewer
> 3. Confirm there is a labelled signature field for the assistant
> 4. Confirm there is space for a date next to the signature
> 5. Print one page and confirm the field is legible and has enough
>    physical space to sign by hand
```

You have to print it. Hold it. Check that there's space to write. No tool can do that for you.

---

## Using AI to Turn Acceptance Tests into a Regression Testing System

Once the stories existed, I asked Claude whether we should build skills out of them.

The answer shaped something I hadn't thought through yet. User stories written for acceptance testing are the same user stories you run for regression testing every time a new feature is added. They're not a one-time checklist — they're a quality baseline. And a quality baseline sitting in a document is only as good as the last time someone remembered to open it.

We built two skills: one that writes new test cases whenever a feature is done, one that runs the full list and produces a pass/fail report. The manual ones get flagged explicitly — the runner pauses, gives you the steps, waits for your confirmation before moving on.

```
| 🔁 Automated | UI state, page content, redirects, API responses        |
| ✋ Manual     | Printed output, visual layout, mathematical spot-checks |
| 🔁/✋ Mixed   | Some criteria automated, some manual                   |
```

That conversation future-proofed the app in a way I hadn't planned for. Every new feature I add from here gets tested not just on its own — but against everything that already exists.

---

If you're vibe coding something, start with the user stories before you start building. Not because it's process for process's sake. Because the act of writing them forces you to define what done actually looks like. It keeps you honest about the MVP. It catches the registration screens you forgot to think about before you've built six features around the gap.

I've been doing this at work for years with proper tooling. I skipped it here because I thought building alone meant the basics didn't apply.

They apply more.

---

*[← Part 8: The Instruction Sheet I Wish I'd Written From Day One](/blog/ai-custom-instructions-skills/) · [Part 10: I Told It to Tell Me What It Learned →](/blog/how-to-improve-ai-custom-instructions/)*
