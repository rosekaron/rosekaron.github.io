---
layout: post
title: "The App That Did Too Much"
date: 2026-03-15
categories: blog
excerpt: "Once I started building properly, things moved quickly. Possibly too quickly. Not out of confusion — out of genuine excitement about the problem."
---

*Part 3 of 7 — **Building an app with Claude: a non-developer's journey***

---

Once I started building properly, things moved quickly. Possibly too quickly.

I was prompting Claude to generate code while making feature decisions, and the decisions came fast — not out of confusion, but out of genuine excitement about the problem. A self-booking system so assistants could claim open shifts themselves. Activity tags on every entry so there'd be detailed records. A dedicated Hours tab with its own calendar, its own reports, its own blocked-time management.

I went particularly deep on the self-booking system because I genuinely liked the idea — assistants with a little autonomy, choosing shifts that worked for them, less back-and-forth for everyone. I built the whole flow. A toggle to enable it. Two radio buttons for approval mode. A dropdown for the booking window — seven days, fourteen, twenty-one. The settings screen for this feature alone had four controls.

Here's what the database schema looked like after all of that. This is the table I built for self-booking:

```typescript
export const openSlots = pgTable("open_slots", {
  id:          text("id").primaryKey(),
  date:        text("date").notNull(),
  startTime:   text("start_time").notNull(),
  endTime:     text("end_time").notNull(),
  hours:       real("hours").notNull(),
  capacity:    integer("capacity").default(1),  // how many assistants can claim
  activityId:  text("activity_id"),
});
```

A whole table. A capacity column. Built with care, for a coordination problem I had never actually experienced.

The activity tags got the same treatment. I defined the types, built colour-coded labels, added filters. It looked good. FK's forms have no field for activity type. I was building a reporting layer for an audit that was never going to happen.

This is a pattern I recognise. In product work there's a version of this that happens in every discovery phase — you find a problem, you get excited, and you start solving adjacent problems before you've validated the core one. It's not recklessness. It's what engagement looks like before you've applied the filter.

The filter, when I finally applied it, was a single question: does this help someone get the monthly FK forms submitted correctly? The self-booking system: no. The activity tags: no. The Hours tab: no.

I started cutting.

---

*[← Part 2: Weeks of Work, Wrong Direction](/blog/weeks-of-work-wrong-direction/) · [Part 4: The Things I Built and Then Took Out →](/blog/the-things-i-built-and-then-took-out/)*
