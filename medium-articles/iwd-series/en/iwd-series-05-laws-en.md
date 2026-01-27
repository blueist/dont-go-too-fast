# Even Laws Can Be Verbs

*IWD Deep Dive Series — Part 5*

---

![image](https://i.ibb.co/DNb2TXT/w05.png)

## The Hardest Noun: "Regulation"

The biggest wall engineers face. Regulation.

> "Per security policy, all users must change their passwords every 3 months, and the system must force this."

A fortress of **nouns**. Many engineers stop thinking here.

> "If that's the regulation, we need to add features or find another solution."

From this moment, an expensive journey begins—not toward real security, but toward **'literal compliance.'**

---

## Case 1: You Already Have a Gun. Why Build a Castle Because You Don't Have a Sword?

When I joined the team, the system already had over 100 users. And a security audit regulation was holding them back.

- **The Contradiction:** The product already supported **2FA (Two-Factor Authentication)**—a powerful security measure—but it didn't have the 'force password change' feature written in the regulation.
- **The Team's Solution:** "We can't violate the regulation, so let's adopt an external authentication solution." (Over **$1,000** per year)
- **IWD's Intervention:** I went to the security audit team and threw a **WHI**:

> "If we don't change passwords every 3 months but instead make the already-deployed **2FA mandatory**, what security problems would occur?"

- **The Result:** The audit team immediately agreed: "2FA is much safer." **A 10-minute conversation saved $1,000.**

**The Real IWD:**

- **Surface:** "Force password change every 3 months" (noun)
- **Essence:** "I wanna DO **prevent unauthorized access**"

---

## Case 2: We Adopted CI/CD, So Why Did Incidents Increase?

Another team was captivated by the **noun** "modern development organization."

- **Noun-based Approach:** "Let's adopt CI/CD too!" → Set up GitHub Actions (Push → Build → Deploy automation)
- **The Result:** Deployments got faster, but bugs also reached production in 10 minutes. Incidents surged.
- **WHI's Diagnosis:** "What exactly did you automate? What about tests and reviews?"
- **The Answer:** "...Just build and deployment. Tests aren't ready yet..."

**The Real IWD:**

- **Surface:** "Adopt CI/CD" (noun)
- **Essence:** "I wanna DO **deploy reliably**"

The team obsessed over **C (Continuous)**—speed—and missed **I (Integration)** and **D (Deployment)**—quality.

> **"First, complete I and D. Then make it C."**
> 
> **"Incomplete CI/CD is worse than complete MI/MD (Manual Integration/Manual Deployment)."**

**The Result:** The team paused the pipeline and first completed their manual I&D process. Added tests, established review criteria, created deployment checklists. Then automated it. Incidents decreased.

---

## When You Can't Change the Regulation

Not all regulations can be changed through conversation.

| Type | Example | Changeable? |
|------|---------|-------------|
| Internal company policy | Code review, approval process | ✓ Through conversation |
| Industry standards | ISO certification, SOC 2 | △ Room for interpretation |
| Government laws | Privacy laws, financial regulations | ✗ Almost impossible |

But understanding the **verb** lets you comply at minimum cost.

For example, you can't change the "personal data encryption" law. But understanding its **real intent (verb: minimize breach damage)**:

- **Noun-based Approach:** Encrypt entire DB (performance hit, high cost)
- **Verb-based Approach:** Encrypt sensitive fields only + access logs + anomaly detection (efficient, safer)

---

## Regulations Are Someone's "I wanna DO"

> **Regulations are just someone's "I wanna DO" that hardened over time.**

| Surface (Noun) | Real Intent (Verb) | Alternative |
|----------------|-------------------|-------------|
| "Change password every 3 months" | Prevent unauthorized access | 2FA |
| "Adopt CI/CD" | Deploy reliably | Complete I&D first |
| "Mandatory data encryption" | Minimize breach damage | Encrypt sensitive fields only |

If you can change it, change it. If you can't, understand **why it exists**.

---

## The Declaration

1. **Regulations are not natural laws—they're 'fossilized intent.'** Verbs that someone once believed were best, hardened into nouns.

2. **It's not about 'features'—it's about 'purpose.'** When you focus on purpose (Verb) rather than building buttons (Noun), engineering becomes smarter.

3. **The most expensive engineering is 'unquestioning obedience.'** Before spending $1,000, have a 10-minute conversation.

4. **Find the "I" who created the regulation.** Understand their verb, and you won't be trapped by the letter.

**"Don't obey the letter (Noun). Obey the intent (Verb)."**

---

Don't Go Too Fast. 🐢

---

## What's Next?

From Part 1 to Part 5, we've learned the core tools of IWD. But how do we **manage all this across an entire project?**

**Part 6 "State Tracking"** covers the systematic approach to practicing IWD as a team.

---

*Part 5 of the IWD Deep Dive Series.*

**Series:**
1. The Fear — Why direction matters
2. Requirement Inversion — Verbs over nouns
3. Who is "I"? — Finding the real owner
4. WHI: The Dual Tool — Dismantling nouns
5. **Even Laws Can Be Verbs** ← You are here
6. State Tracking 

---

*Have you ever paid unnecessary costs because of a regulation? Or have you found the "I" behind a regulation and changed it?*
