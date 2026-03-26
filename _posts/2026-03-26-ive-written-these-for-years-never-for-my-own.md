---
layout: post
title: "I've Written These for Years. Never for My Own."
date: 2026-03-26
categories: blog
permalink: /blog/acceptance-testing-vibe-coded-app/
excerpt: "User stories, acceptance criteria, regression testing — I write these for other people's products for a living. It took building my own app to realise I hadn't written a single one for it."
---

*Part 9 of 9 — **Building an app with Claude: a non-developer's journey***

---

I write acceptance criteria for a living.

As a PM, I've written hundreds of them. *As a guardian, I want to submit a monthly report, so that I can document the hours worked.* That format is muscle memory at this point. I've pushed teams to write them before a single line of code is touched. I've blocked releases because the criteria weren't clear. I know what they're for.

I hadn't written one for this app.

Not one. Sixteen features built, some of them tested manually, none of them formally documented. It didn't feel like a gap until someone asked me to list the user stories for acceptance testing — and I opened a blank file and realised I was starting from scratch.

They came quickly. I know the format. Thirty minutes and I had sixteen stories across login, dashboard, assistants, reports, settings, FK compliance. Solid first pass.

Then: "What about registration?"

I'd missed it entirely. Not in the code — registration existed. In my thinking. I'd gone straight to what the app does once you're in, and skipped the part where you get in at all. Two stories added: guardian self-registration and assistant invite-based setup. Edge cases included — duplicate email, expired invite link, weak password.

A gap in the criteria, not the code. Those are the ones that matter most. You find them before you find the bugs.

---

The PDF section took longer.

There's a button in the app that generates a PDF of the monthly report. The PDF gets printed, signed, and mailed to FK. I had one test case for it: *did it download?*

That's not a test. That's checking that a file exists.

The actual questions are: does the patient name in the PDF match the patient name in Settings? Does the month on the PDF match the month of the report — not today's date, not the approval date? Does the total at the bottom equal the sum of all the daily entries? Does the right assistant's name appear on the right report, and not bleed across to another?

Seven test cases for one button. And one of them — the last one — can't be automated at all.

```
### US-12g — PDF signature field is present and printable

> ✋ Manual — must not be skipped
> 1. Download the PDF for an approved report
> 2. Open it in Preview or a PDF viewer
> 3. Confirm there is a labelled signature field for the assistant
> 4. Confirm there is space for a date next to the signature
> 5. Print one page and confirm the field is legible and has enough
>    physical space to sign by hand
> 6. Confirm result before this test is marked as passed
```

The signature field has to be checked by hand, on paper, with a pen nearby. No browser tool can tell you whether there's enough physical space to write in. That one is mine to do every time.

---

What shifted the whole conversation was one observation: these aren't just acceptance tests. They're a baseline. Every time a new feature is added, this list is what you run against to make sure nothing broke in the process.

That's regression testing. And once you see it that way, a document in a folder isn't enough. You need something that runs the tests and tells you what passed.

We talked through whether to build skills for it. The case for yes: the pattern is fixed — run the tests, flag the failures, pause on the manual ones, save a report. The case for two skills instead of one: writing new test cases and running existing ones are triggered at different moments. You write when you build. You run when you want to verify.

Two skills built:

**`kalinga-test-writer`** — triggered when a feature is done. Knows the format, knows the FK rules, knows the PDF requirements. Appends new stories to the file. Tags each one: automated, manual, or mixed.

**`kalinga-test-runner`** — triggered before a release or after a significant change. Runs the full list in journey order. Produces a report. Doesn't skip the manual tests — it stops, gives you the steps, waits for confirmation.

The test type tags are now on every story in the file:

```
| Tag          | Meaning                                                  |
|--------------|----------------------------------------------------------|
| 🔁 Automated | UI state, page content, redirects, API responses         |
| ✋ Manual     | Printed output, visual layout, mathematical spot-checks  |
| 🔁/✋ Mixed   | Some criteria automated, some manual                     |
```

Twenty-two stories. Fourteen automated. One manual. Seven mixed.

The one that's purely manual is the signature field. Every version of the app, I print a PDF and check that there's space to sign it. That's the test.

---

*[← Part 8: The Instruction Sheet I Wish I'd Written From Day One](/blog/the-instruction-sheet-i-wish-id-written-from-day-one/)*
