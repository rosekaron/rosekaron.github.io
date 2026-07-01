---
layout: post
title: "When 1,000 Is a Warning, Not a Result"
date: 2026-07-01
permalink: /blog/n8n-datatable-pagination-silent-truncation/
excerpt: "n8n's getRows node has a default page limit. When your fetch returns exactly that many rows, you haven't confirmed success — you've triggered a canary."
tags: [vibe-coding, n8n, product-management, ai]
---

A row count of 1,000 in your n8n execution log looks like success. It might be a warning.

I was building a daily n8n workflow this week — fetches new chatbot feedback rows from a datatable, deduplicates them, runs them through an AI analysis pipeline. Standard automation logic. I enabled "Return All" on the `n8n_manage_datatable` getRows node, which is the right move: it tells the node to bypass its default page limit and retrieve every matching row.

And then I added one more node: a small Code node that checks the count.

That second decision took about five minutes to think through and maybe ten lines of code to implement. It's the one I want to talk about.

---

## The Silent Truncation Risk

n8n's built-in nodes have default row limits — community reports put the datatable getRows cap at around 1,000 rows. Enable Return All and the node should retrieve everything, bypassing that cap.

Should.

The issue isn't that Return All doesn't work. In most cases, it does. The issue is that *you can't tell whether it worked from the green execution indicator alone.*

If your workflow fetched 347 rows, that's an unambiguous result. 347 is not a limit. 347 rows exist and were processed.

But if your workflow fetched exactly 1,000 rows? That number is the cap. Getting back precisely 1,000 means either: you happened to have exactly 1,000 new rows (possible), or something in the node's behavior didn't honour Return All (also possible), or you've reached a scale where you should be paying closer attention (definitely worth knowing). You can't tell from a green node status which of those is true.

This is what I'm calling a pagination sentinel: **the exact page limit is not just a count — it's a canary value.** When your result equals the ceiling, you've learned something different from when it doesn't.

---

## What the Canary Node Does

Immediately after the fetch step, I added a Code node. It does two things:

1. **Logs the row count** to the execution output on every run — visible in n8n's execution history, easy to spot trends over time
2. **Flags a warning** when that count equals the configured page limit, adding a `possibleTruncation: true` field to the output

When the digest email goes out in a later phase of this workflow, that warning field conditionally adds a line to the email body: "⚠ Possible truncation detected — fetched count equals page limit." For now, in the early phases where I'm just proving the data pipeline, it sits in the execution data. The plumbing is there; the visibility comes later.

It's about making a silent assumption observable. The assumption — "Return All means we got everything" — isn't wrong. But assumptions you can't see are assumptions you can't catch when they break.

---

## The PM Instinct Underneath This

I've shipped metrics reports where the numbers looked fine because the pipeline that would catch *them not being fine* was itself broken. Quiet success. Green dashboards. Incorrect data.

In those situations, the failure mode isn't dramatic. No errors are thrown. The workflow runs. The report arrives. It's only when someone spots a discrepancy — a count that doesn't match a spot-check, a chart that's suspiciously flat for three days — that you realise the data was wrong all along.

Vibe coding is faster than those old pipelines. It's also no more immune to the gap between "the workflow ran" and "the workflow ran correctly."

What I've been building into every automation with a data fetch step is an explicit count check — something that makes the fetch's completeness a first-class observable, not an assumption. It's the same instinct that made me [add a checker agent to review my UI spec before any code ran](/blog/vibe-coding-design-rules-ai-audit/): AI doesn't announce its own gaps. You have to ask.

The canary node asks.

---

## What I'd Tell You to Try

**Add a count-check node after every data fetch step.** It doesn't need to be sophisticated — `items.length`, logged to execution output, is enough to spot anomalies over time.

**Know your page limits before your first production run.** Check the node's documentation and test with a dataset large enough to reach the cap. If "Return All" is an option, test what happens when you hit the natural limit with it enabled.

**Decide your failure signal before you need it.** If you fetch exactly the limit, does the run continue and flag a warning, or stop and alert? The decision is easier to make on a quiet Tuesday than on the morning the count first reads 1,000.

The sentinel doesn't prevent truncation. It makes truncation visible — which is the prerequisite for doing anything about it.
