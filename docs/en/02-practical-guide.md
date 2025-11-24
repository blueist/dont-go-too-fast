# ROD, TFD, DGTF: Practical Guide

## Part 1: Theoretical Foundation of Software Development

### Three Core Theories

To understand this methodology, you first need to know three validated theories.

----------

### 1. Daniel Kahneman's Dual Process Theory (Human Thinking)

**System 1 (Fast Thinking)**

- Characteristics:
	- Automatic, unconscious
	- Requires little effort
	- Quick judgment
	- Emotional
	- Pattern recognition

- Advantages:
	- Immediate response
	- Efficient
	- Energy saving

- Disadvantages:
	- Vulnerable to bias
	- Prone to mistakes
	- Unsuitable for complex problems
	- Bad decisions under pressure


**System 2 (Slow Thinking)**

- Characteristics:
	- Deliberate, conscious
	- Requires focus and effort
	- Careful analysis
	- Logical
	- Reasoning

- Advantages:
	- Accurate judgment
	- Complex problem solving
	- Long-term thinking

- Disadvantages:
	- Slow
	- Tiring
	- Energy consuming
	- Deactivates under pressure

**Impact on Software Development:**

- Design Phase (time available):
	- System 2 can be activated
	- Deep thinking
	- Complete design

- Implementation Phase (pressure):
	- System 1 naturally dominates
	- Quick judgment
	- Risk of bad urgent decisions

**Key:**
"Which system to use at which phase matters"


----------

### 2. Donella H. Meadows' Systems Thinking (Understanding Systems)

**What is Systems Thinking:**

- Definition:
	System = collection of interconnected elements
	The whole is greater than the sum of its parts

- Core Principles:
	1. See the whole
	   - Looking only at parts means missing the whole
	2. Understand relationships
	   - Connections between elements matter
	3. Identify feedback loops
	   - Output affects input
	4. Find leverage points
	   - Small changes for big effects
	5. Define system boundaries
	   - What is inside and outside

**Impact on Software Development:**

- Local vs Global Optimization:
	- Even if each function is optimized
	- The entire system can still be slow
	- Relationships and flow matter

- Danger of Missing:
	- If part of the system is missing
	- Unpredictable behavior
	- The whole doesn't work

- Leverage Points:
	- Design phase = most powerful intervention point
	- Hundreds of times more effective than post-implementation fixes

----------

### 3. Genrich Altshuller's TRIZ (Problem Solving)

**What is TRIZ:**

- Theory of Inventive Problem Solving
= Creative problem solving theory

- Origin:
	- Analysis of millions of patents
	- Discovered innovation patterns
	- Repeatable methodology

**Core Concepts:**

1. Technical Contradiction
   "Improving A worsens B"
   Example: "Fast development" vs "High quality"
   
2. TRIZ Resolution Principle
   "Resolve contradictions, don't compromise"
   Methods:
   - Time separation (at different times)
   - Space separation (at different places)
   - Condition separation (in different situations)
   
3. Ideal Final Result (IFR)
   "What if the problem solves itself without additional resources?"
   
4. 40 Inventive Principles
   - Segmentation, extraction, prior action, prior counteraction, etc.
   - Repeatable solution patterns


**Impact on Software Development:**

- Traditional Compromise:
	"Fast" → sacrifice quality
	"Quality first" → sacrifice speed

- TRIZ Solution: Apply time separation
	- Design phase: Slow and accurate
	- Implementation phase: Fast (follow the design)
	- Result: Fast yet high quality

- Ideal Final Result:
"What if confusion disappears by itself during implementation?"
→ ROD: Complete it during the design phase

----------

### Integration of the Three Theories

**Role of Each Theory:**

```
┌─────────────────────────────────┐
│ Kahneman                        │
│ "Why do we make mistakes under  │
│  pressure?"                     │
│                                 │
│ → System 1 vs System 2          │
│ → When to use which thinking    │
└─────────────────────────────────┘
              ↓
┌─────────────────────────────────┐
│ Meadows                         │
│ "What should we design?"        │
│                                 │
│ → Systems Thinking              │
│ → See the whole and relationships│
└─────────────────────────────────┘
              ↓
┌─────────────────────────────────┐
│ Altshuller                      │
│ "How do we resolve contradictions?"│
│                                 │
│ → TRIZ                          │
│ → Resolution without compromise │
└─────────────────────────────────┘
              ↓
┌─────────────────────────────────┐
│ ROD + TFD + DGTF                │
│ "Practical Methods"             │
│                                 │
│ → Concrete application          │
│ → Measurable results            │
└─────────────────────────────────┘

```

**Analyzing Software Development Problems:**

Problem:
- "Must build high-quality software under deadline pressure"

- Kahneman Analysis:  
	- Pressure → System 1 activation
	- System 1 → Bad urgent decisions
	- Result → Technical debt, bugs

- Meadows Analysis:  
	- Looking at parts only → Missing occurs
	- Missing → Incomplete system
	- Adding during implementation → Greater confusion

- Altshuller Analysis:
	- "Fast" vs "Quality" = Contradiction
	- Compromise makes both worse
	- Resolve with separation principles

Solution → ROD + TFD + DGTF


**Practical Application in Programming:**

- Pressure situation:
"Please build the login feature by tomorrow"

- ❌ Traditional approach (compromise):
	System 1: "Let's build it fast!"  
	→ Skip design  
	→ Skip tests  
	→ Hardcoding  
	→ Global variables  
	Result: Fast but poor quality   

- ✅ ROD + TFD + DGTF (contradiction resolution):
	
	1. Time Separation (TRIZ):
	   - This afternoon (2 hours): Design + Tests
		   - System 2 active (Kahneman)
		   - Complete system (Meadows)
	   - Tomorrow morning (3 hours): Implementation
		   - Follow the design
		   - Control with DGTF
	   
	2. Result:
	   Total 5 hours
	   High quality
	   No rework


----------

## Part 2: ROD (Responsibility-Oriented Design)

### Core Principle

**"More is better than missing"**

This doesn't mean "build more," but rather:

> If you define completely in the design phase (Meadows: complete system), you can easily remove unnecessary things during implementation, but adding missing things is dangerous (Kahneman: System 1 explosion)

### Theoretical Background

**Systems Thinking Perspective (Meadows):**

- Systems Theory:
"System behavior is determined by all elements and their relationships"

- ROD Application:
	- Each service = element of the system
	- Call relationships = connections between elements
	- Complete chain = entire system
	- Missing = incomplete system → unpredictable

- Design Principles:  
	1. Identify all elements (services)
	2. Define all relationships (calls)
	3. Clarify boundaries (interfaces)
	4. Design feedback loops (tests)

**Kahneman Perspective:**

- Problem:
Implementation phase = pressure → System 1 dominance

- When Missing is discovered:  
	"Huh? I need this but it's not in the design?"    
	→ System 1 explosion  
	→ "Global variable!", "Singleton!", "Hardcoding!"  
	→ Bad urgent decisions  

- ROD Solution:
	Design phase (relaxed) = System 2 active  
	→ Complete service chain  
	→ No Missing during implementation  
	→ Prevent System 1 explosion  

**TRIZ Perspective:**

- Ideal Final Result (IFR):
	"What if confusion disappears by itself during implementation?"

- ROD Answer:
	"If you define everything in design,  
 implementation just follows"

- Prior Action Principle:
	- Solve before problems occur
	- Define everything in advance during design phase
	- No urgent decisions needed during implementation

- Segmentation Principle:
	- Divide large system into services
	- Develop/test each independently
	- Replaceable (SOLID)

### Problems During Implementation Phase

**Under Pressure (Kahneman + Meadows):**

- Situation:
	- Deadline pressure
	- Requirement changes
	- Not enough time

- Kahneman Analysis:  
	→ Stress  
	→ System 1 dominance  
	→ "Must solve it quickly!"  

- Meadows Analysis:   
	→ Looking at parts only    
	→ Missing the whole system    
	→ Discovering Missing    

- Combined Effect:  
	Missing discovered + System 1 active     
	→ "How do I build this?"  
	→ First solution that comes to mind  
	→ "Global variable!", "Singleton!"  
	→ Technical debt   

### Service Chain Concept (Systems Thinking)

**What is a Service Chain:**

- Applying Meadows' Systems Thinking:
	System = network of services
	- Each service = node of the system
	- Call relationships = edges of the system
	- Data flow = system feedback

Example: User Login System
```
┌─────────────────────────────────┐
│ Login System (Complete)         │
├─────────────────────────────────┤
│                                 │
│ ValidateCredentialsFormat       │
│         ↓                       │
│ FindUserByUsername              │
│         ↓                       │
│ VerifyPassword                  │
│         ↓                       │
│ CheckAccountStatus              │
│         ↓                       │
│ CreateSession                   │
│         ↓                       │
│ StoreSession                    │
│         ↓                       │
│ GenerateSessionToken            │
│         ↓                       │
│ LogLoginEvent                   │
│                                 │
└─────────────────────────────────┘
```
- Characteristics:
	- Each service: Clear responsibility (single)
	- Connection: Explicit call relationships
	- Complete: Entire system
	- No Missing: All elements defined



**Leverage Points (Meadows):**

- Meadows' Teaching:
	"System structure determines behavior"
	"The most powerful intervention point is at the structure level"

- ROD Application: 
	- Design phase = Structure definition
		- Define all services
		- Define all relationships
		- Define interfaces
	- Implementation phase = Behavior implementation
		- Follow already defined structure
		- Minimize structure changes

- Effect:
	2 hours investment in design phase →
	10 hours saved in implementation phase


### Constructor/Static Prohibition Rule

**TRIZ Perspective - Prior Counteraction Principle:**

- Prior Counteraction:
	"Block in advance what could cause problems"

- Problems with Constructor/Static:
	1. System 1's escape route  
	   "Must build it quickly"  
	   → "Just use new User()"  
	   → Skip service design  
	   
	2. Hiding Missing
	   "Where does User come from?"  
	   → Question doesn't arise  
	   → Can't discover Missing  

- ROD Enforcement:
	- Prohibit Constructor/Static  
	→ "Need User? Create UserFinderService"  
	→ Force Missing discovery  
	→ Complete service chain  

**Kahneman Perspective:**

Allowing Constructor = Providing System 1 an easy path

- Design Phase (System 2):  
	- Developer: "I need User"  
	        "Hmm... how to get it?"  
	        "DB? API? Cache?"  
	        → Doesn't think (Constructor is available)  

- Implementation Phase (System 1):  
	- "Need User!"  
	→ "new User()" (immediate response)  
	→ Thoughtless decision  

- ROD Enforcement = Block System 1's easy path  
	→ Force System 2 thinking  
	→ Complete design  

### Finding "Missing" (Systems Thinking)

**System Analysis Technique:**

- Meadows' Question:  
"To fully understand the system?"

1. Identify all elements  
   ❓ "Where does this object come from?"  
   → Missing if no service!  
   
2. Identify all relationships  
   ❓ "How do I get this data?"  
   → Missing if no service!
   
3. Identify all transformations  
   ❓ "How do I create this?"  
   → Missing if no service!
   
4. Identify all storage  
   ❓ "How do I store this?"  
   → Missing if no service!
   
5. Identify all validations  
   ❓ "How do I validate this?"  
   → Missing if no service!
   
6. Identify all error handling  
   ❓ "How do I handle errors?"  
   → Missing if no service!

### Practical Example: User Login

**❌ Design with Missing (Incomplete System):**

- Service Chain:
	1. ValidateCredentials(username, password)
	2. CreateSession(user)  // ← Where does user come from?

- Systems Thinking Analysis:
	- Missing element: User object
	- Missing relationship: How to get User
	- Missing transformation: password verification
	- Missing validation: account status

- Result:  
	→ Incomplete system  
	→ Unpredictable behavior  
	→ Confusion during implementation

**✅ ROD (Complete System):**

Complete Service Chain:

1. ValidateCredentialsFormat(username, password)  
   Role: Input format validation  
   Systems Thinking: Boundary validation (system entrance)  
   
2. FindUserByUsername(username)  
   Role: User lookup  
   Systems Thinking: Data retrieval element   
   
3. VerifyPassword(user, password)  
   Role: Password verification  
   Systems Thinking: Security validation element  
   
4. CheckAccountStatus(user)  
   Role: Account status check  
   Systems Thinking: Status validation element  
   
5. CreateSession(user)  
   Role: Session creation  
   Systems Thinking: State creation element  
   
6. StoreSession(session)  
   Role: Session storage  
   Systems Thinking: Persistence element  
   
7. GenerateSessionToken(session)  
   Role: Token generation  
   Systems Thinking: Output transformation element  
   
8. LogLoginEvent(user, timestamp)  
   Role: Login event logging  
   Systems Thinking: Feedback/monitoring element  

Systems Thinking Validation:  
✓ All elements identified  
✓ All relationships clear  
✓ System boundaries defined  
✓ Feedback loops exist  
✓ No Missing  

TRIZ Validation:  
✓ Prior action complete  
✓ Segmentation principle applied  
✓ Self-contained  

Kahneman Validation:  
✓ Designed with System 2  
✓ System 1 explosion prevented during implementation  

### ROD Checklist

**Theory-Based Validation:**

Systems Thinking (Meadows):  
□ All elements identified?  
□ All relationships defined?  
□ System boundaries clear?  
□ Feedback loops exist?  
□ Leverage points utilized?  

TRIZ (Altshuller):  
□ Prior action applied?  
□ Segmentation principle applied?  
□ Close to Ideal Final Result?  

Kahneman:  
□ Designed with System 2?  
□ System 1 explosion prevention in place?  
□ No confusion elements during implementation?  

Practical:  
□ No Constructor used?  
□ No Static field used?  
□ No assumptions made?  
□ No Missing?  
□ SOLID applied?  

---

## Part 3: TFD (Test-First Development)

### Core Principle

**"Requirements = Tests"**  

Tests are:
- Not afterthoughts but specifications (Systems Thinking: system specs)
- Implementation guide
- Completion criteria
- Feedback loop (Meadows)

### Theoretical Background

**Systems Thinking Perspective (Meadows):**

Feedback Loop:  
"System output affects input"

TFD's Feedback:  
Test → Implement → Verify → Improve → Test

```
┌──────────────────────────────┐
│                              │
│   ┌─────┐      ┌─────┐       │
│   │Test │ ──→  │Impl │       │
│   └─────┘      └─────┘       │
│      ↑            ↓          │
│      │         ┌─────┐       │
│      └───────  │Verify│      │
│                └─────┘       │
│                              │
└──────────────────────────────┘
```

Systems Theory:
- Fast feedback = Fast learning
- Clear criteria = Clear behavior
- Continuous verification = Stable system

**TRIZ Perspective:**

Prior Action Principle:  
"Solve before problems occur"

TFD Application:
- Tests first = Requirements clarification
- Define verification criteria before implementation
- Prevent bugs in advance

The Other Way Round Principle:  
"Traditional: Implement → Problem → Solve"  
"TFD: Predict problem → Test → Implement"  

Self-Service Principle:  
"Tests verify themselves"  
→ No need for manual human verification   
→ Automated feedback

**Kahneman Perspective:**

Using System 2:
Test design = Careful thinking
- "What cases are there?"
- "What errors could occur?"
- "Edge cases?"

Safety Net During Implementation:
Tests = Catching System 1's mistakes
- Even if coding quickly
- Tests catch mistakes
- Provides confidence

### TFD vs TDD

**Differences:**

TDD (Test-Driven Development):
- Write test → Implement → Refactor
- Red-Green-Refactor cycle
- Primarily unit tests
- Technique-focused

TFD (Test-First Development):
- Design (ROD) → Test Design → Implement
- All levels of tests
- Requirements = Tests
- Philosophy-focused

TFD is more comprehensive:
- Unit tests
- Integration tests
- E2E tests
- Performance tests
- Security tests

### Combining ROD and TFD

**Integrated Process (Three Theories Applied):**

Phase 1: ROD (Systems Thinking + Kahneman)  
Complete service chain design
- Design with System 2
- Understand entire system
- Eliminate Missing

Phase 2: TFD (Meadows + TRIZ)  
Design test cases for each service
- Design feedback loops
- Prior action (predict problems)
- Define completion criteria

Phase 3: Implementation (Kahneman + DGTF)  
Implement to pass tests
- Control System 1 with DGTF
- Secure safety net with tests
- Continuous verification

Phase 4: Verification (Meadows)  
All tests pass = Complete  
- Confirm feedback
- Verify system completeness

**Synergy Effect:**

ROD:
"What to build" defined
→ Complete service chain

TFD:
"Does it work correctly" verified
→ Criteria for each service

Together:
= Complete specification
= Executable documentation
= Automatic verification system

### TFD Checklist

**Theory-Based Validation:**

Systems Thinking (Meadows):
□ Feedback loops designed?
□ Covers entire system?
□ Integration tests exist?
□ E2E tests exist?

TRIZ (Altshuller):
□ Errors predicted with prior action?
□ All contradiction cases tested?
□ Approached with reversal thinking? (Tests before implementation)

Kahneman:
□ Tests designed with System 2?
□ All edge cases considered?
□ Tests catch System 1's mistakes?

Practical:
□ ROD service chain complete?
□ Tests for each service?
□ Normal/error/edge cases included?
□ Tests are independent?
□ Sufficient test coverage? (>80%)

---

## Part 4: DGTF (Don't Go Too Fast)

### Core Principle

**"Slow is smooth, smooth is fast"**

Carefulness is:
- Not slow but smooth (TRIZ: contradiction resolution)
- Smooth ends up being fast
- Because there's no rework

### Theoretical Background

**Kahneman Perspective:**

Core Problem:
Pressure → System 1 activation → Bad decisions

DGTF Solution:
Activate System 2 even under pressure
→ Careful judgment
→ Good decisions

Method:
1. Recognition: Detect System 1 triggers
2. Pause: "Wait, let me think"
3. Activation: System 2 ON
4. Proceed: Carefully

**TRIZ Perspective:**

Contradiction:
"Fast" vs "Careful"

Traditional Compromise:
- Fast → Many mistakes
- Careful → Too slow

TRIZ Solution: Time Separation
- Phase separation:
  Design: Slow and accurate (System 2)
  Implementation: Fast but verified (DGTF)
  
- Iteration separation:
  Each iteration: Careful (25 min)
  Break between: Evaluate (5 min)

Result:
→ Fast yet careful
→ Contradiction resolved

**Systems Thinking Perspective:**

Meadows' Teaching:
"Hurrying means missing feedback"

DGTF Application:
Continuously check feedback
- Verify at each step
- Run tests
- Impact analysis

System Behavior:
Careful progress = Stable feedback
→ Early error detection
→ Quick fix
→ Overall faster

### Human Psychology Under Pressure

**Detailed Kahneman Analysis:**

Triggers (System 1 Activation):

Internal Triggers:
- "Must finish quickly"
- "This is simple"
- "Can just do it roughly"
- "Tests can wait"
- "No time to check"

External Triggers:
- "When will it be done?"
- "Can you finish by tomorrow?"
- "It's just a simple thing"
- "Others did it quickly"
- "Demo is tomorrow"

System 1 Activation Signals:

Physical:
- Increased heart rate
- Shallow breathing
- Tense shoulders
- Sweating

Mental:
- Impatience
- Repeating "hurry" thoughts
- Tunnel vision (seeing only parts)
- Unable to think of other things

Behavioral:
- Rushing
- Skipping checks
- Not reading documentation
- Skipping tests
- Choosing first solution

System 1's Typical Decisions:
"Just use a global variable!"
"Singleton will work!"
"Just hardcode it here!"
"Fix it later!"

### Driving Analogy (Three Theories Integration)

**Driver's License ≠ Good Driver**

Bad Driver (System 1 - Kahneman):
Behavior:
- Rush through
- Don't check signals
- Aggressive driving
- "Must get there fast!"

Systems Thinking (Meadows):
- Doesn't see overall traffic flow
- Ignores relationships with other cars
- Ignores feedback (horns, brakes)

Result:
→ Accident (contradiction worsens - TRIZ)
→ Stress
→ Overall slower (dealing with accident)

Good Driver (System 2 + DGTF):
Behavior:
- Check in advance
- Consistent speed
- Maintain safe distance
- Predictive driving

Systems Thinking:
- Understands overall traffic flow
- Considers other cars
- Responds to feedback

TRIZ:
- Contradiction resolved (fast vs safe)
- Time separation (speed adjusted by situation)

Result:
→ No accident
→ Comfortable
→ Overall faster

**Programming is the same:**

Coding ability ≠ Good programmer

Good programmer =
  Technical skill
  + Good habits
  + Careful attitude
  + DGTF

### DGTF Workflow (Three Theories Integration)

**5-Step Process:**

Step 1: Recognition - Kahneman
"Am I rushing right now?"

Check:
□ Increased heart rate?
□ Repeating "hurry" thoughts?
□ Want to skip checks?

→ Confirm System 1 trigger
→ Recognize danger

Step 2: Pause - DGTF
"Wait, let me stop"

Actions:
- Hands off keyboard
- 3 deep breaths
- Wait 5 seconds
- Ask "What's rushing me?"

→ Suppress System 1
→ Prepare to activate System 2

Step 3: Check - Systems Thinking + ROD + TFD
"Check the entire system"

Questions:
□ Did I check the ROD design document?
□ Did I verify TFD test cases?
□ Does this affect other parts? (Systems Thinking)
□ What feedback is there? (Meadows)
□ Any contradictions? (TRIZ)

→ Understand entire system
→ Check Missing
→ Impact analysis

Step 4: Plan - System 2
"Plan carefully"

Questions:
□ What order to proceed?
□ How to test?
□ Expected time?
□ Who to ask if stuck?

→ Clear plan
→ Predictable progress

Step 5: Execute - DGTF + Feedback
"Proceed carefully"

Actions:
→ Implement one service at a time
→ Test at each step
→ Check feedback (Meadows)
→ Verify as you go
→ If problem found → Back to Step 2 (Pause)

### Communication (Three Theories Applied)

**With Managers/Clients:**

❌ Bad Response (System 1):
"Yes, I'll do it right away!"
→ No plan
→ Rework
→ Trust decreases

✅ Good Response (System 2 + Systems Thinking):

Step 1: Recognition
"This is an important request"
"Must answer carefully"

Step 2: Buy time
"I'll give you an accurate timeline after checking"
→ Time to activate System 2

Step 3: Analysis (30 min)
- ROD perspective: How many services?
- Systems Thinking: Impact on other parts?
- TFD: Test scope?
- TRIZ: What contradictions?

Step 4: Honest estimate
"Analysis shows X days needed"

Step 5: Explain (business value)
"If done carefully:
 • No bugs → Customer satisfaction
 • No rework → Cost savings
 • Stable → Risk reduction
 
 If rushed:
 • Bugs → Emergency fixes
 • Rework → Takes longer
 • Technical debt → Future slowdown"

Result:
→ Build trust
→ Realistic schedule
→ Successful deployment

### "DGTF ≠ Slow" (TRIZ Contradiction Resolution)

**Paradox:**

TRIZ Analysis:
Apparent Contradiction: "Carefulness" vs "Speed"
Reality: Not a contradiction (resolved through time separation)

Rushing Developer (System 1):
┌────────────────────────┐
│ Behavior:              │
│ • 100 lines per day    │
│ • 10 bugs              │
│ • Rework needed        │
│                        │
│ Kahneman: System 1     │
│ Systems: Sees only parts│
│ Result: Low productivity│
└────────────────────────┘

DGTF Developer (System 2):
┌────────────────────────┐
│ Behavior:              │
│ • 50 lines per day     │
│ • 1 bug                │
│ • No rework            │
│                        │
│ Kahneman: System 2     │
│ Systems: Sees the whole│
│ Result: High productivity│
└────────────────────────┘

Key (Meadows):
"Lines of code written" ≠ Productivity
"Value integrated into system" = Productivity

Paradox:
Looks slow (carefulness)
Actually fast (no rework)
→ TRIZ: Contradiction resolved

---

## Part 5: How the Three Work Together

### Complete Development Process (Theory Integration)

```
┌──────────────────────────────────────────┐
│ Phase 1: Design (ROD)                    │
│ ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━   │
│                                          │
│ Time: Available                          │
│ Kahneman: System 2 active                │
│ Meadows: Understand entire system        │
│ TRIZ: Prior action                       │
│                                          │
│ Tasks:                                   │
│ • Complete service chain (Systems Thinking)│
│ • Eliminate Missing (completeness)       │
│ • Apply SOLID (replaceable)              │
│ • Contradiction resolution design (TRIZ) │
│                                          │
│ Outputs:                                 │
│ • Service chain document                 │
│ • Interface definitions                  │
│ • Dependency graph                       │
│                                          │
│ Effect: "What to build" is clear         │
└──────────────────────────────────────────┘
              ↓
┌──────────────────────────────────────────┐
│ Phase 2: Test Design (TFD)               │
│ ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━   │
│                                          │
│ Kahneman: System 2 active                │
│ Meadows: Design feedback loops           │
│ TRIZ: Prior protection (predict errors)  │
│                                          │
│ Tasks:                                   │
│ • Test cases for each service            │
│ • All scenarios (normal/error/edge)      │
│ • Integration test design                │
│ • E2E test design                        │
│ • Define completion criteria             │
│                                          │
│ Outputs:                                 │
│ • Test case document                     │
│ • Test structure code (TODO)             │
│ • Feedback mechanism                     │
│                                          │
│ Effect: Verification criteria for        │
│         "does it work correctly"         │
└──────────────────────────────────────────┘
              ↓
┌──────────────────────────────────────────┐
│ Phase 3: Implementation (DGTF)           │
│ ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━   │
│                                          │
│ Time: Pressure begins                    │
│ Kahneman: System 1 dominates (danger)    │
│ Response: Activate System 2 with DGTF    │
│                                          │
│ Safeguards (Three Theories):             │
│ • ROD service chain (guide)              │
│ • TFD tests (criteria)                   │
│ • Systems Thinking (see the whole)       │
│ • TRIZ (contradiction resolution)        │
│ • DGTF workflow (control)                │
│                                          │
│ Execution:                               │
│ • Implement one service at a time carefully│
│ • Test at each step (feedback)           │
│ • Continuous verification                │
│ • Pause when System 1 trigger detected   │
│                                          │
│ Outputs:                                 │
│ • Working code                           │
│ • Passing tests                          │
│ • Clean structure                        │
│ • Documentation                          │
│                                          │
│ Effect: "How to build" safely            │
└──────────────────────────────────────────┘
```

### Synergy of the Three Methodologies (Complete Theory Integration)

```
┌─────────────────────────────────────────┐
│ Kahneman (Human Thinking)               │
│ ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━   │
│                                         │
│ System 1 vs System 2                    │
│ "When to use which thinking"            │
│                                         │
│ Application:                            │
│ • Design: System 2                      │
│ • Implementation: Maintain System 2     │
│   with DGTF                             │
│ • Pressure: Recognize System 1 triggers │
└─────────────────────────────────────────┘
              ↓
┌─────────────────────────────────────────┐
│ Meadows (System Structure)              │
│ ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━   │
│                                         │
│ Systems Thinking                        │
│ "What to design"                        │
│                                         │
│ Application:                            │
│ • ROD: Understand entire system         │
│ • TFD: Feedback loops                   │
│ • Leverage point: Design phase          │
└─────────────────────────────────────────┘
              ↓
┌─────────────────────────────────────────┐
│ Altshuller (Problem Solving)            │
│ ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━   │
│                                         │
│ TRIZ                                    │
│ "How to resolve contradictions"         │
│                                         │
│ Application:                            │
│ • ROD: Prior action, segmentation       │
│ • TFD: Prior protection                 │
│ • DGTF: Time separation (fast vs careful)│
└─────────────────────────────────────────┘
              ↓
┌─────────────────────────────────────────┐
│ ROD + TFD + DGTF (Practice)             │
│ ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━   │
│                                         │
│ Integrated Methodology                  │
│                                         │
│ ROD:                                    │
│ • Understand whole with Systems Thinking│
│ • Prior action with TRIZ                │
│ • Design with Kahneman System 2         │
│                                         │
│ TFD:                                    │
│ • Meadows feedback loops                │
│ • TRIZ prior protection                 │
│ • Test design with Kahneman System 2    │
│                                         │
│ DGTF:                                   │
│ • Kahneman System 1 control             │
│ • TRIZ time separation                  │
│ • Meadows continuous feedback           │
│                                         │
│ Result:                                 │
│ = High-quality software                 │
│ = Predictable deployment                │
│ = Sustainable development               │
└─────────────────────────────────────────┘
```
