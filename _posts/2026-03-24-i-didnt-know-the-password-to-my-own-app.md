---
layout: post
title: "I Didn't Know the Password to My Own App"
date: 2026-03-24
categories: blog
excerpt: "Several months into building this, I sat down to test whether the whole thing actually worked end to end. The first step was logging in. I typed in what I thought the password was. Wrong."
---

Several months into building this, I sat down to test whether the whole thing actually worked end to end.

The first step was logging in. I typed in what I thought the password was. Wrong. Tried a variation. Wrong. Tried the version I use when I'm trying to be more careful. Wrong.

I had locked myself out of an app I'd been building for months. Claude's fix was to generate a new encrypted password and update it directly in the database:

```sql
UPDATE auth
SET password_hash = '$2b$10$...hashed...', email_verified = true
WHERE email = 'mj@karon.se';
```

It took about a minute. I understood roughly what it was doing. I now have a test password, which is `test1234`, which I'm sharing because it's a local development environment and also because it feels like an accurate representation of where I am.

Once I was in, the dashboard showed nothing. No entries, no assistant names, no hours. Blank cards. The data existed — I could see it if I looked at the database directly. The app just wasn't showing any of it.

Claude found the problem and explained it clearly. The backend was sending data with field names in one format. The frontend had been written to expect them in a different format. One side used `weeklyHours`. The other side was looking for `weekly_hours`. They're just two different conventions for writing the same thing in code, and I'd been inconsistent about which one I was using from the very beginning.

Here's what it looked like — the broken version on top, the working version below:

```tsx
// What I had written (looking for the wrong names):
const weekly        = profile?.weekly_hours ?? 129;
const guardianFirst = profile?.guardian_name?.split(" ")[0];
entries.filter(e => e.req_status === "approved" && e.rep_status === "pending")

// What the data actually looked like coming from the database:
const weekly        = profile?.weeklyHours ?? 129;
const guardianFirst = profile?.guardianName?.split(" ")[0];
entries.filter(e => e.reqStatus === "approved" && e.repStatus === "pending")
```

Every page had some version of this. It had been silently broken, probably for a while, and everything had looked fine on the surface because I'd never looked carefully enough to notice that the data wasn't real.

We went through the files one by one. I understood most of the fixes. I made a note to be more consistent about this going forward, while accepting that I probably won't be.

---

Once the data was loading, I started checking whether the logic was right. Claude offered to read Försäkringskassan's actual documentation — go to the website, pull the relevant pages, summarise the process. I said yes. It came back with a clear explanation of how the two forms work, what counts as reportable time, and when everything is due.

It also found another bug. I had the FK deadline wrong. I thought reports were due on the 5th of the month after the work month. They're due on the 5th of the *second* month after — so January's reports aren't due in February, they're due in March.

Here's what was in the code:

```tsx
// What I had — deadline set to 5th of the following month:
const dueDate = new Date(now.getFullYear(), now.getMonth() + 1, 5);

// What it should be — 5th of the month after that:
const dueDate = new Date(now.getFullYear(), now.getMonth() + 2, 5);
```

One number off. In practice it meant the countdown was showing the wrong date every month. A real difference if you're planning around it.

---

After fixing the data and the deadline, we rebuilt the dashboard. The old version had some cards with numbers on them. It looked like a dashboard. But I kept asking: if I sat down on the first of the month to do the FK submission, would this page help me? The honest answer was: not really.

The new one has three things. A seven-day grid showing which assistant is working each day and how many hours. A section showing whether anything is waiting for my approval before I can submit. A countdown to the FK deadline that turns amber when it's getting close and red when it's urgent.

That's it. I didn't arrive at that design by thinking about good dashboard design. I arrived at it by finally understanding the process well enough to know what mattered.

Next up: testing everything properly against a real checklist of user stories, and then building the assistant-facing side of the app. That's the half I've been putting off. The guardian side is nearly solid. Time to build for the people actually doing the work.
