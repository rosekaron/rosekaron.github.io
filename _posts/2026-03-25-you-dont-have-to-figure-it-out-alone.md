---
layout: post
title: "You Don't Have to Figure It Out Alone"
date: 2026-03-25
categories: blog
excerpt: "At some point last Sunday, Claude installed Python in my Node.js project. I didn't catch it immediately. This happened more than once over the weekend."
---

*Part 6 of 7 — **Building an app with Claude: a non-developer's journey***

---

At some point last Sunday, Claude installed Python in my Node.js project.

I didn't catch it immediately. I was deep in trying to get a PDF to decrypt properly — one of those problems that sounds simple until you're three layers into it and nothing is working. Claude suggested a Python library. I said yes. It installed files, wrote scripts, ran them. For a few minutes it looked like progress.

Then I read what it had actually done.

I'm not a developer, but I know enough to know that a Python script in a JavaScript project is not a solution. It's a detour into a problem you now own. I said so, directly, and we deleted everything and started again.

This happened more than once over the weekend. Not always Python — sometimes it was a library that silently introduced a bug Claude didn't spot until much later. Sometimes it was an approach that worked on the surface and was broken underneath. The data on my dashboard looked fine for days. It wasn't real. Every page was reading the wrong field names and returning nothing, and nothing in the app flagged it. It only became visible when I looked closely enough to notice the numbers were always the same.

I don't have the technical depth to catch these things early. I can feel when something is wrong before I can name it — a result that's too clean, a fix that came too fast, a feature that works in the demo but never quite holds up when I poke at it. That instinct is something. But instinct without knowledge only gets you so far.

What I do have is a network. Friends who are engineers, product people, AI practitioners — people further along in this than I am, who I've watched build things and ask hard questions for years. When I hit walls I couldn't see through clearly, I texted them. Sent screenshots. Asked stupid questions without softening them.

Every single one of them answered.

Some of it was technical — someone pointed me toward the right approach for the PDF problem. Some of it was directional — a conversation about what the app actually needed to do versus what I was building. Some of it was just: you're not as lost as you think.

There's a version of this project where I tried to figure all of it out alone, got stuck, and quietly stopped. That version exists for a lot of people. I've seen it. You start something ambitious, hit the first wall that feels too technical to climb, and decide the gap is too wide.

The gap is usually smaller than it looks. And most of the people on the other side of it are happy to lean over.

---

*[← Part 5: I Didn't Know the Password to My Own App](/blog/i-didnt-know-the-password-to-my-own-app/) · [Part 7: Why I'm Writing This →](/blog/why-im-writing-this/)*
