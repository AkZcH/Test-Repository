> **Video:** What I'd Learn Instead of Coding in 2027  
> **Channel:** Maddy Zhang  
> **URL:** [http://www.youtube.com/watch?v=q3U-KYNzCGM](http://www.youtube.com/watch?v=q3U-KYNzCGM)

## Core Thesis

The value of software engineers is shifting from **writing code** toward **understanding, verifying, directing, and integrating software systems**.

AI can generate large amounts of code, but humans remain responsible for:

- Verification
- Judgment
- Problem scoping
- Agent management
- Communication
- System alignment

> **Key idea:** AI reduces the cost of producing code. It does not eliminate the need to determine whether the code should exist, whether it is correct, or what problem it should solve.

---

## 1. The Shift in Software Engineering

### What is changing?

- AI can now generate a significant portion of new code.
- Engineering hiring remains relatively resilient despite widespread tech layoffs.
- Developers increasingly spend time **reviewing and debugging AI-generated code**.
- AI agents can operate for long periods, but often spend substantial time waiting for human direction.
- This makes **human attention, judgment, and decision-making** major bottlenecks.

### Implication

The competitive advantage is moving from:

```
Knowing how to write code
        ↓
Knowing how to make software work
        ↓
Knowing what to build
        ↓
Knowing how to verify and direct AI-built software
```

---

# 2. Skill: Verification

> **Verification = reading code you didn't write and determining whether it is correct, reliable, secure, and maintainable.**

AI-generated code creates a new problem:

**Someone still needs to determine whether the output is actually good.**

### What to develop

- Code review
- Debugging
- Security analysis
- Performance reasoning
- Concurrency reasoning
- Understanding edge cases
- Understanding failure modes

### Fundamentals become more important

Use these as an auditing checklist:

- Data Structures & Algorithms
- Operating Systems
- Databases
- Networking
- Concurrency
- Security
- Distributed Systems

### Practice

1. Pick an open-source issue.
2. Give the issue to an AI coding agent.
3. Inspect the generated diff **before running tests**.
4. Predict:
    - Edge cases
    - Race conditions
    - Timeout issues
    - Security problems
    - Incorrect assumptions
5. Run tests.
6. Compare your predictions with the actual failures.

### Key takeaway

> **The ability to generate code is becoming cheap. The ability to recognize bad code is becoming valuable.**

---

# 3. Skill: Judgment & Problem Scoping

AI is good at solving **defined problems**.

Humans still need to define the problem.

### The shift

Traditional junior developer:

```
Receive task
    ↓
Implement task
```

AI-era engineer:

```
Ambiguous problem
    ↓
Understand context
    ↓
Define requirements
    ↓
Identify constraints
    ↓
Evaluate trade-offs
    ↓
Delegate implementation
    ↓
Verify result
```

### Important abilities

- Breaking ambiguous problems down
- Identifying constraints
- Choosing appropriate abstractions
- Evaluating trade-offs
- Knowing what **not** to build
- Making architectural decisions

### Practice: Decision Logs

Maintain a `DECISIONS.md` or decision log inside projects.

Document:

```
Decision
Context
Options considered
Chosen approach
Why?
Trade-offs
Rejected alternatives
Consequences
```

Example:

```
Decision: PostgreSQL instead of MongoDB

Reason:
The system requires relational integrity,
transactions, and complex queries.

Rejected:
MongoDB because schema flexibility is not
valuable enough to justify weaker relational guarantees.
```

---

# 4. Skill: Managing AI Agents

The engineer increasingly becomes an **orchestrator of software agents**.

Instead of manually performing every step:

```
Human → Code → Test → Review → Deploy
```

The workflow becomes:

```
Human
  ↓
Specification
  ↓
Agent → Implementation
  ↓
Agent → Review
  ↓
Automated Tests
  ↓
Human Verification
  ↓
Merge
```

### Meta-work

Important skills include:

- Writing structured specifications
- Designing agent workflows
- Creating review pipelines
- Defining validation criteria
- Managing context
- Building automated feedback loops

### Practice

Build a simple agent pipeline:

```
Agent 1
  ↓
Implement specification
  ↓
Agent 2
  ↓
Review diff against requirements
  ↓
Test Suite
  ↓
Human approval
  ↓
Merge
```

### Key principle

> Don't just learn how to use an AI coding tool. Learn how to **design a reliable system around it**.

---

# 5. Skill: Communication & System Alignment

AI-heavy engineering does **not** eliminate communication.

It can actually increase it.

AI-generated implementations can expose:

- Hidden technical debt
- Missing requirements
- Organizational constraints
- Architectural disagreements
- Poor documentation
- Unclear ownership

### Design documents become more valuable

A good design document serves two purposes:

```
Human alignment
       +
AI specification
```

### Practice

Before implementing a significant change:

1. Write the problem.
2. Define requirements.
3. Define constraints.
4. Explain the proposed architecture.
5. Document trade-offs.
6. Write the PR description.
7. Then implement.

### Communication skills

Learn to explain:

- Why a particular architecture was chosen
- What trade-offs exist
- What risks remain
- What the system cannot guarantee
- Why a simpler solution wasn't sufficient

---

# 6. Career Guidance

## Students

Focus on demonstrating **reasoning**, not code volume.

### Portfolio should show

- Architecture diagrams
- Decision logs
- Design documents
- Tests
- Benchmarks
- Failure analysis
- Incident reports
- AI-assisted development workflows

Don't optimize for:

> "I wrote 20,000 lines of code."

Optimize for:

> "I designed the system, evaluated the trade-offs, used AI to accelerate implementation, and verified the result."

---

## Early-Career Engineers

Seek ambiguous problems.

Good opportunities:

- Migrations
- Incident response
- Post-mortems
- Reliability improvements
- Performance investigations
- Infrastructure work
- System redesigns

These force you to develop **problem-definition skills**, not just implementation skills.

---

## Mid/Senior Engineers

Focus on areas where AI provides high leverage:

- System Architecture
- Reliability
- Infrastructure
- Security
- Distributed Systems
- Technical Strategy

The higher the abstraction level, the more important **judgment and system understanding** become.

---

# 7. The New Engineering Skill Stack

```
                 AI-Native Engineer
                         │
        ┌────────────────┼────────────────┐
        │                │                │
   Verification      Judgment        Agent Management
        │                │                │
   Fundamentals    Problem Scoping   AI Workflows
        │                │                │
        └────────────────┼────────────────┘
                         │
                  Communication
                         │
                  System Thinking
```

### Priority order

1. **Fundamentals**
2. **Verification**
3. **Problem Scoping**
4. **System Design**
5. **AI Agent Management**
6. **Communication**
7. **Domain Expertise**

---

# Personal Takeaway

The title **"What I'd Learn Instead of Coding"** is slightly misleading.

The answer isn't to stop coding.

It's to stop treating **typing code as the core engineering skill**.

The stronger model for 2027 is:

> **Understand → Specify → Delegate → Verify → Iterate**

AI becomes the implementation multiplier.

**Engineering judgment remains the control layer.**