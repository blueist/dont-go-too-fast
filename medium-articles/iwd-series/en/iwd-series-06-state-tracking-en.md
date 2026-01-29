# State Tracking: IWD for Teams

*IWD Deep Dive Series — Part 6*

---

## You Can Do It Alone. But What About Your Team?

You now know IWD.

- How to find the verb behind the noun
- How to find the real "I"
- How to excavate and verify with WHI
- How to turn regulations into verbs

You can do it alone.

But Monday morning, in the team meeting:

> PM: "When will this feature be done?"
> 
> You: "Wait, I'm not sure who the real 'I' is for this requirement..."
> 
> PM: "Just follow the spec. The customer said it directly."

**IWD vanishes from your mind.**

---

## The Problem: What's Invisible Isn't Managed

Requirements usually have two states:

```
[ ] Not done
[x] Done
```

But reality is messier:

```
"I heard the customer wants this" → Did you verify?
"The PM said so" → Did you ask the real owner?
"It's in the spec document" → Is that spec correct?
```

**If it's invisible, you don't question it.**
**If you don't question it, you don't verify it.**
**If you don't verify it, you run in the wrong direction.**

---

## Status vs State: Different Questions, Different Answers

Most teams manage **Status** in Jira or Kanban boards:

```
To-do → In-progress → Done
```

This answers: **"How much work is done?"**

But IWD manages **State**:

```
[Estimated] → [Verified] → Implement
```

This answers: **"How much can we trust this requirement?"**

| | Status | State |
|---|--------|-------|
| Question | How much done? | How trustworthy? |
| Example | To-do, In-progress, Done | Estimated, Verified, Corrected |
| Measures | Progress | **Reliability** |

**You need both. But they're different.**

"Done" but [Estimated]? **That's completed garbage.**

---

## The Solution: Add "State" to Requirements

Code has `// TODO`.
Requirements need state too.

| State | Meaning | Next Action |
|-------|---------|-------------|
| **[Estimated]** | Hypothesis. Not verified. | WHI needed. **No development.** |
| **[Verified]** | WHI passed. Real problem. | Development allowed |
| **[Corrected]** | Discarded/Postponed. | None. Not for development. |

---

## The Engineer's 'Safe Refusal Right'

**[Estimated] means no development.**

This is not a rule. It's a **right**.

```
PM: "Finish this by Friday."

Engineer: "This feature is still [Estimated].
          We need the owner's answer to WHI questions.
          Coding without verification is gambling."
```

When the whole team recognizes this State, engineers can **legitimately refuse**.

Remember the anger from Part 1:

> "His mistake was his to make. The burden was mine to carry."

**The [Estimated] label is a safety mechanism that corrects this asymmetry.**

---

## Case: Real-time Dashboard

The team received a requirement:

> "Make the dashboard real-time."

**Without state:**

```
[ ] Real-time dashboard → Start implementing WebSocket
```

**With state:**

```
Requirement A: [Estimated] "Real-time dashboard"
    ↓
WHI: "What happens if it's not real-time?"
    ↓
Answer: "I can't trust the data"
    ↓
WHI: "If there's another way to ensure reliability, do you still need real-time?"
    ↓
Answer: "No, as long as it's accurate"
    ↓
Requirement A: [Corrected] — "Real-time" was wrong requirement (discarded)
    ↓
Requirement B: [Estimated] "Trustworthy data" (newly discovered)
    ↓
WHI passes
    ↓
Requirement B: [Verified] "I wanna DO trust the data I see"
```

**Key:**
- A goes to [Corrected] → trash
- B is new requirement → [Estimated] → [Verified]

**When [Estimated] is visible, the whole team knows "this is still a hypothesis."**

---

## State Transition Diagram

```
[Estimated] ──WHI passes──→ [Verified] → Create Task
     │
     └──WHI fails/unnecessary──→ [Corrected] (end)
```

**Rules:**

1. All requirements start as **[Estimated]**
2. Pass **WHI** → become **[Verified]**
3. If WHI reveals it's wrong or unnecessary → **[Corrected]** (no development)
4. Only **[Verified]** items can be developed

---

## Team Workflow

### 1. When Receiving Requirements

```
PM: "The customer wants Excel export"

Developer: 
"[Estimated] Excel export — customer request (via PM)"
```

→ Record with source

### 2. During Backlog Review

```
Team Lead: "We have 3 [Estimated] items. 
           Need WHI before this sprint."

Developer A: "I'll WHI the customer about Excel export."
Developer B: "I'll verify the dashboard with the PM."
```

→ When [Estimated] is visible, action follows

### 3. Sharing WHI Results

```
Developer A: "Excel export WHI results:
        
        Q: 'What happens without Excel?'
        A: 'It takes 4 hours to make reports'
        
        Q: 'If there's another way to make reports faster?'
        A: 'Don't need Excel then'
        
        Excel export → [Corrected] (discarded)
        New requirement: Reduce report time → [Verified]"
```

→ The process becomes transparent

### 4. During Sprint Planning

```
Team Lead: "Only [Verified] items go into the sprint.
           [Estimated] items are WHI targets this week."
```

→ Only verified items get implemented

---

## Tools: Start Simple

### Option 1: Tags in Ticket Titles

```
[Estimated] Excel export feature
[Verified] Reduce report generation time
```

### Option 2: Custom Fields

```
Add "IWD Status" field in Jira/Asana:
- Estimated
- Verified
```

### Option 3: Spreadsheet

| Requirement | State | Source | WHI Result |
|-------------|-------|--------|------------|
| Excel export | Corrected | PM | Not needed — report time is real issue |
| Reduce report time | Verified | PM (via WHI) | Confirmed |
| Real-time dashboard | Corrected | Customer | Not needed — data trust is real issue |
| Trustworthy data | Verified | Customer (via WHI) | Confirmed |

---

## Handling Resistance

### "We don't have time"

```
PM: "No time for WHI. Just do it."

Response: "It takes 10 minutes. 
          Compare that to building for 3 weeks 
          and hearing 'that's not what I meant.'"
```

### "The customer wants it"

```
PM: "The customer said it directly."

Response: "I'll record it as [Estimated], 
          and do one WHI with the customer.
          If it's correct, we change it to [Verified]."
```

### "The spec is already finalized"

```
PM: "It's in the contract."

Response: "Even laws can be verbs. (See Part 5)
          If we implement the intent, not the letter,
          the customer will be happier."
```

---

## Making It Team Culture

### Day 1-3: Organize

```
- Classify existing requirements: agreed → [Verified] / unclear → [Estimated]
- New requirement rule: start as [Estimated]
- Practice saying "this is still a hypothesis"
- No WHI required yet
```

### Week 1-2: Practice

```
- Start WHI on [Estimated] items
- Share WHI results
- Only [Verified] items enter sprints
```

### Month 2+: Habit

```
- Seeing [Estimated] triggers automatic WHI
- Team members question each other
- "Did you WHI?" "Is it [Verified]?" becomes natural
```

---

## Add This Question to Your Team

**"Is it [Verified]?"**

This one question:
- Confirms the real 'I' (Part 3)
- Confirms the verb (Part 2)
- Confirms WHI passed (Part 4)

Before starting development, this one question is enough.

---

## The Declaration

1. **What's invisible isn't managed.** Make requirement state explicit.

2. **[Estimated] isn't shameful.** Admitting you don't know is professional.

3. **Only implement [Verified].** Implementing hypotheses is gambling.

4. **State tracking is the team's survival manual.** Even if you're great alone, it's useless if the team loses direction.

**"Label your hypotheses. Only then can you verify them."**

---

Don't Go Too Fast. 🐢

---

## Closing the Series

Over 6 parts, we've journeyed through IWD together.

**Series:**
1. The Fear — Why direction matters
2. Requirement Inversion
3. Who is "I"?
4. WHI: The Dual Tool
5. Even Laws Can Be Verbs
6. **State Tracking** 

The tools are ready.

What remains is **practice**.

Tomorrow, in your team meeting, try saying:

> "This is [Estimated]. Should we WHI it?"

That one sentence is the beginning.

---

*Part 6 of the IWD Deep Dive Series (Finale).*

**Series:**
1. The Fear — Why direction matters
2. Requirement Inversion — Verbs over nouns
3. Who is "I"? — Finding the real owner
4. WHI: The Dual Tool — Dismantling nouns
5. Even Laws Can Be Verbs — Regulations hide verbs too
6. **State Tracking — IWD for Teams** ← You are here

---

*How does your team manage requirement states? Do you distinguish between [Estimated] and [Verified]?*
