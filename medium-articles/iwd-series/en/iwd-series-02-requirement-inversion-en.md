# Requirement Inversion: The Moment You Name the Solution, You've Lost the Problem

*IWD Deep Dive Series — Part 2*

---

![image](https://i.ibb.co/PZL9750v/Gemini-Generated-Image-ikn7hxikn7hxikn7.png)

## The Wall

I saw it coming.

The specification said "database required." But the data we needed to store barely changed. Maybe once a month. Maybe less.

I walked to the requirement analyzer's desk.

"Do we really need a full database for this? The data is mostly static. A simple file would work."

He didn't look up.

> **"It's set in stone. No way to go back."**

The conversation was over.

---

## The Cost of a Noun

Because someone wrote "DB" in a document, the customer paid for:

- A commercial software license they didn't need
- An engineer they didn't need to hire
- Infrastructure they didn't need to maintain

The data barely changed. A text file would have been enough.

But "DB" was written. "DB" was defined. And nouns, once named, become walls.

---

## Two Walls, Same Prison

In Part 1, after the disaster, the requirement analyzer said:

> "Sorry. There's nothing I can do."

In Part 2, before the disaster, he said:

> "It's set in stone. No way to go back."

| Wall | When | Effect |
|------|------|--------|
| "Set in stone" | Before building | Blocks questions |
| "Nothing I can do" | After failure | Blocks accountability |

The noun creates the first wall. The failure creates the second.

**IWD breaks the first wall—before the noun is even written.**

---

## Why Nouns Win

Why do requirement analyzers cling to nouns?

Because nouns are easy to manage.

"Implement Oracle database" can be checked off a list. "Manage data efficiently" cannot. Nouns fit neatly into contracts and project plans. They give managers a false sense of control.

This is **Contractual Coupling**—when administrative convenience kills engineering judgment.

And there's a deeper reason. When someone says "We need a database," your brain translates instantly:

| What was said | What System 1 heard |
|---------------|---------------------|
| "Database" | Oracle. MySQL. PostgreSQL. |
| "Storage" | Servers. Backups. Replication. |

The noun feels concrete. Defined. Safe.

But that safety is an illusion. The problem isn't defined—it's buried under an assumption you never questioned.

**"The moment you name the solution, you've lost the problem."**

---

## Requirement Inversion

You know DIP—the Dependency Inversion Principle.

Robert C. Martin said: "High-level modules should not depend on low-level modules. Both should depend on abstractions."

**IWD applies the same principle to requirements.**

| Code Level (DIP) | Requirement Level (IWD) |
|------------------|-------------------------|
| High-level module | User's intention (Verb) |
| Low-level module | Specific solution (Noun) |
| Interface | "I wanna DO..." |
| Implementation | DB, File, Cloud, etc. |

- **The verb is the interface.** What the user wants to accomplish.
- **The noun is the implementation.** One of many ways to serve that verb.

When the noun dictates the verb, you're locked in before you understand the problem.

When the verb dictates the noun, you're free to choose the best solution—and replace it later.

This is **Requirement Inversion**.

---

## The Freedom of Verbs

If we had asked the verb first:

> "What do you want to DO with this data?"
> 
> "Store it. Look it up sometimes. It doesn't change much."

Then options open up:

| Solution | Cost | Complexity | Fit |
|----------|------|------------|-----|
| JSON file | Free | Minimal | ✓ Perfect for static data |
| SQLite | Free | Low | Good for queries |
| Commercial RDBMS | $$$ | High | Overkill |

The verb—"store static data"—is the interface.

The noun—whatever storage we pick—is just an implementation.

**Implementations can be replaced. Interfaces remain.**

---

## The Noun Trap Is Everywhere

This isn't just about databases. Every trendy noun is a potential trap:

| Noun (Trap) | Verb (Freedom) |
|-------------|----------------|
| "We need microservices" | "We want to scale independently" |
| "We need Kubernetes" | "We want reliable deployments" |
| "We need AI" | "We want to predict behavior" |
| "We need blockchain" | "We want tamper-proof records" |

**The noun locks you in. The verb keeps options open.**

---

## The Noun Eraser Test

Here's a tool you can use today.

Take any requirement. Erase the technology name. What's left?

| Original | After Erasing | Verdict |
|----------|---------------|---------|
| "Implement Oracle" | "Implement ___" | ❌ Empty. Fake requirement. |
| "Build microservices" | "Build ___" | ❌ The noun WAS the requirement. |
| "Store data securely" | Still meaningful | ✓ Real requirement. |

**If erasing the noun leaves nothing, you've found a fake requirement.**

---

## Why This Matters: Money

"So what?" a manager might ask.

**Verbs save money.**

When you design around verbs, you can swap implementations without rewriting everything:

| Scenario | Noun-Driven | Verb-Driven |
|----------|-------------|-------------|
| Oracle → PostgreSQL | Massive migration | Swap implementation |
| On-premise → Cloud | Rewrite everything | Change adapter |
| Vendor A → Vendor B | Renegotiate, rebuild | Plug in new module |

**Verb-centered design is business agility.**

Every noun you hardcode is a future invoice you can't control.

---

## The Declaration

From now on:

1. **Hear a noun? Ask for the verb.** "Database" → "What do you want to DO?"

2. **Write requirements as verbs first.** Not "Login system needed." But "Users want to access their data securely."

3. **Treat nouns as hypotheses.** The noun is one possible answer. The verb is the real question.

4. **Remember:** Nouns create coupling. Verbs create freedom.

**Solutions should serve intentions. Not the other way around.**

---

Don't Go Too Fast. 🐢

---

## What's Next?
You've learned to flip nouns into verbs. But whose verb is it? The messenger? The manager? The real sufferer?
**Part 3: "Who is 'I'?"** shows how to find the true problem owner—because wrong "I" means wrong destination.

---

*Part 2 of the IWD Deep Dive Series.*

**Series:**
1. The Fear — Why direction matters
2. **Requirement Inversion** ← You are here
3. Who is "I"? 
4. WHI: The Dual Tool 
5. Even Laws Can Be Verbs 
6. State Tracking 

---

*Have you been trapped by a noun? Share your story.*
