# IWD — When Perfect Code Solves the Wrong Problem

---

![image](https://i.ibb.co/BKsRyqmv/09.png)

You built it perfectly.

Clean architecture. Full test coverage. Documented. Reviewed.

Ship it. Done.

Three months later, the client says:

"That's not what I meant."

---

We've all been there.

The code was right. The solution was elegant. The execution was flawless.

But the destination was wrong.

---

## The Happy Journey to the Wrong Destination

I call this **"the happy journey to the wrong destination."**

You're moving fast. The team is motivated. Progress is visible. Everyone's happy.

Until you arrive.

And realize you've been building the wrong thing. Perfectly.

---

This is the most expensive mistake in software development.

Not bugs. Not technical debt. Not missed deadlines.

**Building the right solution to the wrong problem.**

---

## The Question Nobody Asked

"The dashboard isn't real-time. Please make the data update without refreshing."

The team dives in. High-performance WebSocket server. Real-time push logic. Now the numbers change instantly when data updates.

Ship it. Done.

But complaints keep coming.

Field worker: "The numbers update instantly, sure. But I still can't trust them. The dashboard says 'Approved' but when I click through, it says 'Pending'. I don't need fancy real-time animations. I need to know what I'm looking at is actually true."

Wait. This wasn't about push vs pull. It was about data consistency across systems.

The user said "not real-time" meaning "I can't trust this data." The developer heard "implement WebSocket." Months wasted on the wrong solution.

---

We heard the requirement.
We built the solution.
We solved the wrong problem.

---

## Nouns vs Verbs

Here's what I discovered after 25 years.

**Requirements spoken as nouns are usually wrong.**

"We need WebSocket." ← Noun (solution in disguise)
"We need encryption." ← Noun (solution in disguise)
"We need a dashboard." ← Noun (solution in disguise)

**Requirements spoken as verbs reveal the truth.**

"I want to trust the data I see." ← Verb (real problem)
"I want to delete my data." ← Verb (real problem)
"I want to see my progress." ← Verb (real problem)

---

The noun tells you WHAT to build.
The verb tells you WHY they need it.

**When you know WHY, the WHAT becomes replaceable.**

---

## IWD: I Wanna DO

This is IWD.

**I Wanna DO** — Express requirements as verbs.
**I Don't Wanna DO** — Express pain as verbs.

Simple. Almost too simple.

But this simple shift changes everything.

---

## WHI: The Excavation Tool

How do you find the real requirement behind the noun?

One question: **"What Happens If...?"**

But there's a sharper version:

**"If there were another way to solve this, would you still need [the noun]?"**

---

User: "We need encryption."
Developer: "What do you want to DO with encryption?"
User: "I don't wanna DO get penalized."
Developer: **"If there were another way to avoid penalties, would you still need encryption?"**
User: "...No, I guess not."

---

That one question just proved: encryption is not the requirement. It's a replaceable solution.

The real IWD: "I don't wanna DO lose our business license."

Now encryption is just one option among many: data masking, tokenization, minimization...

**WHI doesn't just excavate. It verifies replaceability.**

---

## The Process

Here's how it works:

```
User: "I wanna DO see real-time data" (first IWD - disguised solution)
    ↓ WHI: "What happens if it's not real-time?"
"I make decisions based on wrong information"

User: "I don't wanna DO make mistakes from bad data" (refined IWD)
    ↓ WHI: "If the data is guaranteed accurate when you click, 
            do you still need WebSocket?"
"Of course not! I just need to trust what I see."

Final IWD: "I wanna DO have reliable data"
```

Started with "real-time." Ended with "data reliability."

Now the solution space opens up:
- Fix data synchronization
- Add cache invalidation
- Show "last updated" timestamp
- Add manual refresh with guarantee

**One problem. Multiple solutions. All replaceable.**

---

## Whose Problem Is It?

The person in front of you might not be the real problem owner.

A project manager says: "We need faster response."
But who actually suffers? The end user. Re-logging in. Losing work.

**IWD asks: "Whose voice should I speak with?"**

This is what I call **Possession** — the skill of speaking as the real problem owner.

---

## The Fluency Test

How do you know if your possession is real?

Ask yourself: **"Can I say this IWD fluently and desperately?"**

**Stuck (Fake Possession):**
"Um... the law says we should... I guess we need encryption..."

**Fluent (Real Possession):**
"If our data leaks, we lose our business license. We have to prevent this!"

---

If you can't answer fluently, you haven't possessed the real owner yet.

**Fluency is verification.**

---

## [Estimated]: The Courage to Admit Hypothesis

Just like `// TODO` in code, mark your requirements with state:

- **[Estimated]**: "User seems to want to avoid re-login." (Still a hypothesis)
- **[Verified]**: "WHI confirmed re-login causes work loss." (Validated)
- **[Corrected]**: "WHI revealed it was wrong or unnecessary." (Discarded)

---

This small notation prevents overconfidence.

It creates room to change direction toward a better destination.

**Acknowledging uncertainty is not weakness. It's engineering discipline.**

---

## The Compass

IWD is a compass:

- **North**: Whose Problem?
- **South**: WHI?
- **East**: Verb (Wanna DO)
- **West**: Replaceability

Before you write code, check your compass.

---

## "How is this different from User Stories?"

User Story is a great tool. It shifted our focus from features to users.

"As a user, I want [goal], so that [benefit]."

IWD doesn't replace User Story. It adds a verification layer on top.

---

**User Story asks: "What does the user want?"**
**IWD asks: "Is this really what they want? How do we verify?"**

- WHI digs deeper into "so that"
- Fluency Test checks if you truly understand the user's pain
- [Estimated] reminds the team it's still a hypothesis

---

Think of IWD as a companion to User Story — a way to pressure-test your assumptions before you commit to building.

---

This article covers the core of IWD. The next series **"IWD: I Wanna DO"** will dive deeper into:

- **Collaborative Excavation**: How developers and domain experts dig together
- **Laws and Policies as IWD**: Turning rigid constraints into flexible design
- **State Tracking in Practice**: Managing [Estimated] → [Verified] across teams

---

*Don't Go Too Fast.* 🐢

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
7. [SNP: Your Little Step Also Worthy]()
8. [Can I Just Escape from a Dirty Pond?]()
9. **IWD — When Perfect Code Solves the Wrong Problem** ← You are here

---

*Connect with me:*
- *GitHub: [dont-go-too-fast](https://github.com/bakmeon/dont-go-too-fast)*
- *Email: stillblueist@naver.com*
