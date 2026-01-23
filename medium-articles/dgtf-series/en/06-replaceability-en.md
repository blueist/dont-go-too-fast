# Replaceability: The Ultimate Objective for Good Software

---

![image](https://i.ibb.co/9kmNkSYg/06.png)

A developer copies a file.
Pastes.
Modifies a little.
It works.
Goes home.

Next day, copies again.
Pastes.
It works.

What's wrong with this?

---

We've heard it. Low Coupling is good. Use Design Patterns. Follow SOLID principles.

But let's be honest.

"I heard it's good. But I don't know what this makes good to my system."

So we copy and paste. Because it works now. Because it's fast. Because we don't want to touch what's already working.

This is the reality for most developers. And they're not wrong.

---

## The Real Problem

Low Coupling. I heard it's good.

"Low coupling makes maintenance easier."

Sounds right. But the order API I'm building right now—is it Low Coupling or not? I don't know. And if it is Low Coupling, what exactly gets easier next month when I maintain it? I can't imagine.

I heard it's good. But it doesn't connect to my current code.

---

Design Pattern. I heard it's good.

"Factory Pattern encapsulates object creation."

Got it. But if I put a Factory in my service now, what gets better? When the next requirement comes in, what changes? I can't picture it.

I heard it's good. But it doesn't connect to my current system.

---

SOLID. I heard it's good. Even said it in interviews.

"Single Responsibility, Open-Closed, Liskov..."

Memorized them all. But does this class follow SRP or not? I don't know. If it does, what gets better later? I can't imagine.

I heard it's good. But it doesn't connect to my current problem.

---

The problem with principles is this:

**I heard they're good. I believe it. But they don't connect to my current problem. I can't imagine what gets better in the future.**

So we copy and paste. Because it works now. Because it's fast.

---

## One Question

Here's a better question.

**"If requirements change, can I replace just this part?"**

This question is different.

- It's not abstract
- You can answer Yes or No
- You know what it's asking, what it intends, how to verify it

"If we switch from MySQL to PostgreSQL, we don't have to change all the data models, right?"
"If we change the payment gateway, we just replace the payment module, right?"
"If we switch from Twilio to another service, where exactly do we need to modify?"

---

And this question automatically creates the next questions.

"In what situation would we need to replace this?"
"How can we replace it?"
"What changes when we replace it?"

**Replaceability makes developers naturally imagine solutions to their next problems.**

Low Coupling doesn't stimulate this imagination. It just describes a state.

---

## Same Time, Different Results

Let's look at an example.

A new request comes in. There's a regular order API, and they want a VIP order API. Almost the same. Just different discount rates and logging.

---

**Without Replaceable point:**

Developer copies OrderService.java.
Creates VIPOrderService.java.
Modifies discount rate.
Modifies logging.

Copies OrderRepository.java.
Creates VIPOrderRepository.java.

Copies OrderValidator.java.
Creates VIPOrderValidator.java.

It works. Done.

- Time: 2 days
- Files: 6 added
- Duplication: 90%

---

**With Replaceable point:**

OrderService already has replacement points.
- DiscountPolicy (interface)
- OrderLogger (interface)

Developer creates VIPDiscountPolicy.java.
Creates VIPOrderLogger.java.
Combines them in configuration.

It works. Done.

- Time: 2 days
- Files: 2 added
- Duplication: 0%

---

**Both copied and pasted. Both took 2 days.**

The difference is quality.

Then the next request comes. "Add an enterprise order API too."

First developer: Copy 6 more. Now 18 files.
Second developer: Add 2 more. Now 6 files.

**With Replaceable points, quality accumulates even when you copy and paste.**

Then another request comes. "Add IP logging to all order logs."

First developer: Modify 3 files.
Second developer: Modify 1 file.

It's easy to imagine what happens next to these two developers.

---

## 100 Customers, One Solution

It was 2007. Maybe 2008.

I was working as an R&D manager at a software solution company. Our product was used by multiple customers. The problem was, each customer had a different version.

Version for Customer A.
Version for Customer B.
Version for Customer C.
...

Add a new feature? Had to apply it separately to every version.
Fix a bug? Had to fix it separately in every version.

It was hell.

---

I created a SWAT team. The mission was one.

**"Create one solution for all customers. Without affecting existing customers."**

We started by analyzing dependencies.
Defined clear responsibilities for each part,
organized the relationships between responsibilities.
Then created holes where needed for replaceable collaboration.
We called them "Extension Points."

Different parts per customer? Separated into Extension Points.
Same parts across customers? Kept in the core.

Thanks to Extension Points, we could maintain the working versions.

---

Result:

We got one solution. For over 100 customers. It was maintained for more than 10 years after that.

And at that time, I didn't even know what SOLID was.

---

When I learned SOLID later, I realized.

"Extension Point" = Replaceable point.

What we discovered was the same thing the theorists talked about. We just found it in practice first.

---

## The Ultimate Objective

Low Coupling, High Cohesion, SOLID, Design Pattern...

What are these for?

**Replaceability.**

They're all for making things replaceable.

- Low Coupling → Less connections make replacement easier
- High Cohesion → When related things are together, replacement units are clear
- SOLID
  - SRP/ISP → Defines the characteristics of what should be replaced
  - OCP/DIP → Provides methods to implement replacement
  - LSP → Explains the theoretical background for why replacement works
- Design Pattern → Proven methods for creating replacement points
- ROD → Designs replacement units
- TFD → Verifies that replacement is safe

**Replaceability is not a principle. It's the goal of principles.**

---

If your company uses OKR, this is easy to understand.

- Objective: Good software
- Key Result: Can it be replaced?

Low Coupling is not a Key Result. It's a method to achieve the Key Result.

**Replaceability is the Key Result.**

---

## Starting Tomorrow

Copy and paste is fine. It's fast, intuitive, and realistic.

Just think about one thing before you copy and paste.

**"Should there be a replacement point here?"**

This one question:
- Creates better quality in the same time
- Makes you imagine solutions to the next problem
- Lets you respond to change while maintaining "don't touch what's working"

---

What is good software?

**Software that can be replaced.**

---

*This article is part of the series "A Software Development Philosophy for Maintaining Quality Under Pressure."*

---

**Series:**
1. [Why Good Developers Make Bad Decisions Under Pressure]()
2. [ROD: Design Complete, Implement Fast]()
3. [TFD: Requirements = Tests]()
4. [DGTF: Slow is Smooth, Smooth is Fast]()
5. [Putting It All Together]()
6. **Replaceability: The Ultimate Objective for Good Software** ← You are here

---

*Connect with me:*
- *GitHub: [dont-go-too-fast](https://github.com/bakmeon/dont-go-too-fast)*
- *Email: stillblueist@naver.com*
