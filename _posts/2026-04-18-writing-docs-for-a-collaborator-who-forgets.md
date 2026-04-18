---
layout: post
title: "Writing Docs for a Collaborator Who Forgets"
date: 2026-04-18
permalink: /blog/writing-docs-for-ai-coding-assistants/
excerpt: "Writing docs for AI coding assistants isn't the same as writing them for people. It needs a reference doc in the repo, a code anchor, and a commit."
tags: [vibe-coding, claude-code, ai, planning]
---

> *"read up on how the hours can be used. this research has been conducted multiple times so you should capture it and document it so we can come back to it if it ever surfaces again."*

That was my note to Claude mid-session. Earlier that morning, Claude had proposed a new database column — `fk_decision_hours_per_day` — for the Swedish assistance-portal app. Plausible name. Plausible use case. Except it's not a field that exists on an actual Försäkringskassan beslut. Weekly hours, yes. Daily hours, no. Claude had made it up.

After the research came back, Claude's recommendation flipped:

> *"Drop `fk_decision_hours_per_day` from the roadmap. No such field is stated on FK beslut (weekly hours only, per 51 kap 9§ SFB)."*

Good catch. Except — this wasn't the first time Claude had researched the FK rules for me. It wasn't the second either. [The first time I asked](/blog/fk-compliance-rules-reference-documents/) was a month ago, and I wrote a whole post about figuring out these rules belong in a reference doc, not a Claude Skill. What I didn't figure out then was how to make the reference doc actually get read. The second time Claude researched the FK rules was three weeks later. This was the third. Every session, the same research, the same citations, the same conclusions — rediscovered from scratch because the last conversation was gone.

That's when the note above came out. And that's when I realized writing docs for AI is a different job than writing them for people.

## Writing docs for AI coding assistants is a different job

When you document for a human, you're documenting *forward*. You write the thing so the person joining next week has a shortcut. You optimise for: easy to find, easy to skim, not outdated.

When you document for an AI coding assistant, you're documenting *sideways*. You're writing for a collaborator who starts every session from scratch. No context from yesterday's conversation. No sense of what's already been decided. Capable of re-deriving anything — but wastefully, at cost, and not always landing in the same place twice.

The fix isn't one artefact. It's three, together:

1. **A durable reference document** that the AI can re-read each morning — ideally committed to the repo where it travels with the code.
2. **A code anchor** — a comment next to the thing the rules apply to, pointing at the reference doc. Because if the AI doesn't know the doc exists, the doc doesn't exist.
3. **A commit hash** in the message trail, so there's a single checkpoint anyone can cite as "this is where that rule became real."

I did all three this session. Together they cost me maybe twenty minutes of extra work. The Swedish FK rules will never be re-researched from scratch again — the next Claude session will find the doc, read it, and carry on.

## What the pieces look like in practice

The reference doc lives at `docs/compliance/swedish-fk-and-labor-rules.md` — 448 lines, with section headers like *How FK beslut hours work*, *ATL §13 — Dygnsvila*, *Husligt arbete — the anhörig carve-out*. Quotes from primary sources. Source URLs. A changelog at the bottom. The front says, in plain language:

> *"When you touch anything that encodes an assistance-hour limit, a shift constraint, a semester entitlement, a sjuklön rule — verify it against this doc first. If the doc is wrong or outdated, fix the doc in the same PR as the code change. That's the deal."*

The code anchor lives in `server/src/db/schema.ts`, directly above the `profile` table:

```ts
// ── Profile ───────────────────────────────────────────────────
// NOTE: weeklyHours below = FK beslut hour entitlement (stated per week per 51 kap 9§ SFB).
// FK does NOT set a daily cap — labor law does (ATL or Lag 1970:943).
// See docs/compliance/swedish-fk-and-labor-rules.md for the complete rule set.
export const profile = pgTable("profile", {
```

Three lines of comment. They exist for one reader: the next Claude session that reads `schema.ts` and is about to make an assumption about what `weeklyHours` means. Without the comment, Claude would probably guess correctly most of the time and confabulate the rest. With the comment, the guess is bounded — Claude knows an authoritative doc exists, knows where it is, and can verify before writing code.

The commit is `765aedc`. Named in the phase context file, referenced by the planning docs, linked from the pull request. Anyone — human or AI — who wants to know when the rules became canon can point at that hash.

## I used to think custom instructions were enough

Earlier on in my vibe coding, I put rules in my Claude Skills. Things like voice preferences, file conventions. [I even wrote about teaching Claude to update its own memory](/blog/how-to-improve-ai-custom-instructions/). That's still useful. But it's the wrong container for domain rules like "FK beslut hours are stated per week."

Skills and memory files are about *how I work*. A reference document like the FK rules doc is about *how the world works*. It belongs with the code, not in my user-level Claude config, because:

- It's project-specific. Another project shouldn't inherit my Swedish tax trivia.
- It should be reviewable. Pull requests can audit it. Memory files can't.
- It should survive me. If I stop working on this app, the person or agent who picks it up finds the doc in the repo. They shouldn't need access to my `~/.claude/` to understand what the code is doing.

That last one is the real shift. I'd been treating my Claude setup as if it were the team. It's not. The team is the repo. Everything that needs to survive session boundaries belongs in the repo — committed, reviewable, versioned.

## What I'd Tell You to Try

**Notice the re-derivation.** The moment you think "we've had this conversation before" — that's a reference doc waiting to be written. Don't finish the conversation. Stop and write the doc first.

**Put the pointer where the code will be read.** A doc that isn't linked from the code is a doc that gets re-researched. A three-line comment is enough if it names the file and the rule.

**Commit it, don't stash it.** Reference docs in Notion or in your own memory don't travel with the code. If someone else opens the repo next month — human or AI — only what's committed can help them.

---

The collaborator I'm working with is unusually good at research and unusually bad at remembering the research. The docs I write for that collaborator look a little different from the docs I used to write. They're shorter. They're closer to the code. They include the primary sources verbatim. And they all have a line at the top that basically says: *yes, you are meant to read this before you do the thing.*
