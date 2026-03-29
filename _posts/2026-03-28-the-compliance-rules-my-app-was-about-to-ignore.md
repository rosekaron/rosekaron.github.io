---
layout: post
title: "The Compliance Rules My App Was About to Ignore"
date: 2026-03-28
categories: blog
permalink: /blog/fk-compliance-rules-reference-documents/
excerpt: "I thought I needed a Claude Skill to store the FK rules. I was wrong — and learning the difference between Skills, memory, and reference documents changed how I think about building with AI."
---

We were halfway through reviewing user stories when the compliance question came up.

The app tracks hours. That part was obvious from day one. What wasn't obvious was which limits to track against. Before writing the story, I asked Claude to research the actual rules.

What came back wasn't what I expected.

## There isn't one limit. There are two.

There are two completely separate systems running in parallel.

The first is the care package — the total weekly hours approved by Försäkringskassan for the arrangement. That one I knew. The second is a per-assistant cap set by Swedish labor law. Each individual assistant has their own ceiling: 40 ordinary hours a week, up to 12 hours overtime across a rolling four-week period, and an annual overtime cap of 300 hours.

Since 2020, FK actively checks both. If one assistant's reported hours exceed their personal limit, FK withholds payment for those hours — not from the overall package. The guardian pays the assistant. FK does not reimburse.

That second part I didn't know. Neither did the app.

## My first instinct was to create a Skill

In Claude Code, a Skill is a reusable set of instructions you invoke on demand with a slash command — like `/commit` or `/review-pr`. You write it once, you call it when you need it. It runs inside your active session and has access to your codebase.

Think of it like a trained reflex. You set it up once and trigger it when you need it. Good for: generating a PDF, running a code review, writing a blog post in a specific voice.

Not good for: storing reference information you want to consult, or that needs to stay up to date as rules change.

What I needed wasn't a reflex. It was a reference.

## Skills, memory, and reference documents — three different tools

This took me a while to get clear on, so here's the distinction:

| Tool | What it is | Use it for |
|------|-----------|-----------|
| **Skill** | A reusable action, invoked by name | Repeatable tasks with a consistent output |
| **Memory** | Facts that carry across conversations | Context, preferences, who you are, what you're building |
| **Reference document** | A file Claude reads automatically | Rules, regulations, specs that change over time |

The FK rules needed a reference document — a file in the project that Claude reads automatically, that any developer can update, and that stays current when regulations change.

## How other AI tools handle reference documents

This is where things get interesting — and where the tools differ a lot.

**Claude Code** treats every file in your project as potential context. No upload step, no special filename, no configuration. You drop a file in the project folder and the AI reads it automatically in every session. `docs/fk-rules.md` exists because I created it. Claude reads it because it's there.

**ChatGPT** requires manual uploads. In Projects, you upload files through a browser — but if you update the document locally, ChatGPT still has the old version until you upload again. There is no sync. Custom GPTs let you build a dedicated assistant with permanent knowledge files, but it is more setup than most projects need.

**Gemini** works similarly. You can attach Google Drive files to a conversation, but you have to re-attach them each session. Gems (Gemini's version of a Custom GPT) can hold permanent knowledge, but they are a product-building tool, not a developer workflow.

One Google tool worth calling out separately: **NotebookLM**. It is built specifically around reference documents — you add sources, and it only answers from those. Excellent if you need a non-developer to query documents in plain language. A care coordinator who wants to ask "what is the overtime limit for an assistant?" without touching code could use it. It cannot write code or do tasks — it is a reading tool, not a building tool.

**GitHub Copilot** has a file called `copilot-instructions.md` that loads automatically from your project — structurally the closest to what Claude Code does. But it is designed for short rules, not long compliance documents.

**Cursor** has `.cursorrules` — same idea, same limitation. A file in the project that loads automatically, best kept concise.

The short version: Claude Code is the only tool where a reference document is just a file. Every other tool requires either manual uploads, re-attaching per session, or a special filename that works best when short.

## A few things worth knowing

- **Skills are for actions, not knowledge.** If your Skill mostly contains facts — use a reference document instead.
- **Memory is for context that doesn't change often.** Who you are, how you work, what the project is.
- **Reference documents are for rules that might change.** Regulations, API specs, compliance requirements. One file. Update it when things shift.
- **The AI reads what's in the project.** You don't have to mention the file or paste it into the chat. It's just there.

The FK rules are now in `docs/fk-rules.md`. The app reads from them when generating reports. When the rules change — and in Swedish labor law, they do — I update one file.

## The thing the reference document can't do

After I created the file, someone asked: does that mean you've built an agent?

No. And the distinction is worth understanding.

A reference document is passive. It sits in the project. Claude reads it. It helps when consulted. But it doesn't know when it's out of date. It won't tell you if FK changes a rule. It won't flag which parts of your app are affected. You have to pick it up.

Here's where it fits in the full picture:

| Tool | What it is | Acts on its own? |
|------|-----------|-----------------|
| **Reference document** | A file Claude reads when working | No — only useful when consulted |
| **Memory** | A standing reminder I follow | No — only triggers in conversation |
| **Skill** | A reusable action, invoked on demand | No — only runs when called |
| **Agent** | An autonomous actor with a goal | Yes — monitors, decides, acts without being prompted |

The agent version of this file would monitor Försäkringskassan publications, detect when a rule changes, update the document, and flag which parts of the app are now out of compliance — without being asked.

That's the jump. From something you consult to something that watches.

I haven't built that yet. But having the rules written down, structured, and in the codebase means there's something for an agent to work with when the time comes.

The app was going to ship without any of this. I'm glad the question came up before it did.

*If you're curious where the agent idea goes from here — [that's the next post](/blog/ai-agents-compliance-knowledge-expert/).*
