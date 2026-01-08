# DGTF++: Practical Guide

## How to Use This Guide

**Prerequisites:**
- Read "DGTF++ Core Concepts" first
- Understand WHY before learning HOW

**This guide provides:**
- Practical rules and methods
- Step-by-step workflows
- Real examples with code
- Checklists for verification
- Common pitfalls and solutions

**Structure:**
1. Quick Reference (Theory summary)
2. ROD in Practice
3. TFD in Practice
4. DGTF in Practice
5. Complete Example
6. For Junior Developers
7. Measuring Success
8. FAQ & Hard Questions
9. Execution Plan

---

## Quick Reference: Theory Summary

For detailed explanations, see "Core Concepts" document.

### Kahneman (When to Think)

```
System 1: Fast, automatic, error-prone, dominant under pressure
System 2: Slow, deliberate, accurate, shuts down under pressure

→ Design with System 2 (calm)
→ Protect System 2 during implementation (DGTF)
```

### Meadows (What to Think About)

```
- See the whole system before parts
- Missing elements cause unpredictable behavior
- Feedback loops enable self-correction
- Design phase = highest leverage point

→ Build complete service chain (ROD)
→ Create feedback with tests (TFD)
```

### TRIZ (How to Resolve Contradictions)

```
"Fast" vs "Quality" is not a trade-off.
Resolve by separation in time:
- Design phase: 100% careful
- Implementation phase: 100% fast (following the plan)

→ Prior action: Do everything before it's needed
→ Segmentation: Divide into independent parts
```

---

## Part 1: ROD in Practice

### The Rules

**Rule 1: No Constructors Inside Services**

```go
// ❌ BAD: Hidden dependency
type OrderService struct{}

func (s *OrderService) CreateOrder(userId string) (*Order, error) {
    user := new(User)  // Where does User come from?
    user.ID = userId   // What data does User have?
    // ... hidden complexity
}

// ✅ GOOD: Explicit dependency
type OrderService struct {
    userFinder UserFinder
}

func (s *OrderService) CreateOrder(userId string) (*Order, error) {
    user, err := s.userFinder.FindByID(userId)
    if err != nil {
        return nil, err
    }
    // ... clear flow
}
```

**Why this rule?**
- `new()` hides dependencies → Missing not discovered
- `new()` is System 1's escape route → "Just create it!"
- Explicit dependencies → Complete service chain

---

**Rule 2: No Static Fields or Methods**

```go
// ❌ BAD: Global state
var globalConfig *Config  // Who sets this? When?

func GetUser(id string) *User {
    db := globalDB  // Hidden dependency
    // ...
}

// ✅ GOOD: Injected dependencies
type UserFinder struct {
    db     Database
    config Config
}

func (f *UserFinder) FindByID(id string) (*User, error) {
    // All dependencies visible
}
```

**Why this rule?**
- Static = global state = hidden connections
- Global state breaks isolation
- Changes in global state affect everything unpredictably

---

**Rule 3: No Implementation Assumptions**

```go
// ❌ BAD: Assuming implementation
type PaymentService struct {
    // Assumes Stripe is always used
}

func (s *PaymentService) Process(amount int) {
    stripe.Charge(amount)  // What if we change to PayPal?
}

// ✅ GOOD: Interface-based
type PaymentGateway interface {
    Charge(amount int) error
}

type PaymentService struct {
    gateway PaymentGateway  // Could be Stripe, PayPal, anything
}

func (s *PaymentService) Process(amount int) error {
    return s.gateway.Charge(amount)
}
```

**Why this rule?**
- Assumptions become surprises
- Surprises trigger System 1
- Interfaces allow safe changes

---

### How to Build a Service Chain

**Step 1: Start with the Requirement**

```
Requirement: "User can log in with email and password"
```

**Step 2: Ask "What Must Happen?"**

```
1. Validate input format
2. Find user by email
3. Verify password
4. Check account status
5. Create session
6. Return token
```

**Step 3: Convert Each Step to a Service**

```
ValidateCredentialsFormat(email, password) → ValidationResult
FindUserByEmail(email) → User
VerifyPassword(user, password) → bool
CheckAccountStatus(user) → AccountStatus
CreateSession(user) → Session
GenerateToken(session) → Token
```

**Step 4: Add Missing Elements**

Ask for each service:
- "Where does the input come from?" → Add service if unclear
- "Where does the output go?" → Add service if unclear
- "What if it fails?" → Add error handling service
- "Who needs to know?" → Add notification/logging service

```
Added:
LogLoginAttempt(email, success, timestamp) → void
HandleFailedLogin(email, attemptCount) → void
NotifySecurityTeam(suspiciousActivity) → void
```

**Step 5: Draw the Complete Chain**

```
┌─────────────────────────────────────────┐
│ Login Service Chain                      │
├─────────────────────────────────────────┤
│                                         │
│ Input: email, password                  │
│         ↓                               │
│ ValidateCredentialsFormat               │
│         ↓ (valid)                       │
│ FindUserByEmail                         │
│         ↓ (found)                       │
│ VerifyPassword                          │
│         ↓ (correct)                     │
│ CheckAccountStatus                      │
│         ↓ (active)                      │
│ CreateSession                           │
│         ↓                               │
│ GenerateToken                           │
│         ↓                               │
│ LogLoginAttempt ←──────────────┐        │
│         ↓                      │        │
│ Output: Token                  │        │
│                                │        │
│ Error paths:                   │        │
│ InvalidFormat ──→ Return 400 ──┘        │
│ UserNotFound ──→ Return 401 ───┘        │
│ WrongPassword ─→ HandleFailedLogin ─┘   │
│ AccountLocked ─→ Return 403 ───┘        │
│                                         │
└─────────────────────────────────────────┘
```

**Step 6: Verify Completeness**

For each path through the chain:
- Can it complete without missing information? → YES = Complete
- Does any service need something not in the chain? → Add it

---

### Example: Payment System

**Requirement:** "User can purchase items in cart"

**Service Chain:**

```
┌─────────────────────────────────────────┐
│ Payment Service Chain                    │
├─────────────────────────────────────────┤
│                                         │
│ 1. Input Validation                     │
│    ValidatePaymentRequest               │
│    FindUserAccount                      │
│    CheckPaymentEligibility              │
│         ↓                               │
│ 2. Calculation                          │
│    GetCartItems                         │
│    CalculateTotalAmount                 │
│    ApplyDiscounts                       │
│    CalculateTax                         │
│         ↓                               │
│ 3. Inventory                            │
│    CheckInventoryAvailability           │
│    ReserveInventory                     │
│         ↓                               │
│ 4. Payment Processing                   │
│    ValidatePaymentMethod                │
│    CreatePaymentTransaction             │
│    CallPaymentGateway                   │
│    ValidatePaymentResponse              │
│         ↓                               │
│ 5. Order Creation                       │
│    CreateOrder                          │
│    ConfirmInventoryReservation          │
│    UpdateUserPurchaseHistory            │
│         ↓                               │
│ 6. Notification                         │
│    SendOrderConfirmation                │
│    SendReceipt                          │
│         ↓                               │
│ 7. Logging                              │
│    LogPaymentEvent                      │
│                                         │
│ Error Handling:                         │
│    ReleaseInventoryReservation          │
│    RefundPayment                        │
│    NotifyPaymentFailure                 │
│                                         │
└─────────────────────────────────────────┘
```

**Key Design Decisions:**

1. **Reservation before Payment:** If payment fails, release reservation
2. **Confirmation after Payment:** Only confirm when payment succeeds
3. **Separate Logging:** Always log, regardless of success/failure

---

### ROD Checklist

```
Design Phase:
□ All requirements mapped to service chains?
□ Each service has single responsibility?
□ All dependencies explicit (no new/static)?
□ All interfaces defined?
□ Error paths included?

Verification:
□ Can each requirement complete through chain alone?
□ No service needs unlisted dependency?
□ No assumptions about implementation?

Signs of Completion:
□ Implementation feels like "just following the chain"
□ No "where does this come from?" questions
□ No "I need to add something" during coding
```

---

## Part 2: TFD in Practice

### From Requirements to Tests

**The Transformation:**

```
Natural Language          →    Test Cases
─────────────────────          ──────────
"User can log in"         →    test_login_success
                               test_login_wrong_password
                               test_login_user_not_found
                               test_login_account_locked
                               test_login_too_many_attempts
```

**Step 1: Identify Happy Path**

```
Requirement: "User can log in"

Happy Path:
- Given: valid email, correct password, active account
- When: login attempted
- Then: returns valid token
```

**Step 2: Identify Failure Cases**

Ask: "What can go wrong?"

```
- Invalid email format
- Email not registered
- Wrong password
- Account locked
- Account not verified
- Too many failed attempts
- System error (DB down)
```

**Step 3: Identify Edge Cases**

Ask: "What about boundaries?"

```
- Empty email
- Empty password
- Very long email (1000 chars)
- Password with special characters
- Unicode in email
- SQL injection attempt
- Concurrent login attempts
```

**Step 4: Write Test Specifications**

```go
func TestLoginService(t *testing.T) {
    // Happy Path
    t.Run("ValidCredentials_ReturnsToken", func(t *testing.T) {
        // Given: valid email, correct password, active account
        // When: Login(email, password)
        // Then: returns token, no error
    })
    
    // Failure Cases
    t.Run("InvalidEmailFormat_ReturnsError", func(t *testing.T) {
        // Given: malformed email
        // When: Login(email, password)
        // Then: returns validation error
    })
    
    t.Run("WrongPassword_ReturnsError", func(t *testing.T) {
        // Given: valid email, wrong password
        // When: Login(email, password)
        // Then: returns auth error, increments failure count
    })
    
    t.Run("AccountLocked_ReturnsError", func(t *testing.T) {
        // Given: valid credentials, locked account
        // When: Login(email, password)
        // Then: returns locked error
    })
    
    t.Run("ThreeFailedAttempts_LocksAccount", func(t *testing.T) {
        // Given: valid email, wrong password
        // When: Login fails 3 times
        // Then: account becomes locked
    })
    
    // Edge Cases
    t.Run("EmptyEmail_ReturnsError", func(t *testing.T) {
        // Given: empty email
        // When: Login("", password)
        // Then: returns validation error
    })
    
    t.Run("VeryLongEmail_HandledCorrectly", func(t *testing.T) {
        // Given: 1000 character email
        // When: Login(longEmail, password)
        // Then: returns validation error (max length exceeded)
    })
}
```

---

### Test Structure: Arrange-Act-Assert

```go
func TestVerifyPassword_CorrectPassword_ReturnsTrue(t *testing.T) {
    // Arrange: Set up preconditions
    hasher := NewBcryptHasher()
    storedHash := hasher.Hash("correctPassword123")
    service := NewPasswordVerifier(hasher)
    
    // Act: Execute the behavior
    result, err := service.Verify("correctPassword123", storedHash)
    
    // Assert: Check the outcome
    if err != nil {
        t.Fatalf("Expected no error, got %v", err)
    }
    if !result {
        t.Error("Expected true, got false")
    }
}
```

---

### Testing Each Service in the Chain

**ROD Chain → TFD Tests**

```
Service Chain:                    Tests:
─────────────                     ──────
ValidateCredentialsFormat    →    test_valid_format
                                  test_empty_email
                                  test_invalid_email
                                  test_empty_password
                                  test_password_too_short

FindUserByEmail              →    test_user_found
                                  test_user_not_found
                                  test_db_error

VerifyPassword               →    test_correct_password
                                  test_wrong_password
                                  test_hash_error

CheckAccountStatus           →    test_active_account
                                  test_locked_account
                                  test_unverified_account
                                  test_deleted_account

CreateSession                →    test_session_created
                                  test_session_has_expiry
                                  test_session_stored

GenerateToken                →    test_token_generated
                                  test_token_contains_claims
                                  test_token_valid_signature
```

**Integration Test:**

```go
func TestLoginFlow_EndToEnd(t *testing.T) {
    // Setup complete system
    db := setupTestDB()
    defer db.Close()
    
    // Create user
    user := createTestUser(db, "test@example.com", "password123")
    
    // Test complete flow
    service := NewLoginService(db)
    token, err := service.Login("test@example.com", "password123")
    
    // Verify
    if err != nil {
        t.Fatalf("Login failed: %v", err)
    }
    if token == "" {
        t.Error("Expected token, got empty string")
    }
    
    // Verify token is valid
    claims, err := validateToken(token)
    if err != nil {
        t.Fatalf("Token invalid: %v", err)
    }
    if claims.UserID != user.ID {
        t.Errorf("Token user mismatch: expected %s, got %s", 
            user.ID, claims.UserID)
    }
}
```

---

### TFD Checklist

```
Test Design:
□ Happy path tested?
□ All failure cases identified and tested?
□ Edge cases covered?
□ Error messages verified?

Test Quality:
□ Each test has single purpose?
□ Tests are independent (no order dependency)?
□ Tests use Arrange-Act-Assert structure?
□ Test names describe behavior?

Coverage:
□ Each ROD service has tests?
□ All public methods tested?
□ Integration tests exist?
□ Coverage > 80%?

Maintenance:
□ Tests don't depend on implementation details?
□ Tests serve as documentation?
□ Failing test clearly indicates problem?
```

---

## Part 3: DGTF in Practice

### The 5-Step Workflow

**Step 1: Recognize**

"Am I rushing right now?"

Check yourself:
- Heart rate increased?
- Thinking "hurry, hurry"?
- Want to skip testing?
- Feel pressure to "just make it work"?

If YES → You're in System 1 territory. Proceed to Step 2.

---

**Step 2: Pause**

"Wait. Let me stop."

Action:
- Hands off keyboard
- 3 deep breaths
- Count to 5
- Ask: "What's making me rush?"

Time required: 5-10 seconds

---

**Step 3: Check**

"Check the system."

Questions:
- Did I check the ROD design?
- Did I check the TFD tests?
- Does this change affect other parts?
- What could go wrong?

Time required: 1-5 minutes

---

**Step 4: Plan**

"Plan the steps."

Write down:
1. What exactly will I change?
2. Which tests should pass after?
3. How will I verify it works?
4. Who should I ask if stuck?

Time required: 2-5 minutes

---

**Step 5: Execute**

"Proceed thoughtfully."

Method:
- One small change at a time
- Run tests after each change
- If anything unexpected → Back to Step 2
- Commit frequently

---

### Real Scenario: "Fix This Bug NOW!"

**Situation:**
```
Friday 4:30 PM
Manager: "Production is broken! Users can't checkout!"
You: (heart rate increases, System 1 activating)
```

**Without DGTF:**
```
→ Jump into code
→ Find something that looks related
→ Change it
→ Deploy
→ Different bug appears
→ Panic more
→ Weekend ruined
```

**With DGTF:**

```
Step 1: Recognize
"I'm panicking. System 1 is taking over."

Step 2: Pause (10 seconds)
Deep breath. "Okay, what do I actually know?"

Step 3: Check (3 minutes)
- Check error logs: "PaymentGateway timeout"
- Check monitoring: Started 30 minutes ago
- Check recent deployments: None today
- Check external status: Payment provider is down

Step 4: Plan (2 minutes)
- This is external, not our bug
- Options:
  a) Wait for provider (they say 30 min)
  b) Switch to backup provider
  c) Enable "retry later" for users
- Decision: Enable retry, notify users

Step 5: Execute (10 minutes)
- Enable retry mechanism (pre-built, tested)
- Add user-facing message
- Monitor
- Notify manager with timeline

Total time: ~15 minutes
Result: Handled professionally, no panic changes
```

---

### Communication Templates

**When Manager Asks "How Long?"**

❌ System 1 Response:
```
"I'll have it done by end of day!"
(No analysis, will probably fail)
```

✅ DGTF Response:
```
"Let me check the scope and give you an accurate estimate."

[30 minutes later]

"I've analyzed the request:
- It touches 3 services
- Need 5 new test cases
- Estimated: 2 days

If we rush, we risk:
- Bugs affecting users
- More time fixing later

Recommended timeline: 2 days with proper testing."
```

---

**When Asked to Skip Testing**

❌ System 1 Response:
```
"Okay, I'll skip tests to go faster"
(Technical debt, future pain)
```

✅ DGTF Response:
```
"I understand the urgency. Let me explain the trade-off:

With tests (2 days):
- 95% confidence it works
- No weekend emergencies
- Maintainable later

Without tests (1 day):
- 50% chance of bugs
- Likely weekend fix needed
- Future changes risky

I recommend the 2-day approach. 
But if we must ship in 1 day, 
I'll document the risk and we should plan 
for immediate follow-up testing."
```

---

### Anti-Patterns to Avoid

**Anti-Pattern 1: "Just One Quick Fix"**

```
Symptom: "I'll just change this one line..."
Reality: One line becomes ten, with no tests
Result: Bug shipped, trust lost

Solution: Even "one line" gets the 5-step treatment
```

**Anti-Pattern 2: "I'll Add Tests Later"**

```
Symptom: "Let me get it working first, then test"
Reality: "Later" never comes
Result: Untested code in production

Solution: Write test specification BEFORE coding
```

**Anti-Pattern 3: "It Works On My Machine"**

```
Symptom: "Tests pass locally, ship it"
Reality: Environment differences cause failures
Result: Production bug

Solution: CI/CD with consistent environment
```

**Anti-Pattern 4: "The Deadline Justifies Everything"**

```
Symptom: "We HAVE to ship today, no matter what"
Reality: Shipping broken code is worse than delay
Result: Emergency fixes, lost trust, more delay

Solution: Negotiate scope, not quality
```

---

### DGTF Checklist

```
Before Starting Work:
□ Am I calm? (If not, pause first)
□ Do I have the ROD design?
□ Do I have the TFD tests?
□ Do I understand what "done" means?

During Work:
□ Am I following the service chain?
□ Am I running tests after each change?
□ Am I committing frequently?
□ Do I feel rushed? (If yes, pause)

When Pressured:
□ Did I pause before responding?
□ Did I give honest estimate?
□ Did I explain trade-offs?
□ Did I document the decision?

At the End:
□ All tests pass?
□ Code reviewed?
□ Documentation updated?
□ Ready for next person to maintain?
```

---

## Part 4: Complete Example - E-commerce Shopping Cart

### Requirement

"User can add items to cart, view cart, and checkout"

### Step 1: ROD - Build Service Chain

**Sub-requirement 1: Add to Cart**

```
AddToCartService Chain:
├── ValidateAddToCartRequest
├── FindUser
├── FindProduct
├── CheckProductAvailability
├── GetOrCreateCart
├── AddItemToCart
├── CalculateCartTotal
├── SaveCart
└── LogCartEvent
```

**Sub-requirement 2: View Cart**

```
ViewCartService Chain:
├── ValidateViewCartRequest
├── FindUser
├── FindCart
├── GetCartItems
├── EnrichCartItems (add product details)
├── CalculateCartTotal
├── ApplyPromotions
└── ReturnCartView
```

**Sub-requirement 3: Checkout**

```
CheckoutService Chain:
├── ValidateCheckoutRequest
├── FindUser
├── FindCart
├── ValidateCartItems (still available?)
├── CalculateTotal
├── ApplyDiscounts
├── CalculateTax
├── CalculateShipping
├── ReserveInventory
├── ProcessPayment
│   ├── ValidatePaymentMethod
│   ├── ChargePayment
│   └── HandlePaymentResult
├── CreateOrder
├── ConfirmInventoryReduction
├── ClearCart
├── SendConfirmationEmail
└── LogCheckoutEvent
```

### Step 2: TFD - Design Tests

**Tests for AddItemToCart:**

```go
func TestAddItemToCart(t *testing.T) {
    // Happy path
    t.Run("ValidItem_AddsToCart", func(t *testing.T) {})
    t.Run("ItemAlreadyInCart_IncreasesQuantity", func(t *testing.T) {})
    
    // Failure cases
    t.Run("ProductNotFound_ReturnsError", func(t *testing.T) {})
    t.Run("ProductOutOfStock_ReturnsError", func(t *testing.T) {})
    t.Run("UserNotFound_ReturnsError", func(t *testing.T) {})
    t.Run("InvalidQuantity_ReturnsError", func(t *testing.T) {})
    
    // Edge cases
    t.Run("QuantityExceedsStock_ReturnsError", func(t *testing.T) {})
    t.Run("MaxCartItems_ReturnsError", func(t *testing.T) {})
}
```

**Tests for Checkout:**

```go
func TestCheckout(t *testing.T) {
    // Happy path
    t.Run("ValidCart_CompletesCheckout", func(t *testing.T) {})
    
    // Failure cases
    t.Run("EmptyCart_ReturnsError", func(t *testing.T) {})
    t.Run("ItemNoLongerAvailable_ReturnsError", func(t *testing.T) {})
    t.Run("PaymentFails_ReturnsError", func(t *testing.T) {})
    t.Run("PaymentFails_ReleasesInventory", func(t *testing.T) {})
    
    // Edge cases
    t.Run("ConcurrentCheckout_OnlyOneSucceeds", func(t *testing.T) {})
    t.Run("PriceChangedDuringCheckout_NotifiesUser", func(t *testing.T) {})
}
```

### Step 3: DGTF - Implementation

**Day 1: Setup and Core Services**

```
Morning (System 2 active):
- Review ROD design
- Review TFD tests
- Set up project structure
- Implement interfaces

Afternoon:
- Implement ValidateRequest services
- Implement FindUser, FindProduct
- Run tests: 5 pass, 15 pending
- Commit
```

**Day 2: Cart Logic**

```
Morning:
- Implement GetOrCreateCart
- Implement AddItemToCart
- Implement CalculateTotal
- Run tests: 12 pass, 8 pending
- Commit

Afternoon:
- Implement SaveCart
- Implement ViewCart flow
- Run tests: 18 pass, 2 pending
- Commit
```

**Day 3: Checkout Flow**

```
Morning:
- Implement inventory reservation
- Implement payment processing
- Run tests: 22 pass, 3 fail (edge cases)
- Debug with System 2 (not panic)

Afternoon:
- Fix edge cases
- Implement order creation
- Run tests: 25 pass
- Commit
```

**Day 4: Polish and Edge Cases**

```
Morning:
- Implement notification services
- Add logging
- Run full test suite: All pass
- Code review

Afternoon:
- Address review comments
- Final testing
- Documentation
- Ready for deployment
```

### Result

```
Time spent: 4 days
Tests: 25 passing
Coverage: 87%
Bugs in production: 0

Compare to rushing (estimated):
Time spent: 2 days coding + 3 days fixing
Tests: 5 (added after bugs)
Coverage: 40%
Bugs in production: 8
Customer complaints: 12
Weekend ruined: Yes
```

---

## Part 5: For Junior Developers

### Where to Start

**Week 1-2: Understand the Problem**

Don't start with methodology. Start with understanding WHY good practices exist.

Exercise:
1. Think of a time you wrote "quick fix" code
2. What happened next?
3. How much time did you spend fixing it?

This is why DGTF++ exists.

### Month 1: Start with DGTF

Before learning ROD or TFD, learn to recognize rushing.

Daily practice:
- Every time you feel "hurry," pause for 5 seconds
- Ask: "What's making me rush?"
- Write it down

This builds the habit of recognition.

### Month 2: Add TFD

Start writing tests before code.

Daily practice:
- Before writing any function, write one test
- Just one. Not perfect coverage.
- Build the habit.

```go
// Before writing the function:
func TestAdd_TwoPositiveNumbers_ReturnsSum(t *testing.T) {
    result := Add(2, 3)
    if result != 5 {
        t.Errorf("Expected 5, got %d", result)
    }
}

// Then write the function:
func Add(a, b int) int {
    return a + b
}
```

### Month 3: Add ROD

Start thinking about service chains.

Daily practice:
- Before implementing a feature, draw the services
- Just boxes and arrows on paper
- Ask: "What's missing?"

### Common Mistakes

**Mistake 1: Trying Everything at Once**

```
❌ "I'll apply ROD + TFD + DGTF perfectly from day one"
→ Overwhelm → Give up

✅ "I'll start with just pausing when I rush"
→ Small win → Build on it
```

**Mistake 2: Seeking Perfection**

```
❌ "My service chain must be perfect"
→ Analysis paralysis → Nothing done

✅ "My service chain is good enough to start"
→ Iterate and improve
```

**Mistake 3: Applying to Everything**

```
❌ "I must use ROD for this one-line script"
→ Overhead kills productivity

✅ "ROD for features, quick scripts can be simple"
→ Right tool for the job
```

### Finding a Mentor

Look for someone who:
- Writes tests before code (naturally)
- Stays calm under pressure
- Asks "what could go wrong?" often
- Is willing to explain their thinking

Ask them:
- "Can I see how you design a feature?"
- "How do you decide what to test?"
- "How do you handle deadline pressure?"

---

## Part 6: Measuring Success

### Individual Metrics

**Before DGTF++:**
```
- Bugs introduced per feature: 5-10
- Time spent debugging: 40%
- Weekend work: Frequent
- Confidence when deploying: Low
```

**After DGTF++ (3 months):**
```
- Bugs introduced per feature: 1-2
- Time spent debugging: 15%
- Weekend work: Rare
- Confidence when deploying: High
```

### Team Metrics

Track monthly:
```
- Escaped defects (bugs found in production)
- Rework ratio (time fixing vs building)
- Deployment confidence score (1-10)
- Test coverage percentage
```

### Signs of Progress

**Week 2:**
- You notice when you're rushing
- You pause sometimes (not always)

**Month 1:**
- Pausing becomes habit
- You write some tests before code
- Fewer "emergency" fixes

**Month 3:**
- Service chains feel natural
- Tests are automatic part of workflow
- Colleagues notice your code quality

**Month 6:**
- Others ask how you stay calm
- You can explain WHY you work this way
- Junior developers want to learn from you

---

## Part 7: FAQ & Hard Questions

### Q: "This takes too much time upfront"

**A:** Let's compare total time:

```
Without DGTF++:
- Design: 0.5 days
- Code: 2 days
- Debug: 3 days
- Fix production bugs: 2 days
- Total: 7.5 days

With DGTF++:
- ROD: 0.5 days
- TFD: 0.5 days
- Code: 2 days
- Debug: 0.5 days
- Production bugs: 0
- Total: 3.5 days
```

The upfront time saves more than it costs.

### Q: "My manager wants results NOW"

**A:** Communicate in business terms:

"I can ship in 2 days with testing, or 1 day without.

Without testing:
- 50% chance of production bug
- Bug fix will take 1-2 days
- Customer impact: negative reviews
- Expected total time: 3 days

With testing:
- 95% confidence it works
- Minimal customer impact
- Expected total time: 2 days

Which do you prefer?"

### Q: "How to apply to legacy code?"

**A:** Gradually.

```
Month 1: New features only
- Apply ROD + TFD to new code
- Don't touch legacy
- Build team skill

Month 2-3: Boy Scout Rule
- When modifying legacy, add tests
- Small improvements only
- Don't rewrite

Month 4-6: Strategic improvement
- Identify highest-risk modules
- Apply ROD redesign
- Comprehensive testing

Never: Big Bang Rewrite
- Too risky
- Too expensive
- Usually fails
```

### Q: "I'm the only one using this. How to spread?"

**A:** By results, not preaching.

```
Month 1-2: Demonstrate
- Apply methodology quietly
- Track your metrics
- Let quality speak

Month 3: Share when asked
- "How do you avoid bugs?"
- "Why is your code so clean?"
- Then explain

Month 4+: Pair programming
- Offer to work together
- Show, don't tell
- Let them experience it
```

### Q: "What about urgent production issues?"

**A:** DGTF still applies—just faster.

```
Normal DGTF:
- Pause: 30 seconds
- Check: 5 minutes
- Plan: 5 minutes

Emergency DGTF:
- Pause: 5 seconds (still pause!)
- Check: 1 minute (what do I know?)
- Plan: 1 minute (simplest safe fix)

The pause is never zero.
Even 5 seconds prevents panic mistakes.
```

### Hard Questions (Devil's Advocate)

No methodology is perfect, and honest critique strengthens understanding. These questions address real limitations and concerns. If you're skeptical, you should be—skepticism is System 2 working correctly.

#### Q1: "How can I recognize System 1 when System 1 is active?"

**The Paradox:** DGTF Step 1 says "recognize you're rushing." But System 1's nature is unconscious. In true panic, you don't know you're panicking. "I'm fine" might be the most dangerous thought.

**Honest Answer:**

You often cannot recognize it in the moment. This is a genuine limitation. That's why DGTF emphasizes:

1. **External triggers**, not self-awareness
   - Deadline announced → automatic pause
   - Bug reported → automatic pause
   - "Urgent" in message → automatic pause
   
2. **Habits, not willpower**
   - Don't rely on recognizing panic
   - Build reflexes that trigger before panic

3. **Environmental design**
   - Pre-commit hooks that force tests
   - PR requirements that force review
   - Physical cues (sticky notes, timers)

The goal isn't perfect self-awareness. It's reducing the situations where you need it.

#### Q2: "What about ego depletion? Willpower runs out."

**The Reality:** Self-control is a finite resource. End of day, end of week, end of project—when willpower is exhausted, how do you practice DGTF?

**Honest Answer:**

This is a real limitation. DGTF doesn't solve fatigue. But:

1. **Reduce willpower dependency**
   - Make DGTF automatic (habit)
   - Use external forcing functions
   - When depleted, stop working—seriously

2. **Sustainable pace**
   - DGTF includes "don't overwork"
   - A 60-hour week makes DGTF impossible
   - This is a symptom of organizational failure

3. **Accept imperfection**
   - You will fail DGTF when exhausted
   - That's information: you're too tired
   - Go home

#### Q3: "What if my environment is toxic?"

**The Reality:** "Slow and careful" could get you fired. Some organizations punish quality.

**Honest Answer:**

DGTF assumes a minimum of organizational rationality. In truly toxic environments:

1. **Short-term: Survive**
   - Apply DGTF where safe
   - Document your quality work
   - Build a portfolio

2. **Medium-term: Decide**
   - Is this environment changeable?
   - Are there allies who want quality?
   - What's your leverage?

3. **Long-term: Exit or Change**
   - Toxic environments don't fix themselves
   - Your career is longer than this job
   - DGTF skills transfer to better places

**What DGTF cannot solve:** Systemic organizational dysfunction. No personal methodology can fix a broken company. This is not a failure of the methodology—it's a recognition of its scope.

In some environments, DGTF may not be externally rewarded. However, you still benefit personally: less stress, cleaner code, better skills, and a stronger portfolio for your next role.

#### Q4: "DGTF is unverifiable. How do I know I'm doing it?"

**The Reality:** TFD has pass/fail. ROD has "missing found." DGTF measures internal state—unmeasurable.

**Honest Answer:**

Direct measurement is impossible. Use proxy metrics:

```
Proxy Metrics for DGTF:
- Bugs per feature (should decrease)
- "Emergency" fixes (should decrease)
- Code review iterations (should decrease)
- Personal stress level (should decrease)
- Regrettable commits (should decrease)
```

Also: **DGTF fails visibly**. When you skip it, consequences appear. Bugs emerge. Rework happens. The absence of DGTF is measurable.

#### Q5: "The virtuous cycle assumes rational managers"

**The Reality:** DGTF → Trust → Autonomy → Less Pressure → Easier DGTF... assumes managers reward quality. Many don't.

**Honest Answer:**

Yes, this cycle requires minimally rational management. If your manager:
- Punishes quality
- Rewards only speed
- Ignores outcomes

Then the cycle breaks. Your options:
1. Find different management
2. Build evidence that changes their mind
3. Accept that DGTF may not be externally rewarded—while still valuable for your own growth

Not every environment supports quality practices. But the skills you build remain yours.

#### Q6: "ROD requires experience. Juniors can't find Missing."

**The Reality:** "Design completely" requires knowing what's missing. Juniors lack the experience to predict missing elements.

**Honest Answer:**

True. ROD alone is senior-dependent. But:

1. **Juniors can learn patterns**
   - Common missing elements are documented
   - Error handling, logging, validation, cleanup
   - These are predictable

2. **Juniors need mentors**
   - Design review with seniors
   - "What did I miss?" sessions
   - Learning Missing over time

3. **TFD helps juniors**
   - Tests reveal Missing earlier
   - Failure feedback is faster
   - Learning accelerates

ROD is harder for juniors. That's why DGTF++ includes "For Junior Developers" section—acknowledge the difficulty, provide scaffolding.

#### Q7: "How do I distinguish 'feels urgent' from 'is urgent'?"

**The Reality:** Production down costs money. Every second matters. Where's the line?

**Honest Answer:**

```
True Urgency Examples:
- Security breach in progress → Act fast (but still think)
- Data corruption spreading → Contain immediately
- Revenue loss per second → Prioritize ruthlessly

False Urgency Examples:
- Manager said "ASAP" → Probably not life-or-death
- Customer complained loudly → Important but not urgent
- Deadline is tomorrow → You had weeks; this is planning failure
```

Even in true emergencies, DGTF applies—just compressed:
- 5 seconds of pause prevents wrong server shutdown
- 1 minute of diagnosis prevents data loss
- "Fast" doesn't mean "thoughtless"

The highest-stakes environments (surgery, aviation) have the MOST pauses and checklists, not fewer.

#### Q8: "DGTF is individual. What about team pressure?"

**The Reality:**
- Pair programming partner says "just do it"
- Code review asks "why so slow?"
- Standup pressure to show progress

**Honest Answer:**

Team DGTF requires:

1. **Team agreement**
   - Discuss methodology as a team
   - Agree on standards
   - "We value quality" as shared value

2. **Explicit communication**
   - "I'm pausing to think through this"
   - "I want to get this right"
   - Name what you're doing

3. **Handling pressure**
   - "I understand the urgency. Let me check one thing."
   - "I can go faster, but we'll likely have bugs. Your call."
   - Make tradeoffs explicit

4. **Cultural change** (hard)
   - One person can model behavior
   - Results eventually speak
   - Not always possible

#### Q9: "What's the recovery strategy when DGTF fails?"

**The Reality:** You panicked. Used a global variable. Skipped tests. What now?

**Honest Answer:**

DGTF failure recovery:

```
1. Acknowledge (don't deny)
   - "I rushed that"
   - No shame, just recognition

2. Contain
   - Don't compound the problem
   - Stop rushing NOW
   - Prevent spread

3. Assess
   - What did I skip?
   - What's the damage?
   - What's the risk?

4. Fix or Accept
   - If fixable now: fix properly
   - If not: document as tech debt
   - Plan remediation

5. Learn
   - What triggered the rush?
   - How to prevent next time?
   - Update environment/habits
```

**Key insight:** DGTF failure is information. It tells you something about your environment, your state, or your triggers. Use it.

#### Q10: "Reading this document triggers System 1"

**The Meta-Problem:** Understanding DGTF conceptually feels like learning it. "Got it, next!" But reading is System 1. You haven't actually learned anything.

**Honest Answer:**

Absolutely true. Reading ≠ Learning. That's why this guide emphasizes:

1. **Practice, not reading**
   - "Execution Plan" section
   - Weekly practice goals
   - Deliberate application

2. **Time requirements**
   - "Month 1: Conscious incompetence"
   - "Month 3: Conscious competence"
   - "Month 6+: Unconscious competence"

3. **Failure as teacher**
   - You will fail DGTF
   - Failure teaches what reading cannot
   - This is expected and necessary

Reading this guide and feeling you "understand" is just the beginning. Real understanding comes from failed attempts, corrections, and gradual improvement over months. The document is a map; walking the territory is different.

---

## Part 8: Execution Plan

### Week 1: Understanding

```
Day 1-2:
□ Read Core Concepts document
□ Understand System 1/2
□ Understand service chains

Day 3-4:
□ Read this Practical Guide
□ Take notes on what resonates
□ Identify one feature to practice

Day 5:
□ Discuss with mentor/colleague
□ Answer: "Why do I want to try this?"
□ Set 4-week goal
```

### Week 2: DGTF Practice

```
Daily:
□ Notice when rushing (at least 3 times)
□ Pause (even if briefly)
□ Journal what triggered rush

End of week:
□ Review journal
□ Common triggers identified?
□ Pausing becoming easier?
```

### Week 3: TFD Practice

```
Daily:
□ Write at least 1 test before code
□ Keep tests simple
□ Run tests frequently

End of week:
□ Count tests written
□ Compare bug count to previous weeks
□ Feeling of testing "before" vs "after"?
```

### Week 4: ROD Practice

```
Daily:
□ Sketch service chain before coding
□ Just boxes and arrows
□ Ask "what's missing?"

End of week:
□ Review sketches
□ Did chains prevent confusion?
□ Implementation easier?
```

### Month 2-3: Integration

```
Weekly:
□ Apply all three to one feature
□ Track metrics (bugs, time, confidence)
□ Review and adjust

Monthly:
□ Compare metrics to Month 1
□ Share learnings with team
□ Adjust approach based on experience
```

### Month 4-6: Mastery

```
Goals:
□ DGTF is automatic
□ TFD is default workflow
□ ROD is natural thinking
□ Metrics significantly improved
□ Can teach others
```

---

## Final Notes

### Remember

```
DGTF++ is not about:
- Being perfect
- Being slow
- Following rules blindly

DGTF++ is about:
- Making better decisions
- Sustainable pace
- Professional results
```

### The Journey

```
Day 1: "This seems like a lot"
Week 1: "I noticed I was rushing!"
Month 1: "I wrote tests first today"
Month 3: "Why didn't I learn this earlier?"
Month 6: "This is just how I work now"
```

### When It Clicks

You'll know DGTF++ is working when:
- You feel calm during pressure
- Your code needs less debugging
- Colleagues trust your estimates
- You look forward to deployment

**Good luck. Take it slow. That's the point.**

---

**Document Version**: 2.2  
**Document Type**: Practical Guide  
**Prerequisites**: Core Concepts v2.1  
**Updated**: 2026-01
