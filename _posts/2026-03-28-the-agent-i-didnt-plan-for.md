---
layout: post
title: "The Agent I Didn't Plan For"
date: 2026-03-28
categories: blog
permalink: /blog/ai-agents-compliance-knowledge-expert/
excerpt: "I always planned to build AI agents — for sales, marketing, support. I didn't expect the first one to be a compliance expert. And I didn't expect it to arrive before the app had a single user."
---

I always knew I'd build agents.

Sales, marketing, support. The operational parts of running a product — the work that takes time but follows patterns. I always assumed those would come after the app was live. Once there were users. Once there was something to operate.

What I didn't expect was a compliance agent.

## How it arrived

Today, reviewing user stories about hour limits, I asked whether we should create a Claude Skill to store the FK knowledge. The answer was no — wrong tool (read the previous post). But the question underneath was different: what if something *lived* with the rules, stayed current as they changed, and could answer questions about them?

That's not a Skill. That's an agent.

## What an agent actually is

A regular AI interaction is a conversation. You ask, it answers, it stops.

An agent is an AI that takes a sequence of actions on its own — research something, read a document, make a decision, update a file, flag a problem — without you driving each step. You give it a goal. It figures out how to get there.

The compliance agent I'm thinking about would know the Swedish rules for personal assistance, answer questions during the build, notice when regulations change, and flag which parts of the app are affected. Then the same agent, re-trained, for Norway. For Finland. For Denmark. For any market where Kalinga operates.

## The thing I didn't clock until today

Care providers who run personal assistance at scale don't just employ assistants. They employ experts. A compliance officer. Someone who tracks labor law changes and translates them into operational decisions. Someone whose job is knowing what the rules mean in practice, not just what they say on paper.

In Sweden, that knowledge infrastructure is what makes it possible to run at scale. It's not optional.

Kalinga is software, not a provider. But it's operating in the same regulatory environment, for the same kinds of arrangements. If it's going to be useful across multiple assistants, multiple households, eventually multiple countries — it needs the same knowledge layer.

It just gets to build it with agents instead of headcount.

## What this looks like

The compliance agent is the first. It knows the rules. It answers questions during development. It flags changes when regulations update.

After that: a sales agent that handles outreach to families looking for support. A marketing agent. An operations agent that monitors what's running and surfaces what's off.

None of this replaces human judgement at the moments that matter. But most of what experts do isn't the critical moments — it's the routine work that makes the critical moments possible.

I was going to start building agents after the app launched. It turns out the first one is a file and a few rules away from where we already are.
