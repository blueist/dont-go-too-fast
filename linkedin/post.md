
**LinkedIn Article 1: Introduction**

---

Friday, 4 PM.

Manager walks over. "Can you fix this bug before we deploy?"
Heart races. Hands on keyboard.
Then it happens.
Global variable. Hardcoded value. Skip the test.
"I'll fix it properly Monday."
Monday comes. You forget. The debt stays.

I've watched this happen for 25 years. 
To myself. To my teams. To developers I respect.

Not because they lack skill. 
They know Clean Code. 
They know SOLID. 
They've read the TDD book.

So why does all of it vanish at 4 PM on a Friday?

I found the answer in a place I didn't expect — cognitive psychology.

Daniel Kahneman won a Nobel Prize for showing how humans actually think. Two systems:
• System 1 — fast, automatic, runs fine under pressure
• System 2 — slow, careful, logical — shuts down under pressure

When the pressure hits, System 2 goes offline. System 1 takes the wheel.
"Global variable!" "Just hardcode it!" "Tests can wait!"
Not a decision. A reflex.

So I stopped asking "how do I get smarter?"
Started asking "how do I build habits that won't fall apart when things get hard?"
That question changed everything.

This is the first of 7 posts. 
No frameworks. 
No checklists. 
Just what I learned trying to answer that question over 25 years.

1. Why good developers make bad decisions under pressure — this one
2. Development practices that survive pressure
3. Replaceability: what good software actually needs
4. SNP: small steps matter
5. Dirty Pond: when fear owns your codebase
6. IWD: when perfect code solves the wrong problem
7. DGTF++: The Map

Link to the full article in comments.

Don't Go Too Fast. 🐢
Just slow enough for thinking to survive.

#softwaredevelopment #softwareengineering #programming #psychology #technicaldebt #dontgotoofast

---

**LinkedIn Article 2: ROD + TFD + DGTF**
---

Three things I wish I learned earlier.

Not principles. 
Not theory. 
Just three habits that kept me from wrecking my own code when deadlines hit.

---
First — sketch the service chain before you touch the keyboard.
I don't mean a perfect design. I mean trace the flow. 

What calls what. 
What depends on what. 
Where does data come from — and where it goes.

Sounds slow. It's not.
What's slow is discovering something's missing at 5 PM on Friday. That's when your brain panics. That's when shortcuts happen. That's when the global variable gets born.

I later gave it a name: ROD — Responsibility-Oriented Design.
Not perfect design. 
Just the chain of responsibility — visible before you write a single line of code.
Because you can delete what you don't need, but you can't calmly invent what you forgot at 5 PM.

---

Second — turn requirements into tests before you code.
Not after. Before.

I resisted this for years. Felt like extra work. It wasn't.
What's extra work is arguing about whether something is "done." What's extra work is fixing bugs that came from fuzzy requirements.

Requirements = Tests. 
Under pressure, I don’t trust my judgment. I trust the test.
It passes or it doesn't. That clarity saved me more times than I can count.

---

Third — notice when you're rushing. The chest tightness? That's the signal to pause.

This one's embarrassing. I spent years not even realizing I was doing it.
Chest tight. Typing fast. Skipping steps. Telling myself "I'll fix it later."
Now I try to catch it. Stop. Check the design. Check the tests. Then continue.
Takes five seconds. Doesn't always work. Works enough. 
I later realized this pause had a name too.
DGTF — Slow is Smooth, Smooth is Fast.

---

That's it. Design first. Tests as requirements. Catch yourself before you rush.
Nothing original. I just stopped assuming I could think straight under pressure.

---

Part 2 of 7.
1. Why good developers make bad decisions under pressure
2. Development practices that survive pressure — this one
3. Replaceability: what good software actually needs
4. SNP: small steps matter
5. Dirty Pond: when fear owns your codebase
6. IWD: when perfect code solves the wrong problem
7. DGTF++: The Map

Full article in comments.

Don't Go Too Fast. 🐢
Just slow enough for thinking to survive.

#softwaredevelopment #softwareengineering #programming #technicaldebt #dontgotoofast
---
**LinkedIn Article 3: Replaceability**
---

Copy. Paste. Change a few lines. Works. Go home.
I've done this a thousand times. Still do sometimes.

We all know it's bad. Low coupling. High cohesion. SOLID. I can explain these in an interview.
But when I looked at my actual code? I couldn't tell if I was doing it right.
"Is this low-coupled enough?" No idea.
"Does this follow SRP?" Honestly? I wasn't sure.
The words didn't help me. Too abstract.

What helped was a simpler question:
"If this changes, can I replace just this part?"
Not theory. Just yes or no.
"If we need to switch payment providers next month — is that a one-day job, or a two-week nightmare?"
That question finally made sense.

2007, maybe 2008. I was running an R&D team. We had one product, but every customer had their own version.
Feature request? Implement it separately for each version.
Bug fix? Fix it separately in each version.
Hell. No other word for it.
So we stopped. Made a rule. One codebase for everyone.
Whatever was different per customer — we pulled it out. Made holes in the system. Let different things plug in.
We called them "extension points." Didn't know any fancy terms back then. Just knew we couldn't keep living like that.
Took months. After that — one codebase, 100+ customers, ran for over 10 years.

Years later I learned SOLID. 
And realized: we had already built it — without knowing the name.
Extension point = Replaceable part.
All those principles — they're just different ways of asking the same thing.
"Can I replace just this part without breaking the world?"
If yes, good software.
If no, you'll pay for it later.

Now when I copy-paste, I ask myself one thing:
"Should this be a replacement point?"
Not always yes. But asking — that's the habit.

Part 3 of 7.
1. Why good developers make bad decisions under pressure
2. Development practices that survive pressure — this one
3. Replaceability: what good software actually needs
4. SNP: small steps matter
5. Dirty Pond: when fear owns your codebase
6. IWD: when perfect code solves the wrong problem
7. DGTF++: The Map

Full article in comments.

Don't Go Too Fast. 🐢
Just slow enough for thinking to survive.

#softwaredevelopment #softwareengineering #programming #cleancode #dontgotoofast

---
**LinkedIn Article 4: SNP**
---

For years, I only knew two states: not there yet, or done.

Below the goal? Failed.
Hit the goal? Finished. Move on.

Either way, I stopped.

Before I reached it, I felt like nothing counted. 
"80% coverage? Not enough. Doesn't matter until I hit 90%."

After I reached it, I felt like I was done. 
"90%? Good. Next thing."

Stupid way to think. 
Burned me out before the goal. 
Made me lazy after it.

---

I used to love the tortoise and hare story. Slow and steady wins.
But I missed the real lesson for years.
The hare didn't lose because he was fast. 
He lost because he decided he'd gone far enough.

Stopping. That's the failure.

---

So I tried to change how I see progress.

Target: 90% coverage.

At 70%?
Before: "Not done. Doesn't count."
Now: "Better than last week. What's one more thing I can cover?"

At 90%?
Before: "Done. Move on."
Now: "Good. But which parts are still fragile?"

Same numbers. Different questions.

---

I started calling this SNP. Spectrum, not pass/fail.

It's a road, not a ladder. 
On a ladder, you've either climbed it or you haven't. 
On a road, every step counts. 
You don't fail because you're not there yet, 
and you don't stop because you arrived.

You're somewhere on the road. 
The only question is: are you still moving?

---

I'm not good at this yet. I still catch myself in pass/fail mode.
But when I remember it's a road, I burn out less. Quit less. And actually get further.

Part 4 of 7.
1. Why good developers make bad decisions under pressure
2. Development practices that survive pressure — this one
3. Replaceability: what good software actually needs
4. SNP: small steps matter
5. Dirty Pond: when fear owns your codebase
6. IWD: when perfect code solves the wrong problem
7. DGTF++: The Map

Full article in comments.

Don't Go Too Fast. 🐢
Just slow enough to keep moving.

#softwaredevelopment #softwareengineering #programming #productivity #dontgotoofast

---
**LinkedIn Article 5: Dirty Pond**
---

Do you have that one file no one touches?
Do you want to fix it?

I have one too.
But I'm not sure I really want to fix it.
Because fixing it requires something uncomfortable first: touching dangerous code.

---

"Don't change that one."
"It might break something."
"It's been like that forever."

So I started asking a different question.
Not "how do we clean this?"
But: "Do we really want to clean this?"

---

Because here's the truth.

If you really want it, you accept the cost. Things might break. Progress will be slow. You'll feel shaky for a while.

If you don't really want it, you work around it. Add flags. Add comments. Promise a rewrite "later."

That choice — repeated enough times — creates what I call a Dirty Pond.

---

A Dirty Pond isn't dirty because people are lazy.
It's dirty because people are afraid.

Fear changes behavior. No small refactors. No naming fixes. No tests. Just avoidance.

And avoidance doesn't freeze the problem. It feeds it.

---

The loop is simple.

Fear leads to avoidance.
Avoidance means no small improvements.
No small improvements quietly turn into bigger problems.
And bigger problems bring more fear.

Not a technical loop. An emotional one.

---

Most teams try to skip the hard question.
They jump straight to solutions. "Let's refactor later." "Let's rewrite it." "Let's document it better."

But if the honest answer to "Do we really want to fix this?" is "not enough to risk breaking it" — none of those work.

---

When the answer is yes, the solution is boring.

Not a rewrite. Not a cleanup sprint.
Just making it slightly safer to touch.

One test. One boundary. One responsibility pulled out. One name that finally makes sense.

Tiny steps. Each one lowers fear. Lower fear invites the next step.

That's how ponds clear.

---

I stopped thinking dirty code was the real problem.
Fear usually was.

And the first fix, for me, wasn't technical.
It was being honest about that.

---

Part 5 of 7.

1. Why good developers make bad decisions under pressure
2. Development practices that survive pressure
3. Replaceability: what good software actually needs
4. SNP: small steps matter
5. Dirty Pond: when fear owns your codebase — this one
6. IWD: when perfect code solves the wrong problem
7. DGTF++: The Map

Full article in comments.

Don't Go Too Fast. 🐢
Slow enough to stay brave.

#softwaredevelopment #softwareengineering #programming #technicaldebt #dontgotoofast

---
**LinkedIn Article 6: IWD**
---

Built it perfectly once.
Clean architecture. Full coverage. Documented.
We shipped it. Everything was working perfectly.  

Three months later: *"That's not what I meant."*

The code was fine. The solution was elegant. I just solved the wrong problem.

---

*"The dashboard isn’t real-time. Please make the data update without refreshing."*

So we built it. WebSocket. Real-time push. Numbers updating live.

Complaints kept coming.

A field worker explained it clearly:
*"I don’t care if it updates instantly. I care that it’s true. It says 'Approved' here, but 'Pending' there. Which one is correct?"*

We heard *“not real-time.”*
He meant *“I can’t trust this data.”*

Months of work. Wrong direction.

---

I noticed a pattern.

When people say *“We need encryption”* — that’s not a requirement. It’s a solution.

When they say *“I don’t want to get penalized”* — that’s the real problem.

**Nouns hide solutions. Verbs reveal problems.**

---

Now I ask one question before building anything:

*"If there were another way to solve this, would you still need [that thing]?"*

Example:

"We need encryption."
"What happens if there's no encryption?"
"We will get penalized."
"If there were another way to avoid penalties, would you still need encryption?"
"...No."

That “no” opens everything up. Encryption was never the requirement. Avoiding penalties was.

---

I have a simple self-test now.

Can I express the requirement out loud as if it’s my own problem?

If I'm stuttering — "um, the regulation says, I think we need…" — I don't get it yet.
If I can say it like I mean it — "If this data leaks, we're done" — I'm close.

I still catch myself building too early. But that one question — *“Would you still need it if there were another way?”* — has saved me more than once.

---

Part 6 of 7.

1. Why good developers make bad decisions under pressure
2. Development practices that survive pressure — this one
3. Replaceability: what good software actually needs
4. SNP: small steps matter
5. Dirty Pond: when fear owns your codebase
6. IWD: when perfect code solves the wrong problem
7. DGTF++: The Map

Full article in comments.

Don’t Go Too Fast. 🐢
Slow enough to aim before building.

#softwaredevelopment #softwareengineering #programming #requirements #dontgotoofast

---

**LinkedIn Article 7: DGTF++: The Map**

---

I kept asking myself —
why do I make bad decisions even when I know better?

It wasn’t skill.
I had experience.
I’d read the books.
I knew the principles.

It was pressure. That’s it.

When pressure hits, something changes.

The careful part of my brain goes quiet.
Reflexes take over.

I implement before I understand.
Design before I ask.
Commit with confidence.
Regret later.

Writing this series, one thing became clear.

More knowledge wasn’t going to fix this.
I needed habits that don’t collapse under stress.

So I didn’t build a methodology.

I drew a map.

Markers I can look back at when things get chaotic:


The path:
- Is this the right problem? (IWD)
- Is responsibility clear? (ROD)
- Did I test first? (TFD)

The pace:
- Am I rushing? (DGTF)
- Am I biting off too much? (SNP)

That’s it.
Five markers. Nothing fancy.

Reading this won’t fix anything.

It didn’t fix me either.

I still rush.
Still build before thinking.
Still make the same mistakes.

But sometimes — not always, but sometimes —
I catch myself before the commit.

Five seconds. That’s all.

I think that’s enough.

Slow is fine.
Still on the road.

Don’t Go Too Fast. 🐢

(Full series on Medium — link in comments)

#softwaredevelopment #softwareengineering #programming #productivity #dontgotoofast