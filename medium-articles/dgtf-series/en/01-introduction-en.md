# Why Good Developers Make Bad Decisions Under Pressure

## And the Three-Part Framework That Changed How I Build Software

---

![image](https://i.ibb.co/C5K0Tv1h/02.png)

**It's Friday afternoon, 4 PM.**

Your manager walks over. "There's a bug in production. Can you fix it before you leave?"

You open the code. Your heart rate increases. You feel the pressure building.

And then it happens.

You reach for a global variable. You hardcode a value. You skip the test. You tell yourself: "I'll fix it properly on Monday."

Monday comes. You've forgotten. The technical debt remains. The cycle repeats.

**Sound familiar?**

After 24 years of building software, I've watched this pattern destroy more projects than any technical challenge. Not because developers lack skill. Not because they don't know better.

But because something happens to our thinking when pressure hits.

---

## The Discovery That Changed Everything

In my early years as a developer, I kept asking the same question: *Why do smart developers make dumb decisions?*

I'd watch talented engineers—people who could explain SOLID principles in their sleep—reach for quick hacks the moment deadlines loomed. I'd see carefully architected systems turn into spaghetti code in the final weeks before delivery.

I thought it was a discipline problem. A motivation problem. A skill problem.

I was wrong.

The answer came from an unexpected source: **Daniel Kahneman**, a psychologist who won the Nobel Prize in Economics.

---

## System 1 vs System 2: The Science Behind Bad Decisions

Kahneman discovered that humans have two modes of thinking:

**System 1: Fast Thinking**
- Automatic and unconscious
- Requires no effort
- Relies on patterns and intuition
- Dominant under stress

**System 2: Slow Thinking**
- Deliberate and conscious
- Requires focus and energy
- Analytical and logical
- Deactivates under pressure

Here's the critical insight: **Under pressure, System 1 takes over.**

When your manager asks "when will it be done?" and your heart rate spikes, your careful, analytical System 2 thinking shuts down. Your fast, reactive System 1 grabs the first solution that comes to mind.

"Global variable!"
"Singleton!"
"Hardcode it!"
"Tests later!"

These aren't conscious decisions. They're **System 1 reactions**—fast, automatic, and often wrong.

---

## The Real Problem Isn't Knowledge

Think about it.

You know Clean Code principles. You understand SOLID. You've heard of TDD.

**So why don't you follow them when it matters most?**

Because knowing *what* to do isn't the same as knowing *how* to do it under pressure.

```
What most developers know:
✓ Clean Code
✓ SOLID principles  
✓ Test-Driven Development
✓ Design patterns

What most developers lack:
✗ How to follow them when deadline hits
✗ How to resist System 1 impulses
✗ How to maintain quality under pressure
```

This gap—between knowing and doing—is where projects fail.

---

## Three Theories, One Framework

Over the years, I found two more pieces of the puzzle.

**Donella Meadows** taught me systems thinking—how to see the whole instead of just the parts. She showed me that the most powerful intervention point is at the design level, before implementation begins.

**Genrich Altshuller** taught me TRIZ—how to resolve contradictions instead of compromising. "Fast" versus "quality" seems like a tradeoff. But what if you could have both?

Combined with Kahneman's insights about human thinking, these three theories became the foundation for a practical framework:

| Theory | Question Answered |
|--------|-------------------|
| Kahneman | **Why** do we make bad decisions? |
| Meadows | **What** should we design? |
| Altshuller | **How** do we resolve contradictions? |

---

## Introducing ROD, TFD, and DGTF

From these theories, I developed three methodologies that have transformed how I build software:

### ROD: Responsibility-Oriented Design
**"More is better than missing"**

Design complete service chains before implementation. When you know exactly what to build, there's no confusion during implementation. No confusion means no System 1 panic. No panic means no bad decisions.

### TFD: Test-First Development  
**"Requirements = Tests"**

Treat tests as specifications, not afterthoughts. When tests define what "done" means, you have immediate feedback. Fast feedback catches System 1 mistakes before they become problems.

### DGTF: Don't Go Too Fast
**"Slow is smooth, smooth is fast"**

Maintain System 2 thinking even under pressure. Recognize when System 1 is taking over. Pause. Think. Then act.

---

## The Paradox: Going Slower Is Actually Faster

Here's what surprised me most:

```
Rushing Developer (System 1 dominant):
├── Day 1: Fast coding, no tests (3 hours)
├── Day 2: Bug fixing (4 hours)
├── Day 3: More bugs (3 hours)
├── Day 4: Refactoring (5 hours)
└── Day 5: Adding tests (2 hours)
Total: 17 hours, unstable code

Thoughtful Developer (System 2 maintained):
├── Day 1: Design + test design (2 hours)
├── Day 2: Careful implementation (3 hours)
├── Day 3: Integration testing (2 hours)
├── Day 4: E2E testing (2 hours)
└── Day 5: Documentation (1 hour)
Total: 10 hours, stable code
```

**The thoughtful approach saves 7 hours (41%) and produces better code.**

This isn't theory. This is what I've observed across hundreds of projects over 24 years.

---

## What's Coming Next

This article is the first in a series exploring ROD, TFD, and DGTF in depth.

**In the next article**, I'll dive deep into ROD—how to design complete service chains that eliminate the "missing pieces" that trigger System 1 panic.

You'll learn:
- Why "more is better than missing" works
- How to build service chains that prevent confusion
- Why prohibiting constructors and static fields actually helps
- Real examples from production systems

**The goal isn't to work harder. It's to think better—even when everything is pushing you to think fast.**

---

## Try This Today

Before the next article, try this simple exercise:

**When you feel pressure building** (heart rate up, "hurry" thoughts repeating):

1. **Recognize** it: "System 1 is activating"
2. **Pause**: Take your hands off the keyboard
3. **Ask**: "What would I do if I had more time?"
4. **Then**: Do that thing anyway

It takes 30 seconds. But it might save you hours of rework.

---

*This article is part of a series on maintaining software quality under pressure. Based on 24 years of software development experience and validated theories from Kahneman, Meadows, and Altshuller.*

**Next: ROD—Design Complete, Implement Fast →**

---

*Have you experienced System 1 taking over during a deadline? Share your story in the comments. And follow me for the rest of this series.*
