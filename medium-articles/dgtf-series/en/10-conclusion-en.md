# DGTF++: The Map

![image](https://i.ibb.co/bMwqNV9d/10.png)
---

*"This is the final article in the series "A Software Development Philosophy for Maintaining Quality Under Pressure."*

---

Nine articles later, I want to explain what I was actually trying to build. Not a methodology. Not a framework. A map.

When I started writing, I had one question: Why do developers who know better still make bad decisions? The answer wasn't skill. It wasn't knowledge. It was pressure.

Pressure changes how the brain works. System 2 shuts down. System 1 takes over. Everything you know disappears exactly when you need it most. That was the Introduction—the "why" behind everything else.

But "why" isn't enough. You need "what" and "how."

So I built this.

---

## The Verticals and Horizontals

### Three Verticals — What to do

These are the pillars of your work, depending on the stage of the code.

* **IWD (Inception)**: Before you build, ensure you are solving the right problem. Nouns hide solutions; verbs reveal problems. Ask "What do you want to DO?" instead of "What do you want?"
* **ROD + TFD + Replaceability (New Code)**: Design the full chain first. Turn requirements into tests. Make everything replaceable. This is the path for greenfield projects.
* **FBS (Dirty Pond / Legacy)**: When you inherit broken code that nobody wants to touch—the eternal Friday 4 PM. You clean it one bucket of water at a time.

### Two Horizontals — How to do it

The horizontals are the behaviors that cut through every stage.

* **DGTF (The Pause)**: Don't Go Too Fast. Pause before you act. Check your design. Check your tests. Five seconds can save five weeks.
* **SNP (The Stride)**: Spectrum, Not Pass/fail. You are not climbing a ladder; you are walking a road. Whether you are below or above the target, keep moving.

---

## The Intersection

The horizontals are how you do everything else.

* **When doing IWD**: Pause (DGTF) before assuming you understand. Use small steps (SNP) in excavating requirements.
* **When doing ROD**: Pause (DGTF) before adding complexity. Use small steps (SNP) in building the chain.
* **When fixing a Dirty Pond**: Pause (DGTF) before touching that file. Use small steps (SNP) in cleaning the mess.

---

## From Knowledge to Habit

Reading these articles is not learning. I wrote about habits, not knowledge. Knowledge you get from reading. Habits you only get from doing. If you read all nine articles and go back to coding the same way tomorrow, nothing changes.

The real work starts after reading:

1. **Week 1**: Notice when you are rushing. Just notice. Don't even try to stop yet.
2. **Week 2**: When you notice the rush, pause for five seconds. That is it.
3. **Week 3**: In those five seconds, ask: "Am I solving the right problem?"
4. **Month 2**: Start writing one test before coding. Just one.
5. **Month 3**: Try designing the service chain before implementing. Even a rough sketch.

---

## Conclusion

This is slow. Painfully slow. That is the point.

Knowledge betrays you under pressure. Habit saves you. You cannot build habits fast.

It took me twenty-five years to find these patterns. To fail enough times to finally write them down. I don't expect anyone to get there in six months. But if one person reads this and pauses before their next commit, the map worked.

Don't Go Too Fast. 🐢
We are on the road.

---

*"This is the final article in the series "A Software Development Philosophy for Maintaining Quality Under Pressure.""*

---

**Series:**

1. Why Good Developers Make Bad Decisions Under Pressure
2. ROD: Design Complete, Implement Fast
3. TFD: Requirements = Tests
4. DGTF: Slow is Smooth, Smooth is Fast
5. Putting It All Together
6. Replaceability: The Ultimate Objective for Good Software
7. SNP: Your Little Step Also Worthy
8. Can I Just Escape from a Dirty Pond? (FBS)
9. IWD: When Perfect Code Solves the Wrong Problem
10. **DGTF++: The Map** (You are here)

*Connect with me:*

* GitHub: dont-go-too-fast
* Email: stillblueist@naver.com


