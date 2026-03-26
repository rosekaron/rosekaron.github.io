---
layout: post
title: "Debugging a Vibe Coded App: Silent Bugs, Wrong Field Names, and a Missing Password"
date: 2026-03-24
categories: blog
permalink: /blog/debugging-vibe-coded-app/
excerpt: "I locked myself out of my own app. Then found silent bugs on every page. Debugging a vibe coded app: wrong field names, missed FK deadline, blank dashboard."
---

*Part 5 of 7 — **Building an app with Claude: a non-developer's journey***

---

Last weekend, I sat down to test whether the whole thing actually worked end to end.

The first step was logging in. I typed in what I thought the password was. Wrong. Tried a variation. Wrong. Tried the version I use when I'm trying to be more careful. Wrong.

I had locked myself out of my own app. Claude's fix was to generate a new encrypted password and update it directly in the database:

```sql
UPDATE auth
SET password_hash = '$2b$10$...hashed...', email_verified = true
WHERE email = 'your@email.com';
```

It took about a minute. I now have a test password, which is `test1234`, which I'm sharing because it's a local development environment and also because it feels like an accurate representation of where I am.

## Silent Bugs on Every Page

Once I was in, the dashboard showed nothing. No entries, no assistant names, no hours. Blank cards. The data existed — I could see it if I looked at the database directly. The app just wasn't showing any of it.

Claude found the problem and explained it clearly. The backend was sending data with field names in one format. The frontend had been written to expect them in a different format. One side used `weeklyHours`. The other side was looking for `weekly_hours`. Two different conventions for writing the same thing, and I'd been inconsistent from the beginning.

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

Every page had some version of this. Silently broken, the whole time. Everything had looked fine on the surface because I'd never pushed hard enough on whether the data was real.

We went through the files one by one. I understood most of the fixes. I made a note to be more consistent about this going forward, while accepting that I probably won't be.

---

## The FK Deadline Was Wrong

Once the data was loading, I started checking whether the logic was right. Claude offered to read Försäkringskassan's actual documentation — pull the relevant pages, summarise the process. It came back with a clear explanation of how the two forms work, what counts as reportable time, and when everything is due.

It also found a bug I hadn't caught. I had the FK deadline wrong. Reports are due on the 5th of the *second* month after the work month — not the next month. January's work is due in March, not February.

```tsx
// What I had:
const dueDate = new Date(now.getFullYear(), now.getMonth() + 1, 5);

// What it should be:
const dueDate = new Date(now.getFullYear(), now.getMonth() + 2, 5);
```

One number. Wrong every month.

---

## Rebuilding the Dashboard Around the Right Question

After fixing the data and the deadline, we rebuilt the dashboard. The old version had cards with numbers. It looked like a dashboard. But it wasn't answering the question that matters on the first of the month: am I ready to submit?

The new one does. A weekly grid showing who's working each day. A pending approvals indicator. A deadline countdown that turns amber then red.

As a PM I've written that brief a hundred times. It took building the wrong version first to know what the right one needed to say.

---

*[← Part 4: How I Decided Which Features to Cut from My Vibe Coded App](/blog/cutting-features-vibe-coded-app/) · [Part 6: Your Network Is a Vibe Coding Tool →](/blog/network-as-vibe-coding-tool/)*
