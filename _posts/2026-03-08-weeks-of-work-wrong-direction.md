---
layout: post
title: "Vibe Coding Mistake: Why I Built Wireframes Before Reading the Requirements"
date: 2026-03-08
categories: blog
permalink: /blog/vibe-coding-mistake-wireframes-before-requirements/
excerpt: "The first vibe coding mistake I made: building wireframes before reading the requirements. Weeks of work before I solved the actual problem."
---

*Part 2 of 7 — **Building an app with Claude: a non-developer's journey***

---

The first thing I did was start designing screens.

I know how that sounds. I've spent years telling teams not to do this — not to fall in love with the interface before you understand the problem. Discovery first. Always. It's one of those rules that's easy to follow when you're managing someone else's work and harder when it's yours.

I had a clear picture in my head of what the app should look like — a weekly calendar, assistant cards, a clean dashboard with summary numbers — and I started describing it to Claude and ChatGPT. Both were helpful. Maybe too helpful. They generated layouts, suggested colour palettes, proposed interaction patterns. I refined things. I iterated. I went deep.

I built out the calendar view in detail — not code, but a clickable prototype. I mapped out the flow for how an assistant would log their hours, what would happen if their entry got rejected, how they'd resubmit. There were several screens for that flow alone.

The thing kept getting more detailed, so I kept feeling like I was making progress.

Then I sat down with my husband — who fills in the FK forms with me every month — and walked him through what I'd made. He looked at it carefully. He asked a few questions. Then he said, more or less: the scheduling part looks very thought-through, but I can't see where the forms actually get filled in.

He was right. I'd been designing the parts that felt interesting to design and had left almost untouched the part that would solve the actual problem. The forms. The hour reporting. The monthly submission. Everything I was building around was there in detail, but the centre was missing.

It's the same trap I've seen teams fall into for years. You start where you feel confident, not where the problem is. I'd skipped the step I know matters most: understand what you're actually trying to solve before you decide how it should look.

I closed the prototypes. I asked Claude to read Försäkringskassan's actual documentation instead. That's where the real build started.

---

*[← Part 1: I Spent Years Correcting the Same Forms. Then I Vibe Coded an App.](/blog/vibe-coding-personal-assistance-paperwork/) · [Part 3: I Built Everything I Could Think Of. Then I Cut Most of It. →](/blog/vibe-coding-feature-creep/)*
