# WHI: The Dual Tool — Dismantling Nouns

*IWD Deep Dive Series — Part 4*

---

![image](https://i.ibb.co/ns87LLYP/w04.png)

## The Owner Still Speaks in Nouns

You found the real "I."

You know whose problem you're solving. You've set your possession hypothesis. You're ready to build.

Then the owner opens their mouth:

> "I need a blinking feature."
> 
> "We need better demand forecasting."
> 
> "The system should use Oracle."

**Nouns.** These aren't user intentions—they're solutions, implementation methods. Even after finding the real owner, they still speak the wrong language (the language of means).

**How do you excavate the verb buried deep under the noun?**

---

## WHI: "What Happens If (not)?"

There's one question that tears through nouns like a shovel through dirt:

> **"What happens if you don't have that?"**
> 
> **"What happens if you can't do that?"**

This is **WHI**—"What Happens If (not)?"

WHI is a dual tool:

| Role | Function |
|------|----------|
| **Excavation** | Dig through surface requests (nouns) to find real requirements (verbs) |
| **Verification** | Test if the requirement is real, if the problem actually exists |

---

## Why "WHI" Instead of "Why"?

| | Why? | WHI? |
|---|------|------|
| **Focus** | Justification | Consequence |
| **Psychology** | Feels like interrogation | Feels like collaboration |
| **Output** | Noun (rationale) | **Verb (real intent)** |

**Why:** "Why do you need Excel export?" → *(feels like: "Are you questioning me?")* → "To make reports." (Stuck on Excel. Conversation ends.)

**WHI:** "What happens if there's no Excel export?" → "I spend 4 hours every week copying and pasting manually." (Verb uncovered. Conversation continues.)

**"Why" is a screwdriver. "WHI" is a power drill.**

---

## Role 1: Excavation — The Truth Behind the Blinking Task

A project manager came to me with a requirement from the customer. This system was a Gantt chart-style production scheduling UI, with hundreds of tasks displayed as boxes across the screen.

**The Surface Request:**

> "The search result tasks need to **blink**. The thick border isn't enough to catch the eye."

This is a noun, a solution, an implementation detail. The PM thought he was the one who 'delivers requirements' and I was the one who 'makes things.' But WHI doesn't stop.

1. **"What happens if there's no blinking?"** → "The customer can't find the searched task well."
2. **"What happens if they can't find it?"** → "There are too many tasks with different colors."
3. **"How many?"** → "About 200 to 300."
4. **"Will blinking solve that problem?"** → Silence.

**The Real IWD Excavated:**

- **Surface:** "Want blinking feature" (noun)
- **Essence:** "I wanna DO **identify** results at a glance" (want to instantly identify results among hundreds of tasks)

**The Result:** The PM suggested a **bigger monitor** to the customer instead of feature development. The blinking feature was quietly deleted from the requirements list. **Problem solved without writing a single line of code.**

---

## WHI Variant: The Replaceability Test

WHI has a powerful variant:

> **"If there were another way to solve this, would you still need [the noun]?"**

In Case 1:

> "If there were another way to find the task easily, would you still need blinking?"
> 
> "...No."

This question instantly reveals whether the noun is the goal or just a means:

| Answer | Meaning |
|--------|---------|
| "Yes, I absolutely need it" | The noun is essential—or they're addicted to their solution |
| "No, other ways would work too" | The noun is just a means—the verb is what matters |

This connects directly to Part 2's **Noun Eraser Test**. If you erase the noun and the requirement still makes sense, the noun was never the real requirement.

---

## Role 2: Verification — The Happy Journey to Wrong Destination

This is what happens when teams skip WHI and grab the noun.

**The Surface Requests:**

> "We need to reduce warehouse costs."
> 
> "The demand forecasting accuracy is the problem."
> 
> "We need warehouse stacking optimization."

The team heard nouns. The team started building excitedly. The so-called **'happy journey to wrong destination.'**

**What the Data Actually Showed:**

- Items with **zero demand** held the highest inventory (mal-stock)
- Well-selling items had **no safety-stock policy**
- Temporary production increases were causing warehouse cost spikes

**The Real IWD:**

- **Surface:** "Better demand forecasting" (noun)
- **Essence:** "I don't wanna DO **pay extra storage fee**" → "I wanna DO **reduce stock as much as possible**"

**Without WHI:** The team would have built a perfect demand forecasting system, but mal-stock would remain unchanged, and warehouse costs wouldn't drop. **"Data doesn't lie, but human requirements can."**

---

## Signals for Finding the Real Problem

The **way** someone answers WHI provides a decisive signal about whether the problem is real.

| Response Type | Signal | Meaning & Action |
|---------------|--------|------------------|
| **Fluent** | Quick, specific, emotional | Real problem. Keep digging. |
| **Stuck** | Silence, vague, contradictory | Fake requirement or wrong direction. **Reset or dig in a different direction.** |
| **Won't** | "That's confidential." / "Just do it." | Political territory or out of scope. Document and move on. |

> If someone can answer a WHI question fluently—with specific numbers and real pain—you've found the real problem.

> If someone can answer a WHI question fluently—with specific numbers and real pain—you've found the real "I."

---

## The Declaration

1. **WHI is the shovel.** Every noun hides a verb. Dig until you find it.

2. **WHI is the test.** Fluent answers prove the problem is real. Stuck answers expose fake requirements.

3. **The best engineering is writing no code.** One WHI question can delete an entire backlog item. That's not laziness—that's **precision**.

4. **Data validates, WHI excavates.** When the owner speaks in nouns, extract verbs with WHI. If the extracted verb contradicts the data, you've just discovered a lie.

**The question "What happens if not?" is worth more than a thousand lines of code.**

---

Don't Go Too Fast. 🐢

---

## One Step Further: WHI Changes the "State" of Requirements

The WHI questions we ask aren't just for getting answers. Through questioning, we manage the **state** of requirements.

| State | Meaning |
|-------|---------|
| **[Estimated]** | A hypothesis about the problem. Not yet verified. |
| **[Verified]** | Fluent answer obtained through WHI. The problem is proven real. Safe to design. |
| **[Corrected]** | WHI revealed it was wrong or unnecessary. Discarded. (Real problem registered as new requirement) |

Record the state of requirements with each question. Just having the team recognize "this feature is still [Estimated]" can prevent more than half the tragedies of departing for the wrong destination.

*Part 6 will explore how to systematically track these states across your project.*

---

## What's Next?

WHI excavates verbs from nouns. But some nouns seem like unbreakable rocks—**laws, regulations, compliance**. These look like forced nouns, not someone's desire.

**Part 5: "Even Laws Can Be Verbs"** covers how  to handle the hardest requirements—when someone says "We have to" instead of "I wanna."

---

*Part 4 of the IWD Deep Dive Series.*

**Series:**
1. The Fear — Why direction matters
2. Requirement Inversion
3. Who is "I"?
4. **WHI: The Dual Tool** ← You are here
5. Even Laws Can Be Verbs 
6. State Tracking 

---

*What's your most satisfying WHI moment—when a single question deleted an entire requirement?*
