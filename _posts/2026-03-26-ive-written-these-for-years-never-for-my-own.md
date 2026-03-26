---
layout: post
title: "I've Written These for Years. Never for My Own."
date: 2026-03-26
categories: blog
permalink: /blog/acceptance-testing-vibe-coded-app/
excerpt: "I use AI to write user stories at work. I skipped them entirely when building my own app. Turns out the basics matter more when there's no team to catch what you miss."
---

*Part 9 of 9 — **Building an app with Claude: a non-developer's journey***

---

At work, I use AI to speed up discovery.

We invite an AI notetaker into grooming and planning sessions. It listens, extracts the requirements, and outputs structured user stories — hooked directly into JIRA and Confluence. By the time the meeting ends, the stories are already drafted. It's one of those workflows that once you have it, you can't imagine not having it.

I didn't apply any of that here.

Not because I forgot. Because I told myself it wasn't necessary. I was building alone. No team to align. No sprint to plan. No stakeholder asking for a ticket. So I skipped the basics and went straight to building.

Weeks in, someone asked me to list the user stories. I opened a blank file.

Sixteen features built. Zero stories written. I started from scratch.

---

They came quickly — I know the format. Thirty minutes and I had sixteen stories across login, dashboard, assistants, reports, settings, FK compliance.

Then: "What about registration?"

I'd missed it. Not in the code — registration existed. In my thinking. I'd documented what the app does once you're inside and skipped the part where you get in. Two stories added: guardian self-registration and assistant invite-based setup, with edge cases for duplicate emails, expired invite links, weak passwords.

That's the thing about user stories. They don't just document what you built. They expose what you assumed.

When you're on a team, someone in the room catches the gap. When you're building alone, there's no one to ask "what about registration?" until you're already deep in development.

---

Writing the PDF test cases made it worse.

The app generates a PDF at the end of the reporting flow — printed, signed, mailed to FK. I had been testing it by checking that it downloaded. That's not a test. That's checking that a file exists.

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

What came out of this session wasn't just a document. It was a quality baseline — something to run every time a new feature is added to make sure nothing in the existing app broke. That's regression testing. And once it exists, you want it to run automatically, not just sit in a file.

We built two skills for it: one that writes new test cases when a feature is done, one that runs the full list and produces a pass/fail report. The manual ones — like the signature field — get flagged explicitly. The runner pauses, gives you the steps, waits for your confirmation before moving on.

The test type tags are now on every story:

```
| 🔁 Automated | UI state, page content, redirects, API responses        |
| ✋ Manual     | Printed output, visual layout, mathematical spot-checks |
| 🔁/✋ Mixed   | Some criteria automated, some manual                   |
```

Twenty-two stories. Most automated. One that requires a printer.

---

If you're vibe coding something — anything — start with the user stories before you start building. Not because it's process for process's sake. Because the act of writing them forces you to define what done actually looks like. It keeps you honest about the MVP. It catches the registration screens you forgot to think about.

I've been doing this at work for years with proper tooling. I skipped it here because I thought building alone meant the basics didn't apply.

They apply more.

---

*[← Part 8: The Instruction Sheet I Wish I'd Written From Day One](/blog/the-instruction-sheet-i-wish-id-written-from-day-one/)*
