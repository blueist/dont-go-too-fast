# SNP: Your Little Step Also Worthy

## Why We're on a Road, Not a Ladder

---

*This article is part of the series "A Software Development Philosophy for Maintaining Quality Under Pressure."*

---

**You know you need tests.**

Does your system have enough tests?

---

You decided to write tests.

First, you designed what to test. For a single login feature: normal cases, error cases, boundary values, concurrency... After designing, you realized you needed dozens of tests.

You wrote a few. It was harder than expected. How do I create mocks? How do I test async? What about the database? How do I know if these tests are even correct?

Then doubt crept in. Even if I write all these, will it be enough? Am I missing any cases? What if bugs still appear despite having tests?

You calculated the time. One week? Two weeks? And you still have features to build.

A voice whispered in your head.

*"You won't be able to write them all anyway."*
*"What's the point of incomplete tests?"*
*"Better spend that time building more features."*

Finally, you said:

"I don't have time for tests right now."

Tests were postponed to "later." And later never came.

---

Why does this happen?

**Because we think "if I can't do it all, it's meaningless."**

---

## A Tale of Two Developers

**Developer A:**

```
Achieved 80% test coverage
    ↓
Bug found
    ↓
Fixed (no test added)
    "Tests are already enough. It's 80%."
    ↓
Another bug found
    ↓
Fixed (no test added)
    ↓
Another bug...
    ↓
Chaos
```

**Developer B:**

```
Achieved 80% test coverage
    ↓
Bug found
    ↓
"Bug found = tests weren't enough"
    ↓
Fix bug + add test
    ↓
Gradually stabilizing
```

**Both started at 80%. Different outcomes.**

What's the difference?

**Developer A:** 80% = Pass (goal achieved) → Done

**Developer B:** 80% = Got this far → "Can I go further?" → Continue

Same number, different interpretation.

This is SNP.

---

## SNP: Spectrum, Not Pass/fail

SNP is how you see the world.

```
Pass/Fail eyes:
    Crossed the hurdle = Success (done)
    Didn't cross = Failure (worthless)

SNP eyes:
    Crossed the hurdle = Got this far (continue)
    Didn't cross = But better than before (worthy)
```

**Pass/Fail is a ladder.** You climbed or you didn't. No in-between.

**SNP is a road.** Wherever you are, you're on the road. You can keep going.

---

## DGTF and SNP

You found a bug. Added a function to fix it. Tests passed.

**DGTF:**

"Wait, am I on the right track?"

Stop and look.

**SNP:**

Tests pass. But...
- Does this function belong here?
- Did I create a weird dependency?
- Is there a better way?

---

**DGTF makes you stop and ask.** "Am I on the right track?"

**SNP gives you guidance from your current state.** "It passes. And it can be better."

---

**DGTF without SNP:**

```
Bug found → Add function → "Wait" → Tests pass → OK, done
```

Stops at Pass. Asks nothing more.

**DGTF with SNP:**

```
Bug found → Add function → "Wait" → Tests pass...
    → But this created a weird dependency
    → Remove function → Redefine responsibilities → Reorganize → Tests pass
```

Asks even after Pass. Finds a better direction.

**DGTF must hold SNP to be complete.**

---

## You Already Know This

The Fox and the Grapes.

Jumped. Couldn't reach.
Jumped again. Couldn't reach.
"Those grapes are probably sour anyway."
Left.

**The Fox: "If I can't reach it, it's meaningless."**

---

The Tortoise and the Hare.

The hare was fast. Got ahead.
"This is enough."
Took a nap.
The tortoise passed by.
The hare lost.

**The Hare: "I've come far enough."**

---

As children, we knew.

The fox was wrong.
The hare was wrong.
The tortoise was right.

---

And yet.

"If I can't write all the tests, it's meaningless." ← The Fox
"Achieved 80% coverage, that's enough." ← The Hare

We cheer for the tortoise
while living like the fox and the hare.

---

SNP isn't new.

We learned it as children.
Be like the tortoise.

Even if slow, don't stop.
Every step has value.

We just haven't applied it to our work.

---

## Why Keep Going

Why didn't the tortoise stop?

Not simply "to win."

---

One test written today.
One weird dependency found today.
One responsibility clarified today.

Seems small.

But when these small things accumulate,
changing code tomorrow becomes less frightening.

---

Good software isn't "perfect software."
It's **"software that can be changed."**

Today's small step becomes
tomorrow's Replaceable Point.

There's no perfect system.
But there's a system that's "easier to change" than yesterday.

That's why the tortoise kept going.
That's why we must keep going.

---

## SNP and Burnout

Doesn't "keep going" cause burnout?

No. **SNP isn't a task. It's a mindset.**

**Pass/Fail mindset:** "Fail if I don't cross the hurdle" → Anxiety → Pressure → Burnout

**SNP mindset:** "Worthy even before the hurdle" → Can breathe → Can keep going

**SNP doesn't cause burnout. It protects from burnout.**

---

## Starting Tomorrow

When writing tests, receiving code reviews, working on projects:

**Don't just ask "Pass or Fail?"**

**Also ask "Got this far. Is there room to go further?"**

Specifically:

- Fixed a bug → "Should I add a test?"
- Completed a feature → "Any room for improvement?"
- Achieved coverage goal → "Any room to add more?"

One small question separates the fox from the tortoise.

---

*Crossing the hurdle isn't the end.*
*Not crossing the hurdle isn't the end either.*
*We are on a road.*

---

*This article is part of the series "A Software Development Philosophy for Maintaining Quality Under Pressure."*

---

**Series:**
1. [Why Good Developers Make Bad Decisions Under Pressure]()
2. [ROD: Design Complete, Implement Fast]()
3. [TFD: Requirements = Tests]()
4. [DGTF: Slow is Smooth, Smooth is Fast]()
5. [Putting It All Together]()
6. [Replaceability: The Ultimate Objective for Good Software]()
7. **SNP: Your Little Step Also Worthy** ← You are here

---

*Connect with me:*
- *GitHub: [dont-go-too-fast](https://github.com/bakmeon/dont-go-too-fast)*
- *Email: stillblueist@naver.com*
