---
layout: post
title: "How I Decided Which Features to Cut from My Vibe Coded App"
date: 2026-03-20
categories: blog
permalink: /blog/cutting-features-vibe-coded-app/
excerpt: "Removing features from a vibe coded app felt different to how I expected. Not loss — clarity. Here's how I applied an MVP test to decide what to keep and what to cut."
---

*Part 4 of 7 — **Building an app with Claude: a non-developer's journey***

---

There's a version of this app that does a lot of things I'll never use.

A self-booking system. Activity tags on every entry. A whole Hours tab with its own calendar and reports. All of it built, none of it essential.

I don't think building those features was a mistake exactly. I think I built them because I was imagining an app for a situation I hadn't fully thought through yet — a more complicated setup, more assistants, more coordination overhead. The self-booking feature makes a lot of sense if you have eight assistants and a complicated rota. I have a small team. We talk to each other.

Removing things felt different to how I expected. I thought it would feel like loss — time spent that came to nothing. It didn't, really. It mostly felt like clarity. Each thing I cut made the thing that remained feel more purposeful. Like the app was becoming more itself.

There was one conversation with Claude that I keep coming back to. I asked whether the Hours tab needed to exist — it was a navigation item I kept forgetting was there, and I wanted an honest answer rather than a list of options. Claude said no, the functionality already lived elsewhere, and removing it would make things cleaner. Then, before I could even ask, it left the relevant line of code commented out with a note:

```tsx
// { to: "/hours", label: "Hours", icon: Clock }, // kept for easy revert
```

That small thing changed how I thought about cutting. You can remove something and leave the door open. The cost of trying is low.

---

*[← Part 3: I Built Everything I Could Think Of. Then I Cut Most of It.](/blog/vibe-coding-feature-creep/) · [Part 5: Debugging a Vibe Coded App →](/blog/debugging-vibe-coded-app/)*
