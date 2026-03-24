---
layout: post
title: "The Things I Built and Then Took Out"
date: 2026-03-20
categories: blog
excerpt: "There's a version of this app that does a lot of things I'll never use. Removing things felt different to how I expected. It mostly felt like clarity."
---

*Part 4 of 5 — **Building an app with Claude: a non-developer's journey***

---

There's a version of this app that does a lot of things I'll never use.

Assistants could browse a list of available shifts and claim the ones they wanted. Each hour logged could be tagged with an activity type — bathing, eating, transport, physiotherapy. There was a whole tab in the navigation called Hours that had its own calendar view, its own reports section, its own way of managing blocked time. All of it built, none of it essential.

I don't think building those features was a mistake exactly. I think I built them because I was imagining an app for a situation I hadn't fully thought through yet — a more complicated setup, more assistants, more coordination overhead. The self-booking feature makes a lot of sense if you have eight assistants and a complicated rota. I have a small team. We talk to each other.

Removing things felt different to how I expected. I thought it would feel like loss — time spent that came to nothing. It didn't, really. It mostly felt like clarity. Each thing I cut made the thing that remained feel more purposeful. Like the app was becoming more itself.

There was one conversation with Claude that I keep coming back to. I asked whether the Hours tab needed to exist — it was a navigation item I kept forgetting was there, and I wanted an honest answer rather than a list of options. Claude said no, the functionality already lived elsewhere, and removing it would make things cleaner. Then, before I could even ask, it left the relevant line of code commented out with a note:

```tsx
// { to: "/hours", label: "Hours", icon: Clock }, // kept for easy revert
```

That small thing changed how I thought about cutting. You can remove something and leave the door open. The cost of trying is low.

What stuck with me was the test I started applying after that: does this help someone get the monthly FK forms submitted correctly? If not, it's not for now. Maybe later, maybe never, but not now. That question cleared out more half-finished ideas than I expected. I'm glad it did.

---

*[← Part 3: The App That Did Too Much](/blog/the-app-that-did-too-much/) · [Part 5: I Didn't Know the Password to My Own App →](/blog/i-didnt-know-the-password-to-my-own-app/)*
