---
layout: post
title: "How I Turned Acceptance Tests Into a Regression System"
date: 2026-03-26
categories: blog
permalink: /blog/acceptance-testing-results-vibe-coded-app/
excerpt: "I ran my first acceptance test on the Kalinga app. Sixteen features, two bugs found in five minutes, and two user stories that described an app that no longer existed."
---

*Part 11 — **Vibe Coding for Non-Developers***

---

I finished the app. Sixteen features. Tested by clicking around and checking that things looked right.

Then I ran an actual test suite against it.

Two bugs in five minutes.

---

## What the Test Runner Did

In the previous post I wrote sixteen user stories and built two Claude skills: one that writes test cases whenever a new feature is done, one that runs the full list and produces a pass/fail report. This week I ran the runner for the first time against the live app.

It checks twenty criteria across the full flow — registration, login, the dashboard, reports, assistants, the PDF generation, FK deadline calculation. Some automated, some mixed, one that requires me to physically print a page.

The results:

| | |
|--|--|
| ✅ Pass | 16 |
| ❌ Fail | 2 |
| ⚠️ Story mismatch | 2 |
| ✋ Manual pending | 1 |

Not bad for a first run. Also not releasable.

---

## The Two Bugs

**Bug one:** wrong password, no error message. If you type the wrong password on the login screen, the form resets. No message. No explanation. The network log confirms a 401 — the server knows it's wrong. The UI just says nothing. A user would have no idea what happened.

**Bug two:** no assistant detail page. Clicking an assistant's name does nothing. The route `/assistants/:id` doesn't exist. I had a user story for it. The feature was never built.

Both are logged now as GitHub Issues with the exact acceptance criteria from the user stories. That connection — from story to test to issue to fix — is the loop working the way it's supposed to.

---

## The Drift Problem

The more interesting finding wasn't the bugs. It was the mismatch.

Two user stories — US-09 and US-16 — described a daily hours form. "Assistant fills in hours for each day of the month." That's not what the app does. At some point during development the design shifted to a shift-based calendar model, where assistants log individual shifts with a start time, end time, and activity type. The form described in the stories doesn't exist and shouldn't exist. The shift model is better.

But the stories never caught up.

This is the kind of thing that's easy to miss when you're building alone. The code evolves. The documentation doesn't. By the time you run a test against a user story that describes the wrong thing, you've been working with the mental model of the old design for so long that it takes a moment to even notice the gap.

The fix is simple: rewrite the stories to describe what the app actually does. But noticing the gap required running the test.

---

## Testing the PDF Without Printing It

The PDF tests were the most interesting to work through. The FK 3059 form is the thing the whole app exists to produce — a legally required document, printed, signed, mailed. Getting it wrong matters.

Seven test cases for one button. Six of them I could verify by reading the server route — the code that generates the PDF. Patient name pulled from the same profile that Settings displays. Month fields set from the work month selected in the UI, not today's date. Totals calculated only from approved shifts. Each assistant's PDF scoped strictly to their own entries.

The seventh test case — whether the signature field is legible when printed, whether there's enough physical space to sign — can't be automated. I have to print it. That one is still pending.

---

## The Regression System

The test runner saves a dated report each time it runs. Today's is `2026-03-26-v0.1-acceptance.md`. The next time I run it, it will compare results to this one and flag anything that changed — a test that was passing and now isn't, or a bug that's been fixed.

That's the regression system. Not a CI pipeline. Not automated nightly runs. Just a skill that remembers what the last run looked like and tells me when something shifts.

It cost one afternoon to build. It found two bugs the first time I used it. I'll take that.

---

*[← Part 10: I Told It to Tell Me What It Learned](/blog/how-to-improve-ai-custom-instructions/)*
