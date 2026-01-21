# Can I Just Escape from a Dirty Pond? : When Fear Rules Your Codebase

## Bringing Code Beyond Human Capacity Back Into Human Hands

---

*This is the eighth article in the series "Software Development Philosophy for Maintaining Quality Under Pressure."*

*This is not improvement. This is restoration for survival.*

---

**"Don't touch that part."**

Have you heard this before?

"It's working fine right now."
"I don't know why it works that way. It just works."
"The person who built it has left the company."

---

This is not just advice.

**It's a confession.**

This code has already exceeded human capacity.
No one understands it.
No one can predict it.
No one can control it.

We call this a **'Dirty Pond.'**

---

## Can I "Just" Escape?

Many developers ask:

"Can I **just** escape from a dirty pond?"

**Just.** Without much effort. Easily. Quickly.

---

Common excuses:

"If I had time, I'd fix it."
"I'll refactor it later."
"I'll do it in the next sprint."

---

The truth is harsh.

**Without skill and habit, you can't fix it even with time.**

Even if given time, you:
- Don't know where to start
- Don't know what to test
- Don't know how to isolate
- Are overwhelmed by fear

---

## Part 1: The Mechanism of Fear

### Fear as a Signal

How do you know you're in a Dirty Pond?

**Fear.**

```
When looking at code:
    "Can I touch this?"
    "Can I understand this?"
    "What if I make a mistake?"

When deploying:
    "No deployments on Friday"
    "Is rollback ready?"

When going on vacation:
    "What if something happens while I'm gone?"
```

Fear is an instinctive warning.
"This is beyond your capacity."

If you feel fear, you are in a Dirty Pond.

---

### How Fear Paralyzes Reason

The problem is not fear itself.
**The problem is how fear distorts our judgment.**

---

A Dirty Pond traps developers in a special state.

**Eternal Friday 4 PM.**

---

Imagine Friday at 4 PM.

Deployment is in one hour.
A bug is discovered.
Your heart races.
Your hands tremble.

At this moment, what happens in your brain:

```
System 1 (Instinct):
    → "Fix it fast! Anything!"
    → "Just make it work!"
    → "Think later!"

System 2 (Reason):
    → ... (paralyzed)
```

**Fear shuts down System 2.**

---

A Dirty Pond is this state **365 days** a year.

Every time you change a line of code, it's Friday 4 PM.
Every time you run tests, it's Friday 4 PM.
Every time you deploy, it's Friday 4 PM.

Chronic pressure.
Chronic fear.
Chronic System 1.

---

### The Entrenchment of Technical Debt

This is the real reason technical debt accumulates.

```
Fear → System 1 dominates
    → Write "just make it work" code
    → More complexity
    → Greater fear
    → Stronger System 1 dominance
    → ...
```

**A vicious cycle.**

Fear creates debt,
and debt feeds fear.

---

### Knowledge vs Habit

What survives in this vicious cycle?

**Knowledge evaporates.**

```
Clean Code?
SOLID?
Design Patterns?

→ Disappears when pressure hits
→ Shuts down with System 2
```

**Habit survives.**

```
The habit of pausing with DGTF
The habit of writing tests first
The habit of small commits

→ Works even in System 1
→ Because it's ingrained in your body
```

---

**"Knowledge betrays me under pressure, but habit saves me."**

We are not fixing the brain.
**We are fixing the habits of our hands.**

You cannot eliminate System 1.
But you can create habits that even System 1 follows.

---

## The Gatekeeper's Question

Before entering the operating room, there is one question.

Gerald Weinberg.
A pioneer of software engineering.
In his book "Are Your Lights On?" he asks:

**"I have a solution. But do you really want it?"**

---

It can be fixed. There is a way.

But do you **really** want it?

---

**The cost of repair:**

```
- You must write tests
- You must revisit the design
- You must go slowly
- You must accept discomfort
```

"I'll do it later" is just another way of saying no.

Only those who truly want it can pass through this gate.

---

## Part 2: The Process of Surgery

If your will is confirmed, the surgery begins.

The goal is clear.

**Beyond Human → Within Human**

Bring the code back within human capacity.
Understandable.
Predictable.
Controllable.

---

*This part covers an overview of the surgery.*
*Specific techniques and practical skills will be covered in depth in the next series **"FBS: Fix Broken Software."***

---

### Step 1: Objectifying the Problem

**"I Wanna DO fix, but I cannot touch it."**

I want to fix it. But I cannot touch it.

This is the starting point of repair.

---

**Why can't you touch it?**

WHI (What Happens If I touch it?):

```
"Something will break."
"What exactly?"
"...I don't know."
```

**Why must it be fixed?**

WHI (What Happens If I don't touch it?):

```
"Live in fear forever."
"Same bugs repeat."
"The pond gets dirtier."
```

---

The real problem becomes clear.

The problem is not the code.
**"I don't know" is the problem.**

---

**Don't misunderstand.**

"Not knowing" is not your incompetence.

The system made you not know.

```
No documentation
No tests
The creator is gone
Code layered like geological strata
```

The system is hiding information and rejecting you.
It's not a matter of your skill.

---

But the result is the same.

Because you don't know, you fear.
Because you fear, you don't touch.
Because you don't touch, you keep not knowing.

Breaking this cycle is the beginning of repair.

---

### Step 2: ROD - Excavating the Hidden Service Chain

Complexity beyond human capacity.
How do you bring it back within capacity?

**Discover the Service Chain within tangled code.**

Thousands of lines of spaghetti code.
Finding the single path where data flows through it.
This is ROD in fixing broken systems.

Legacy code is like geological strata.

```
Planning from 3 years ago.
An urgent bug fix from 1 year ago.
An unreasonable requirement from yesterday.

Layered upon layered.
```

But even within those strata, there is flow.
There are original responsibilities that the creators intended (or missed).

---

**Replacing parts on a moving train.**

Fixing broken systems deals with systems that cannot stop.

```
The train is running (service in operation)
Passengers are aboard (users exist)
It cannot stop (no downtime allowed)

But parts are worn (legacy)
They must be replaced (repair)
```

You cannot replace all parts at once.
**Find the core parts (Service Chain) and replace them one by one.**

```
ROD in new code:
    → Draw the blueprint
    → Create the Chain from the start

ROD in fixing broken systems:
    → On a moving train
    → Identify the core parts (Chain)
    → Replace them one by one
```

**This is precision work.**

```
1. Analyze legacy code
   → Trace the actual flow within the strata

2. Extract Service Chain
   → "What does this code actually do?"
   → Find only the core chain of responsibility

3. Erect barriers (Interface)
   → Set boundaries based on the Chain
   → Isolate worn parts behind interfaces
   → Secure space for new parts
```

When the Chain is visible, Replaceability opens up.

---

### Step 3: TFD - Safety Support

In TFD, tests are requirements.

What is the requirement in legacy?

**"Exactly as it currently works."**

It must not change.
Users expect it to work as it does now.
That is the requirement.

Therefore, you need a test that records the current behavior.

**Pinning Test.**

Michael Feathers introduced this technique in his book "Working Effectively with Legacy Code."
A must-read for any developer dealing with legacy code.

```
Before fixing code:
    Record the current input/output as-is
    "This is how it works now" = requirement
    
After fixing code:
    If the test passes → requirement maintained
    If the test fails → requirement broken
```

---

**"But I'm scared to even write tests?"**

I'm not talking about perfect unit tests.

```
Put in an input value
Log the output.
That's also a Pinning Test.
```

**Even a crude stake is fine.**

Better a crude stake than touching without one.

Remember SNP.
No test < One crude test < A better test.
Small steps have value.

---

Pinning Test is a psychological safety net.
Your only ally in the eternal Friday 4 PM.

---

**The moment you excavate the Chain (ROD) and drive in the stake (Pinning),**
**the code is no longer an 'unknowable monster.'**

It becomes a 'finite unit of responsibility' that fits in your head.
At this moment, the code returns to human control.

---

### Step 4: DGTF + SNP - Safe Stride

The greater the pressure, the slower you go.

But "slowly" is abstract.
**You need a concrete stride.**

---

**DGTF Rhythm:**

```
Before modification:
    Pause for 10 seconds
    "Am I going the right way?"
    
After modification:
    Run tests
    "Did I break something?"
```

Stop the rushing System 1.

---

**SNP Stride:**

Don't fix the whole function at once.
**Change one variable name and run the tests.**

```
Step 1: Rename variable → Test → Commit
Step 2: Extract function → Test → Commit
Step 3: Insert interface → Test → Commit
...
```

**Revert Rule:**

```
If the test breaks?
    → Don't think about it
    → Revert immediately
    → Break it into smaller pieces and start again
```

The moment you hesitate, System 1 intervenes.
"Ah, this much should be fine..."
**It's not fine.**

---

This is how you replace parts on a moving train.

```
Break it small
Test it
Commit it
If it breaks, revert
Break it smaller
```

A pond 1% cleaner than yesterday.
**Purification has already begun.**

---

## The True Meaning of Escape

What is true escape?

**It's not changing locations.**

```
"Let's rebuild from scratch" → You'll create the same pond again
"Let's change companies" → There's a pond there too
```

**It's gaining capability.**

```
- The skill to clarify code responsibility
- The skill to protect with tests
- The skill to make replaceable
- Habits that work under pressure
```

With capability, you can purify any pond.

---

## One Bucket of Clean Water

You cannot purify the entire pond at once.

But today you can pour one bucket of clean water.

```
One Pinning Test made today.
One responsibility separated today.
One barrier erected today.
```

The one bucket of clean water you pour today
is the foundation for tomorrow's **Replaceability**.

You cannot drain the entire pond,
but if you successfully isolate one Chain with an interface,
you have escaped from that part of the pond.

When these accumulate, the pond becomes clean.

---

**Do you really want to escape?**

If so, start today.

With one bucket of clean water.

---

*Don't Go Too Fast.*

*Crossing the hurdle is not the end.*
*Not crossing the hurdle is not the end.*
*We are on the road.*

---

*This is the eighth article in the series "Software Development Philosophy for Maintaining Quality Under Pressure."*

---

**Series:**
1. [Why Good Developers Make Bad Decisions Under Pressure]()
2. [ROD: Design Complete, Implement Fast]()
3. [TFD: Requirement = Test]()
4. [DGTF: Slow is Smooth, Smooth is Fast]()
5. [Putting It All Together]()
6. [Replaceability: The Ultimate Goal of Good Software]()
7. [SNP: Small Steps Have Value]()
8. **Can I Just Escape from a Dirty Pond? : When Fear Rules Your Codebase** ← You are here

**Next Series:**
- FBS: Fix Broken Software (Coming Soon)

---

*Connect with me:*
- *GitHub: [dont-go-too-fast](https://github.com/bakmeon/dont-go-too-fast)*
- *Email: stillblueist@naver.com*
