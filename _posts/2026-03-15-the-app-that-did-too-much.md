---
layout: post
title: "The App That Did Too Much"
date: 2026-03-15
categories: blog
excerpt: "Once I started building properly, things moved quickly. Possibly too quickly. I was prompting Claude to generate code while making feature decisions, and the decisions came fast."
---

Once I started building properly, things moved quickly. Possibly too quickly.

I was prompting Claude to generate code while making feature decisions, and the decisions came fast because each one felt logical in the moment. Assistants should be able to claim open shifts themselves — that would reduce the back and forth. Each shift should have an activity tag so there'd be detailed records if anyone ever needed them. There should be a whole tab just for managing hours, with sub-sections for the schedule, the reports, and blocked-off time.

I went particularly deep on the self-booking system. I built a flow where the guardian could post open slots, assistants could see and claim them, and the guardian could either approve manually or set the system to auto-confirm. I added a setting for it with a toggle. Then two radio buttons for the approval mode. Then a dropdown for the booking window — seven days in advance, fourteen, twenty-one. The settings screen for this feature alone had four separate controls.

Here's what the database schema looked like after all of that. This is the table I built for the self-booking feature:

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

A whole table. A capacity column, in case multiple assistants wanted the same slot. I was building infrastructure for a coordination problem I had never actually experienced.

The activity tags got similar treatment. I defined a list of types — active assistance, waiting time, standby, sick leave — built a colour-coded label that appeared on every shift, added a filter for it. It looked good. FK's forms do not have a field for activity type. I was designing a reporting layer for an audit that was never going to happen.

The moment of reckoning arrived slowly. I kept noticing that when I thought about what actually needed to happen — the monthly process, submitting the forms, getting paid — most of what I'd built wasn't part of it. The self-booking system existed alongside the real workflow, not inside it. The activity tags were data I was collecting for no one.

I wrote down the minimum the app needed to do: record that an assistant worked certain hours on a certain day. Approve those hours. Generate the forms. That was the whole list.

Everything else was a guess about a version of the problem I didn't have yet. I started cutting. That turned out to be its own education.
