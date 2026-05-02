---
layout: post
title: "My Accounting Notebooks, Now Inside Claude"
date: 2026-05-02
permalink: /blog/notebooklm-mcp-claude-accounting-sweden/
excerpt: "I'm connecting NotebookLM to Claude via MCP so questions about Swedish tax, migration, and social insurance get answers grounded in actual Skatteverket and Migrationsverket guidance — not just training data."
tags: [claude-code, ai, workflow, notebooklm, sweden]
---

The most useful AI setup I've built this year doesn't involve writing a single line of code. It's a connection between Claude and Google NotebookLM — linked via an MCP server — so I can ask Claude questions about Swedish bureaucracy that draw from official sources instead of just its training data.

I run an enskild firma. It works. It's simple — one set of finances, straightforward tax filing, no board meetings with yourself. But at some point the question becomes whether simple is still the right call, or whether switching to an aktiebolag would actually be more beneficial. The AB structure means corporate tax instead of income tax on profits, the option to retain earnings in the company, limited liability. It also means more administration, a separate legal entity, and figuring out how to pay yourself in a way that doesn't cancel out the tax advantage.

The right answer depends on revenue, costs, risk appetite, and a handful of Swedish tax rules I can never keep entirely straight.

I've asked Claude. I've gotten useful answers. I've also gotten answers that were confidently structured, cited general principles, and turned out to be slightly off about how Swedish rules specifically work. The gap isn't Claude's fault — it's trained on a lot of things, not primarily on Skatteverket's current guidance for small Swedish businesses.

The fix isn't to stop asking AI. The fix is to give it better source material.

---

## The Source Material Problem

Skatteverket publishes detailed guidance on deductibles, business structure, F-skatt, VAT thresholds, everything. Bolagsverket has its own documentation on company forms. This is all public, authoritative, and free. The problem is it lives across dozens of pages I'd have to hunt through every time I have a question.

NotebookLM solves the curation problem. You upload sources — in this case, the relevant pages from Skatteverket.se and Bolagsverket.se, PDFs of the guidance documents that matter to my specific situation — and then you ask questions. NotebookLM reads your sources and cites which document, which paragraph, which sentence it's drawing from. It doesn't fill in from training data when your sources don't cover something; it tells you the sources don't cover it.

That's exactly the behavior I want for questions about Swedish tax. I don't want an approximation. I want an answer I can trace back to an official document.

---

## What MCP Actually Does Here

The remaining problem was that NotebookLM and Claude lived in separate tabs. I wanted to ask accounting questions while I was already in Claude — while thinking about how to structure my consulting invoices, or whether a particular purchase belongs in the business or not — without breaking context to switch tools.

MCP is a standard for connecting Claude to external data sources and tools. I've written about [why I started using a workflow framework for AI coding](/blog/ai-coding-framework-multi-agent-workflow/) — MCP is part of that same ecosystem. In this case: a small server runs locally on my machine. Claude sends it a question and a notebook ID. The server asks NotebookLM, gets a cited answer, passes it back. From my side it just looks like Claude can read my notebooks.

---

## The Part That Surprised Me

I assumed Google would have an official MCP server for NotebookLM. It's a Google product, MCP is increasingly mainstream. This seemed like the kind of integration a product team would publish.

There is no official NotebookLM API. Not for personal accounts.

What exists is a community-built ecosystem. The most-used server — `notebooklm-mcp-cli`, currently at 4,100 GitHub stars with over a hundred releases since January this year — uses browser cookie extraction to talk to NotebookLM's internal endpoints. Google hasn't blocked it. They also haven't published it. The whole thing runs on undocumented internals that could break whenever Google ships a UI update.

This is apparently how AI tooling works in 2026. The gap between what these tools can do and what they officially support gets filled by people who want it badly enough to figure it out. The maintenance story is real though: 100+ releases since January means when Google breaks something, someone patches it within days. I'm betting on that.

---

## Two Things Worth Knowing Before You Set This Up

Authentication can't be scripted. You run a command that opens a real Chrome window, log in with your Google account manually, and it captures your session. That persists to disk for a few weeks. Annoying once, then forgotten.

The other thing — specific to Claude Desktop rather than Claude Code CLI — is that Claude Desktop doesn't inherit your shell's PATH. If you install the server via `uv` and point Claude Desktop at just `uvx`, it won't find it. You need the absolute path, the kind you get from `which uvx` in your terminal. I recognise this class of problem now. A year ago it would have stopped me cold.

---

## The Questions I'm Planning to Ask

Once this is running, my starting point is a notebook built from Skatteverket's guidance on deductibles for enskild firma owners, the comparison of enskild firma vs. AB at different income levels, and what the actual conversion process involves. Questions like:

- At what income level does the tax math shift in favour of AB for someone in my situation?
- What can I deduct as an enskild firma that I couldn't — or could more easily — as an AB?
- What are the real administrative costs of running an AB versus what the official guidance says they are?

I want those answers to come with citations to the Skatteverket page they came from. That way, when I take the question to my revisor, I'm not walking in with "Claude said" — I'm walking in with "Skatteverket says this, here's the page, what am I missing about my specific situation?"

---

## For Anyone New to Sweden

Accounting is just where I started. The same pattern works for any Swedish authority that publishes detailed public guidance — which is most of them.

Migrationsverket's website covers residence permits, work permits, the path from one status to another. Försäkringskassan publishes everything about sick leave, parental leave, and what benefits look like when you're self-employed rather than salaried. Skatteverket has more than just business tax — there's guidance on declaring foreign income, moving to Sweden mid-year, the rules for someone who's newly registered in the Swedish system.

All of it is public. All of it is authoritative. Most of it is in Swedish, which adds a translation layer on top of the already-steep learning curve of figuring out how the system works.

The NotebookLM + Claude approach is particularly useful here because the questions aren't one-off lookups — they're layered. *I got a decision from Migrationsverket. What does this status allow me to do? Does it affect my FK benefits? Can I register an enskild firma with this permit?* Those questions span three different authorities and two different notebooks. Being able to ask Claude while it holds context from multiple sources — rather than opening five tabs and synthesising manually — is the part that changes how much you can actually understand, not just find.

I think of it as building your own Swedish bureaucracy guide, one that reflects your actual situation rather than the generic overview. [The same principle I've applied to documentation for AI tools](/blog/writing-docs-for-ai-coding-assistants/) — the source material quality is the binding constraint — applies here. The official sources exist. The work is curating them into something queryable.

---

That's the setup. I'll write about the answers once I've actually run the questions.
