# TFD: Requirements = Tests

## Why Test-First Development Makes TDD Actually Work

---

*This is Part 3 of a series on maintaining software quality under pressure. [Read Part 2: ROD—Design Complete, Implement Fast]()*

---

**Have you tried TDD?**

Most developers have. And most developers have given up.

```
Attempt 1:
"Write tests first, they said..."
→ "What do I even test?"
→ Overwhelmed
→ "Let me just code first, I'll add tests later"
→ Give up

Attempt 2:
"Red → Green → Refactor!"
→ Write first test
→ "Is this even right?"
→ Test structure changes while implementing
→ "This is too hard"
→ Give up

Attempt 3:
"Start simple..."
→ Test utility functions
→ "This is easy!"
→ Complex business logic appears
→ "How do I test this?"
→ Give up
```

If this sounds familiar, the problem isn't you. The problem is TDD's hidden assumption:

**TDD assumes you know what to test.**

But if you don't have a clear design, you don't. You're trying to write tests for code that doesn't exist yet, in a structure you haven't figured out.

No wonder it's hard.

---

## TFD: Test-First Development

TFD doesn't replace TDD. **TFD makes TDD possible.**

Here's the difference:

```
TDD:
Test → Implement → Refactor
"Tests drive the design"

TFD:
Design (ROD) → Test → Implement
"Design defines the tests"
```

When you have a complete service chain from ROD, you know exactly what each service does. Its inputs. Its outputs. Its error cases.

Writing tests becomes *filling in the blanks*, not *inventing from nothing*.

---

## The Core Principle: Requirements = Tests

Here's the insight that changes everything:

> **A test is a requirement made executable.**

Think about traditional requirements:

```
Traditional requirement:
"Users should be able to make payments"

Questions immediately arise:
- What if the card fails?
- What if insufficient balance?
- Duplicate payment prevention?
- Partial refunds?
- Timeouts?
```

The requirement is ambiguous. Different developers will interpret it differently. Edge cases will be discovered in production.

Now look at TFD requirements:

```go
func TestPaymentService(t *testing.T) {
    // The NORMAL case
    t.Run("ValidPayment_ShouldSucceed", func(t *testing.T) {
        // Valid card, sufficient balance → Success, return receipt
    })
    
    // CARD ERRORS
    t.Run("InvalidCard_ShouldReturnCardError", func(t *testing.T) {
        // Invalid card number → CardError, no charge
    })
    
    t.Run("ExpiredCard_ShouldReturnExpiredError", func(t *testing.T) {
        // Expired card → ExpiredCardError
    })
    
    // BALANCE ERRORS  
    t.Run("InsufficientFunds_ShouldReturnFundsError", func(t *testing.T) {
        // Insufficient balance → InsufficientFundsError, no charge
    })
    
    // DUPLICATE PREVENTION
    t.Run("DuplicatePayment_ShouldReturnDuplicateError", func(t *testing.T) {
        // Same order paid twice → DuplicatePaymentError
    })
    
    // REFUNDS
    t.Run("PartialRefund_ShouldRefundPartialAmount", func(t *testing.T) {
        // Refund $30 of $100 → $30 refunded, $70 remaining
    })
    
    // CONCURRENCY
    t.Run("ConcurrentPayments_ShouldHandleCorrectly", func(t *testing.T) {
        // Simultaneous payments for same order → Only one succeeds
    })
}
```

**Every ambiguity is resolved. Every edge case is documented. Every requirement is executable.**

PM asks: "Does it handle duplicate payments?"
You answer: "Run the test. If it passes, yes."

---

## How TFD Works with ROD

Remember our login service chain from the last article?

```
1. ValidateCredentialsFormat
2. FindUserByUsername
3. VerifyPassword
4. CheckAccountStatus
5. CreateSession
6. StoreSession
7. GenerateSessionToken
8. LogLoginEvent
```

Each service becomes a test suite:

```go
// Service 2: FindUserByUsername
func TestFindUserByUsername(t *testing.T) {
    // NORMAL
    t.Run("ExistingUser_ShouldReturnUser", func(t *testing.T) {
        // Username exists → Return user object
    })
    
    // ERROR
    t.Run("NonExistingUser_ShouldReturnNotFound", func(t *testing.T) {
        // Username doesn't exist → ErrUserNotFound
    })
    
    t.Run("EmptyUsername_ShouldReturnInvalidInput", func(t *testing.T) {
        // Empty string → ErrInvalidInput
    })
    
    // EDGE
    t.Run("DeletedUser_ShouldReturnNotFound", func(t *testing.T) {
        // User was soft-deleted → ErrUserNotFound
    })
    
    t.Run("CaseInsensitive_ShouldFindUser", func(t *testing.T) {
        // "JOHN" should find "john" → Return user
    })
}

// Service 3: VerifyPassword
func TestVerifyPassword(t *testing.T) {
    // NORMAL
    t.Run("CorrectPassword_ShouldReturnTrue", func(t *testing.T) {
        // Correct password → true, nil
    })
    
    // ERROR  
    t.Run("WrongPassword_ShouldReturnFalse", func(t *testing.T) {
        // Wrong password → false, nil (not an error!)
    })
    
    t.Run("EmptyPassword_ShouldReturnFalse", func(t *testing.T) {
        // Empty password → false, nil
    })
    
    // EDGE
    t.Run("UnicodePassword_ShouldWork", func(t *testing.T) {
        // Password with unicode → handles correctly
    })
}
```

**You're not inventing tests. You're translating your service chain into executable specifications.**

---

## The Three Categories of Tests

For each service, think about three categories:

### 1. Normal Cases (Happy Path)
What happens when everything goes right?

```go
t.Run("ValidInput_ShouldSucceed", ...)
```

### 2. Error Cases
What can go wrong? What should happen?

```go
t.Run("InvalidInput_ShouldReturnSpecificError", ...)
t.Run("ServiceUnavailable_ShouldReturnError", ...)
t.Run("Timeout_ShouldReturnTimeoutError", ...)
```

### 3. Edge Cases
What about boundaries and weird inputs?

```go
t.Run("EmptyString_ShouldHandle", ...)
t.Run("VeryLongInput_ShouldHandle", ...)
t.Run("SpecialCharacters_ShouldHandle", ...)
t.Run("ConcurrentCalls_ShouldHandle", ...)
```

Miss any category and you have blind spots. System 1 will find them during implementation and panic.

---

## The Feedback Loop

Here's why TFD matters for pressure situations:

```
┌─────────────────────────────────────┐
│                                     │
│   Write Test                        │
│       ↓                             │
│   Run Test (RED)                    │
│       ↓                             │
│   Implement Service                 │
│       ↓                             │
│   Run Test (GREEN)                  │
│       ↓                             │
│   Next Test ←─────────────────┘     │
│                                     │
└─────────────────────────────────────┘
```

This loop provides **immediate feedback**.

- Code wrong? Test fails now, not in production
- Forgot a case? See it in test results, not customer complaints
- System 1 made a mistake? Test catches it before commit

**Fast feedback is System 1's antidote.**

When you know within seconds whether your code is correct, there's no time for panic to build. No gap for bad decisions to fill.

---

## TFD vs TDD: The Relationship

Let me be clear: **TFD doesn't compete with TDD.**

```
If TDD works for you:
→ Great! Keep doing it.
→ TFD just adds a design phase before

If TDD has been hard:
→ That's because you lacked clear design
→ Try ROD first
→ With service chain in hand, TDD becomes natural
```

The cycle becomes:

```
ROD: Design service chain
     ↓
TFD: Write test structure for each service
     ↓
TDD: Red → Green → Refactor for each test
     ↓
Done: Complete, tested, clean
```

**TFD provides the clarity that TDD assumes you already have.**

---

## Real Example: Search System

Let's see TFD applied to a search feature.

**ROD Service Chain:**
```
1. ValidateSearchQuery
2. ParseSearchFilters
3. ExecuteSearch
4. RankResults
5. ApplyPagination
6. EnrichWithMetadata
7. FormatResponse
```

**TFD Tests for Service 3 (ExecuteSearch):**

```go
func TestExecuteSearch(t *testing.T) {
    // NORMAL
    t.Run("ValidQuery_ShouldReturnResults", func(t *testing.T) {
        // Search "laptop" → Return laptop products
    })
    
    t.Run("QueryWithFilters_ShouldApplyFilters", func(t *testing.T) {
        // Search "laptop" + price:100-500 → Filtered results
    })
    
    // ERROR
    t.Run("EmptyQuery_ShouldReturnError", func(t *testing.T) {
        // Empty string → ErrEmptyQuery
    })
    
    t.Run("DatabaseUnavailable_ShouldReturnError", func(t *testing.T) {
        // DB down → ErrServiceUnavailable
    })
    
    // EDGE
    t.Run("NoResults_ShouldReturnEmptyList", func(t *testing.T) {
        // Search "xyzabc123" → Empty list (not error!)
    })
    
    t.Run("SpecialCharacters_ShouldEscape", func(t *testing.T) {
        // Search "laptop; DROP TABLE" → Escaped, safe
    })
    
    t.Run("VeryLongQuery_ShouldTruncate", func(t *testing.T) {
        // 10000 char query → Truncated to limit
    })
    
    t.Run("UnicodeQuery_ShouldWork", func(t *testing.T) {
        // Search "노트북" → Return Korean results
    })
}
```

**Every test is a requirement. Every requirement is testable. No ambiguity.**

---

## The Single Source of Truth

Once you adopt TFD, something beautiful happens:

```
Traditional project:
├── Requirements doc (outdated)
├── Design doc (maybe updated)  
├── Code (actual behavior)
├── Comments (lies)
└── Tests (if they exist)

→ 5 sources of "truth", all different

TFD project:
├── Tests = Requirements = Design = Documentation
└── Code (implements the tests)

→ 1 source of truth, always current
```

Tests become your documentation. If someone asks "what does this service do?", point them to the tests. If the tests pass, the code is correct. If the tests are wrong, fix the tests.

**Tests are the contract.** Everything else is commentary.

---

## The TFD Checklist

Before implementing a service:

**Coverage:**
- [ ] Happy path test exists?
- [ ] All error cases covered?
- [ ] Edge cases identified?
- [ ] Integration points tested?

**Clarity:**
- [ ] Test names describe the requirement?
- [ ] Expected behavior is obvious?
- [ ] Error messages are specific?

**Independence:**
- [ ] Tests don't depend on each other?
- [ ] Tests don't depend on external state?
- [ ] Tests can run in any order?

**Feedback:**
- [ ] Tests run fast (< 1 second each)?
- [ ] Failures point to the problem?
- [ ] Coverage is measurable (> 80%)?

---

## Coming Next: DGTF

ROD tells you what to build.
TFD tells you how to verify it.

But what happens when the deadline hits anyway? When your manager is standing over your shoulder? When System 1 is screaming "HURRY!"?

**In the next article**, I'll introduce DGTF (Don't Go Too Fast)—the methodology for maintaining System 2 thinking even under pressure.

You'll learn:
- How to recognize System 1 triggers
- The 5-step workflow for staying calm
- Why self-control creates professionals
- Practical techniques you can use today

---

*TFD isn't about writing more tests. It's about knowing exactly what "done" means—before pressure makes you ship something broken.*

**Next: DGTF—Slow is Smooth, Smooth is Fast →**

---

*What's the most valuable test you've ever written? The one that saved you from disaster? Share in the comments.*
