# Who is "I"? — The Same Symptom, Three Different Owners

*IWD Deep Dive Series — Part 3*

---

![image](https://i.ibb.co/rKpDmT2C/w03.png)

## One Symptom, Three Requests

A works in the Operations Management Division. He's overwhelmed. Too much work, too little time. He's exhausted and afraid of making mistakes.

This is a fact everyone agrees on.

But three different people came to me about this "problem." And each one wanted something completely different.

---

## Request 1: From A Himself

A came to me directly.

> "I'm drowning. The workload is crushing me. I'm making mistakes because I'm exhausted. Can you help?"

**What A wants:** Relief. Less manual work. Automation. A better interface. Something that makes *his* life easier.

**Solution direction:** Automation scripts. UI improvements. Reducing manual steps.

---

## Request 2: From the Team Manager

The team manager called me into his office.

> "We have a problem. A is overloaded, and it's affecting the whole team's productivity. If he burns out or makes a critical error, we're all in trouble."

**What the manager wants:** Stability. He wants B to be able to help A. He wants parallel work capability. He wants the team to function even if A is sick or quits.

**Solution direction:** Restructure tables so B can work simultaneously. Add concurrent access. Build work queues.

---

## Request 3: From the Division Head

The division head stopped me in the hallway.

> "I've been thinking about A's workload situation. Is all that work actually necessary? Some of it seems redundant."

**What the division head wants:** Efficiency. He's questioning whether the work itself should exist. Maybe some tasks can be eliminated entirely.

**Solution direction:** Analyze business logic. Modify the system. Maybe *delete the feature entirely*.

---

## Same Symptom, Different Problems

| Who | What They See | What They Want | Solution |
|-----|---------------|----------------|----------|
| **A** | "I'm overwhelmed" | Make my work easier | Automate |
| **Manager** | "Team is at risk" | Make work distributable | Parallelize |
| **Division Head** | "Is this necessary?" | Eliminate waste | Delete |

Three people. Same symptom. **Three completely different problems.**

---

## The Trap: System 1 Wants to Code

When someone brings you a requirement, your **System 1** (intuition) immediately starts designing. Database schema. API endpoints. UI wireframes.

But this is exactly where Part 1's fear begins—the happy journey to the wrong destination.

Gerald Weinberg, in his classic *"Are Your Lights On?"*, reminds us:

> **"A problem is a difference between things as desired and things as perceived."**

The same symptom—A is overwhelmed—is perceived identically by all three people. But what they *desire* is completely different. That's why it's three different problems.

Weinberg also asks: **"Whose problem is it?"**

You may not know the answer at first. The real owner often emerges as you dig deeper—through a tool we'll explore in Part 4.

You must deliberately activate **System 2** (analysis) and ask:

> **"Who gave me this problem? What is that person really trying to DO?"**

A wants to survive his day.

The manager wants his team to survive A's departure.

The division head wants the organization to stop doing unnecessary work.

**If you don't know whose problem you're solving, you're building the wrong solution.**

---

## When Engineering Isn't the Answer

This is where Weinberg's book title becomes a method:

If people already *perceive* the problem clearly (the lights are on), you might not need a complex system at all. A simple reminder—a notification, a checklist, a conversation—might be the entire solution.

Weinberg puts it sharply:

> **"Don't solve problems for others that they could perfectly well solve for themselves."**
> 
> **"If it's their problem, make it their problem."**

If A's real issue is personal work habits, no system will help him.

If the manager's real issue is team communication, no code will fix it.

If the division head's real issue is organizational politics, no architecture can solve that.

**The most sophisticated engineering is recognizing when engineering isn't needed.**

---

## The Over-Solution Trap

If the division head is the true problem owner, but you build a sophisticated parallel-work system for the manager's request—what happens?

You've built an **over-solution**.

Worse: you've built infrastructure for work that might get eliminated next quarter. Your beautiful system becomes the next problem to solve.

Weinberg warns:

> **"Don't take their solution method for a problem definition."**

The manager said "parallel work capability." That's a solution, not a problem. The real problem might be: "this work shouldn't exist at all."

---

## Finding the True "I"

**Step 1:** Ask "Who brought this to me?"  
→ This is your starting hypothesis.

**Step 2:** Ask "What do THEY want to DO?"  
→ Not what they said. What they actually want to accomplish.

**Step 3:** Ask "Whose life changes if this is solved?"  
→ Who has the most at stake?

**Step 4:** Ask "Who can decide NOT to solve this?"  
→ The person who can say "never mind" is often the true owner.

---

## The Hierarchy of Solutions

When you identify the true problem owner, the solution changes completely:

| True Owner | Solution | Code Required |
|------------|----------|---------------|
| **A** | Automate his tasks | Medium |
| **Manager** | Enable parallel work | High |
| **Division Head** | Eliminate the work | **Zero** |

**The higher you go, the less code you might need.**

If the division head's question leads to "this work is unnecessary," the best solution is to write no code at all. Just delete the feature.

---

## The Declaration

Weinberg's bitter observation:

> "There's never enough time to do it right, but there's always enough time to do it over."

To break this tragic cycle, we declare:

1. **The messenger is not the owner.** Don't accept the voice of the person in front of you as "I." Find the real owner hiding behind the requirement document—the one truly suffering—and speak on their behalf.

2. **Possession is a hypothesis.** The moment you borrow someone's voice, it's an unverified hypothesis. If you hesitate or sense contradictions when saying "I wanna DO," you're mimicking the wrong person.

3. **Fluency is proof of ownership.** If you've found the real owner, you should be able to answer fluently: "Why must this problem be solved?" If you're stuck or vague, you haven't met the real "I" yet.

4. **"I" determines everything.** The destination and path change completely depending on who the subject is. Before writing a single line of code, clearly define whose desire you're coding.

**Wrong "I", wrong destination.**

---

Don't Go Too Fast. 🐢

---

## What's Next?

Now that you've found the true owner "I," you need a tool to excavate their vague desires into concrete requirements.

**Part 4** introduces **WHI—"What Happens If (not)?"**—the dual tool that both excavates real requirements and verifies your possession.

---

*Part 3 of the IWD Deep Dive Series.*

**Series:**
1. The Fear — Why direction matters
2. Requirement Inversion
3. **Who is "I"?** ← You are here
4. WHI: The Dual Tool 
5. Even Laws Can Be Verbs 
6. State Tracking 

---

*Have you ever solved the wrong person's problem? Share your story.*
