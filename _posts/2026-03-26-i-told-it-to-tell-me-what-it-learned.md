---
layout: post
title: "I Told It to Tell Me What It Learned"
date: 2026-03-26
categories: blog
permalink: /blog/how-to-improve-ai-custom-instructions/
excerpt: "One instruction made my AI custom instructions smarter every session. The pro tip nobody mentions when setting up Claude Skills or ChatGPT Custom Instructions."
---

*Part 10 of 10 — **Building an app with Claude: a non-developer's journey***

---

At the end of a session, something appeared in the interface.

Claude had flagged a few things it picked up during our work — patterns worth remembering, approaches that changed the output — and asked which ones I wanted to carry forward into the skill.

![A checklist prompt asking which learnings to add to the skill](/assets/images/skill-learnings-checklist.png)

I hadn't asked for that. I'd given it one instruction earlier: *when you learn something new, ask me if I want to update the skill.*

That was it. One line. And it changed what the skill is.

---

If you've read the previous post, you know what a skill is — a saved set of instructions that tells your AI how to behave in a specific situation. You write it once, and the tool refers to it automatically. The idea is that you stop repeating yourself.

What nobody really talks about is what happens after you write it.

Most people write a skill, use it for a while, and leave it as-is. Which is fine. It still works. But a skill written on day one only knows what you knew on day one. It has no idea what you figured out on day twelve.

The checklist changes that.

---

Here's what the skill I use for these blog posts knows now that it didn't know when I started:

It knows to ask the purpose of a post before writing a single word. I learned this the hard way — Claude wrote a technically accurate post that completely missed the point because I hadn't explained why I was writing it. That failure is now in the skill. It won't happen again.

It knows that the most honest parts of my story are usually the moments where my professional instincts as a PM collided with my personal project. That I skipped user stories because I thought building alone meant the basics didn't apply. That I over-specified features not out of confusion but out of genuine product engagement. Those aren't generic observations — they're specific to me, and they're in the skill now because a session surfaced them.

It knows the SEO rules for publishing. How to write a slug that ranks. How to structure an excerpt so it front-loads the terms people actually search for. That came out of a session where we published a post and I asked why it wasn't showing up the way I expected.

None of that was in the original skill. All of it is there now.

![Claude updating the skill after being asked](/assets/images/skill-update-in-action.png)

---

The pro tip is simple: add one instruction to whatever skill you're building.

*When you learn something new in this session, ask me if I want to update the skill.*

That's all it takes. The tool will flag the moments worth capturing. You decide what goes in. Over time, the skill stops being a document you wrote once and becomes something that reflects how you actually work — not how you thought you worked when you started.

The skill I have now looks nothing like the first version. Same structure. Completely different depth. Every session that went sideways, every output that missed the mark, every question that changed the direction of a post — it's in there.

A static skill is a starting point. A skill with that one instruction is something that grows.

---

*[← Part 9: I've Written These for Years. Never for My Own.](/blog/acceptance-testing-vibe-coded-app/) · [Start from the beginning →](/blog/vibe-coding-personal-assistance-paperwork/)*
