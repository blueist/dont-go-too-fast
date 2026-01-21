# ROD: Design Complete, Implement Fast

## Why "More is Better Than Missing" Will Save Your Projects

---

![image](https://i.ibb.co/C5K0Tv1h/02.png)

*This is Part 2 of a series on maintaining software quality under pressure. [Read Part 1: Why Good Developers Make Bad Decisions Under Pressure]()*

---

**Picture this scenario:**

You're implementing a login feature. You've designed two services:
1. `ValidateCredentials(username, password)`
2. `CreateSession(user)`

You start coding. Everything flows smoothly until you reach `CreateSession`.

*Wait... where does `user` come from?*

Your design doesn't say. There's no service for finding the user. There's no service for verifying the password.

**You've discovered a "Missing."**

And it's 4 PM on Friday.

Your heart rate spikes. Your System 1 kicks in. And you do what every stressed developer does:

```go
// Quick fix - I'll clean this up later
var globalUserCache = make(map[string]*User)
```

This is how technical debt is born. Not from laziness or ignorance—from **incomplete design meeting deadline pressure**.

---

## The Core Principle: More is Better Than Missing

ROD (Responsibility-Oriented Design) is built on one simple idea:

> **It's easier to remove unnecessary things than to add missing things.**

Why? Because of how our brains work under pressure.

**Removing during implementation:**
- System 2: "This service isn't needed. Delete it."
- Action: Press delete key
- Risk: Low (the code never existed)

**Adding during implementation:**
- System 1: "I need something that's not in the design!"
- Panic: "How do I make this?"
- Reaction: Global variable / Singleton / Hardcode
- Risk: High (rushed decisions create debt)

When you design "more than needed," you give yourself safe things to remove. When you design "just enough," any gap triggers panic.

---

## What is a Service Chain?

A service chain is a complete map of every step needed to accomplish a task.

Let's redesign that login feature properly:

```
Login Service Chain (Complete)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

1. ValidateCredentialsFormat(username, password)
   → Validates input format
   → Returns: error or nil

2. FindUserByUsername(username)
   → Looks up user in database
   → Returns: *User or ErrUserNotFound

3. VerifyPassword(inputPassword, user.HashedPassword)
   → Compares passwords securely
   → Returns: bool, error

4. CheckAccountStatus(user)
   → Verifies account isn't locked/disabled
   → Returns: error or nil

5. CreateSession(user)
   → Creates new session object
   → Returns: *Session

6. StoreSession(session)
   → Persists session to storage
   → Returns: error or nil

7. GenerateSessionToken(session)
   → Creates JWT or session token
   → Returns: string, error

8. LogLoginEvent(user, timestamp)
   → Records login for audit
   → Returns: error or nil
```

**Notice what happened:**

The original 2-service design had 6 missing pieces. That's 6 opportunities for System 1 to make bad decisions during implementation.

The complete 8-service chain has zero gaps. Implementation becomes *following*, not *inventing*.

---

## The No-Constructor, No-Static Rule

Here's a rule that seems strange at first:

> **Never use constructors or static fields in your service design.**

Why? Because they're System 1 escape hatches.

**With constructors allowed:**
```go
// System 1: "I need a user!"
user := new(User)  // No service needed!
user.Name = "admin"  // No validation!
// Problem hidden. Debt created.
```

**Without constructors:**
```go
// System 2: "I need a user... how do I get one?"
// → Check design
// → No service for this!
// → MISSING DISCOVERED
// → Fix design now, not in production later
```

The constraint forces you to think: *"Where does this object come from?"*

If the answer isn't in your service chain, you've found a Missing. Better to find it during design than during a Friday afternoon crisis.

---

## Real Example: Payment System

Let's see ROD applied to something more complex.

**Bad design (6 services with hidden gaps):**
```
1. ValidatePayment
2. ProcessPayment
3. SendConfirmation
4. UpdateInventory
5. CreateOrder
6. LogTransaction
```

Looks reasonable. But during implementation:
- Where does payment method validation happen?
- Who encrypts the card data?
- What about the payment gateway response?
- How do we handle partial failures?

**Complete ROD design (18 services, zero gaps):**

```
Payment System Service Chain
━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Input Validation Subsystem:
1. ValidatePaymentRequest
2. FindUserAccount  
3. CheckUserPaymentEligibility

Calculation Subsystem:
4. CalculateTotalAmount
5. CheckInventoryAvailability

Reservation Subsystem:
6. ReserveInventory

Payment Processing Subsystem:
7. ValidatePaymentMethod
8. CreatePaymentTransaction
9. EncryptPaymentData
10. CallPaymentGateway
11. DecryptPaymentResponse
12. ValidatePaymentResponse

State Management Subsystem:
13. UpdateTransactionStatus
14. ConfirmInventoryReservation

Output Subsystem:
15. CreateOrderRecord
16. SendPaymentConfirmation
17. UpdateUserPaymentHistory

Monitoring Subsystem:
18. LogPaymentEvent
```

**Yes, it's longer. That's the point.**

Every service is a clear, single responsibility. Every gap is filled. When you implement service #10 (`CallPaymentGateway`), you know exactly what comes before it (#9: encrypted data) and after it (#11: decrypt response).

No surprises. No panic. No global variables.

---

## Connecting to SOLID

You might be thinking: "This sounds like Clean Architecture."

You're right. ROD isn't revolutionary new technology. It's the answer to **why**, **when**, and **how** to apply principles you already know.

| SOLID Principle | ROD Application |
|----------------|-----------------|
| **S**ingle Responsibility | Each service = one job |
| **O**pen-Closed | Add services, don't modify existing |
| **L**iskov Substitution | Services are replaceable via interfaces |
| **I**nterface Segregation | Minimal interfaces per service |
| **D**ependency Inversion | Depend on interfaces, not implementations |

The difference is timing:

```
Traditional approach:
Implement → Notice problems → Apply SOLID → Refactor

ROD approach:
Design with SOLID → Implement → No refactoring needed
```

ROD puts the thinking where it belongs: **in the design phase**, when System 2 is active and you have time to think properly.

---

## What About Over-Engineering?

"But won't this create over-engineered systems?"

No. And here's why:

**Over-engineering** means *implementing* unnecessary features.

**ROD** means *defining* complete services—then only implementing what you need.

```
ROD Design Phase (1 hour):
- Define 20 services
- Understand the complete system
- See all relationships

Implementation Phase:
- Implement 15 services
- Remove 5 as "not needed yet"
- Informed decision, not rushed guess
```

Removing a service you defined is trivial. Adding a service you missed is where disasters happen.

---

## The ROD Checklist

Before you start implementing, verify:

**Completeness:**
- [ ] Can I accomplish the requirement using only these services?
- [ ] Every object has a service that creates it?
- [ ] Every validation has a dedicated service?
- [ ] Every external call has a service?

**No Shortcuts:**
- [ ] Not using constructors directly?
- [ ] Not using static fields?
- [ ] No assumptions about "obvious" steps?

**SOLID Applied:**
- [ ] Each service has one responsibility?
- [ ] Services communicate through interfaces?
- [ ] Dependencies are injectable?

If any box is unchecked, you have a Missing waiting to ambush you.

---

## Try This Now

Take a feature you're currently working on. Ask yourself:

1. **List every service** needed to complete it
2. **For each object**, ask: "Where does this come from?"
3. **For each step**, ask: "What could go wrong here?"
4. **Add services** for anything missing

Don't worry if the list seems long. Length isn't the enemy—gaps are.

---

## Coming Next: TFD

ROD answers "what should we build?"

But how do we know when we're done? How do we catch mistakes early? How do we get feedback fast enough to prevent System 1 from taking over?

**In the next article**, I'll introduce TFD (Test-First Development)—how treating requirements as tests creates the feedback loop that makes ROD work.

You'll learn:
- Why "Requirements = Tests" changes everything
- How TFD makes TDD easier, not harder
- Building feedback loops that catch System 1 mistakes
- Real test scenarios from production systems

---

*ROD isn't about writing more code. It's about knowing exactly what code to write—before pressure makes the decision for you.*

**Next: TFD—Requirements = Tests →**

---

*What's the worst "Missing" you've discovered mid-implementation? Share in the comments.*
