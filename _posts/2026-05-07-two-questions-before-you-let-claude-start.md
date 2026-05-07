---
layout: post
title: "Two Questions Before You Let Claude Start"
date: 2026-05-07
permalink: /blog/verify-claude-account-working-directory/
excerpt: "Before letting Claude write a single file, ask which account it's signed in as and which working directory it'll write to. Two PM-style checks most sessions skip."
tags: [vibe-coding, claude-code, product-management, ai]
---

A new engineer on day one gets two questions: which laptop they're on, and where they put their work. I now ask my AI assistant the same two questions — which account, which working directory — before any session, before "let's build X." For the same reasons.

## The session that almost wrote into my home directory

I'd just opened Claude Code in my terminal. The terminal happened to be sitting in `/Users/rosekaron/`. My first message wasn't "let's build the dashboard." It was:

> *"Before we begin, tell me the email address of the account that is logged in and where you are storing what we build."*

The reply confirmed the email but flagged that the working directory was my home folder. The init script for `/gsd-new-project` (a planning workflow I run) had already detected existing files there and wanted to mark the directory as brownfield. I was one confirmation away from creating a `.planning/` directory and a `CLAUDE.md` file in my home folder, on top of decades of unrelated files. The two-question intro caught it before any file was written. I asked Claude to create a project folder instead. Twenty seconds spent; a directory of accidental files prevented.

## Why account and working directory, in that order

Identity first, location second.

If the identity is wrong, you stop and fix it before anything else happens. Claude Code commits with whatever git credential helper is currently in scope. If you have two GitHub profiles set up — work and personal — the wrong one can sign your work without you noticing until a PR shows up in someone else's name.

If the location is wrong, you stop and `cd` somewhere sensible — or have Claude do it for you. Claude Code launches in whatever directory you opened the terminal in. That default is invisible until it's wrong.

Both are cheap to check at the start, expensive to unwind after the fact.

## Why AI changes the math

When a new engineer joins, you can shadow them for a day. You see them open the IDE, sign into git, navigate to the repo. The setup is observable. If anything goes wrong, you spot it.

Claude doesn't shadow well. You type a message and it responds. The intermediate steps — which account it operates as, which directory it writes into — are invisible unless you ask. The defaults pick themselves. By the time you notice the wrong path in a file tree, you've already created files in the wrong place.

That asymmetry is the whole reason the question matters. With a human collaborator, the wrong setup announces itself. With an AI collaborator, it doesn't.

## What I'd Tell You to Try

**Make this your first message in any new Claude session.** Not a system instruction, not a CLAUDE.md note. The literal first thing you type, before describing the work. It forces Claude to surface assumptions it would otherwise act on silently.

**Treat the answers as facts to verify, not promises.** Claude can claim it's writing to `~/myproject/` while actually writing to `~/`. Cross-check the first file path it produces against where you expected it to land.

**Capture the answers if the session is long.** If you're context-switching across days, write down which account and which directory somewhere quick to find. Future-you, three days later, will thank you.

The bigger habit underneath this: catch silent drift before it costs you. The same instinct that makes me run [a verification agent over my plan files](/blog/ai-coding-framework-multi-agent-workflow/) makes me ask Claude where it's standing before letting it move. The framework checks plans after they're written. The two-question intro checks the setup before anything is written at all. Both are cheap. Both prevent the kind of mistakes you only notice when they've already cost you something.
