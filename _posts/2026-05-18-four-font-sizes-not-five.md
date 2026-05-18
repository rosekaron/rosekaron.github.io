---
layout: post
title: "Four Font Sizes, Not Five"
date: 2026-05-18
permalink: /blog/vibe-coding-design-rules-ai-audit/
excerpt: "AI makes design decisions fast and invisibly. You can't audit them by eye the way you'd inspect your own work. So I set up a step where AI audits itself — before any code runs."
tags: [vibe-coding, ai, product-management, n8n]
---

> *"The spec's own note claims 'exactly 4' by treating 12 and 13 as one tier, but both are explicitly assigned as separate pixel values to different elements. Maximum allowed is 4."*

That's not me reviewing a spec. That's a checker agent reviewing a researcher agent's work — and refusing to accept the reasoning in the notes.

The researcher had written: "Exactly four tiers." The checker read the actual table, counted five distinct pixel values, and blocked the spec. That exchange is why vibe coding design rules need a separate verification step before any code is written. Not because the researcher agent is bad at its job — it drafted a solid spec — but because it has the same blind spot any author has: it can convince itself its rationalizations are true. The checker has no such investment.

---

## The Problem With Invisible Decisions

When I build something in React, I can open DevTools and inspect every design decision the AI made. Computed font size. Padding. Stacking order. If Claude wrote 12px where the spec said 13px, I'll see it in the panel and fix it live.

A lot of AI-generated UI doesn't work that way. An email template is a string. An n8n webhook response is a string. A PDF rendered by a library is a string. The "component" doesn't exist until it renders — and by then, the inconsistencies are already baked in.

I'm building an internal dashboard served as an HTML page from an n8n webhook. The styling lives in an embedded `<style>` tag inside a JavaScript string in a Code node. There is no component tree. If the design is inconsistent, I'll catch it the way I catch anything wrong with a rendered page: by looking carefully, knowing what to look for, holding the spec in my head. That's a weak audit. It depends on my attention and on me spotting a 1px discrepancy in a browser window.

So I moved the audit earlier — before the string is written — and made AI do it.

---

## What the Vibe Coding Design Rules Actually Caught

Before any Code node is written, a researcher agent drafts a `UI-SPEC.md`: typography scale, spacing scale, color roles, copywriting, component layout. A checker agent then reads the spec against six dimensions. Blocking issues go back to the researcher for revision. Only when the checker approves does code get written.

This phase produced two blocks.

**Five font sizes, not four.** The spec declared sizes across badge text, footer, labels, title, and values — five distinct pixel values. The researcher's notes called this "exactly four tiers" and argued that two sizes were effectively the same tier. The checker's response was the quote above: both are explicitly declared as separate values, maximum is four, blocked.

Fix: consolidate two sizes into one. The 1px difference between a footer and a label isn't a visual tier worth encoding. It's drift. Four sizes now.

**Badge padding not on the spacing scale.** The badge used `2px 6px`. The spacing scale uses only multiples of four. Fix: `4px 8px`. Visually identical at that size. On the scale.

Neither would have been obvious in the rendered output. They're the kind of thing that makes an interface feel slightly off without anyone being able to say why — and exactly what a human reviewer would most likely let slide as "close enough."

The checker is not capable of "close enough." That's the point.

---

## The PM Version of the Same Idea

In product, there's a class of decision that looks harmless in isolation and consequential at volume. A font size here. A spacing value there. Copy that says "No results" instead of telling you what to do next. Each one is a rounding error. Together they're the gap between a product that feels considered and one that feels assembled in a hurry.

The answer was never to tell developers to be more careful. It was to build systems that enforced consistency — design tokens, component libraries, copy guidelines, review gates. You remove the decision from the moment of writing and put it into a shared constraint that applies whether or not anyone remembers it.

AI-assisted development has the same volume problem, faster. An agent building a UI makes dozens of small decisions per minute. Most are fine. Some drift. The only reliable way to catch the drift is to have the constraint documented before the code runs, and to have something check it that doesn't share the author's interest in letting small violations pass.

The checker flagging the researcher's self-justifying note — "exactly four tiers, trust me" — is that gate working. [It's the same reason I don't let a single agent plan and verify its own work](/blog/ai-coding-framework-multi-agent-workflow/). Not because the agent is incompetent, but because authors are poor auditors of their own decisions. That's not an AI problem. It's just true of anyone who wrote the thing they're reviewing.

---

## What I'd Tell You to Try

**Lock design constraints before you write any styled output.** Type scale, spacing scale, color roles — decide them once, in a document, before any code. The constraint doesn't know what runtime it's governing. It applies to a React component and to a string of HTML in a Code node in exactly the same way.

**Have a second agent check the first agent's work.** The researcher that drafts the spec and the checker that validates it should be separate — separate context, separate prompt, no shared investment in the outcome. The researcher will rationalize. The checker should not be able to.

**When the checker blocks, trust the block.** The researcher said four tiers. The checker said five values. The checker was right. Fix the exact thing named and move on.

The font size was 1px off. It would have been in production HTML I couldn't easily patch in DevTools. Small thing. But small things are exactly what the process is for.
