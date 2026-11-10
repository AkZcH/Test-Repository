Yes. I found the recent versions. The one you're remembering is essentially this:

## Your Engineering Learning Loop

**Learn → Build → Break → Fix → Explain → Document → Publish → Repeat**

### 1. Learn

Learn just enough theory and fundamentals to understand what you're about to build.

### 2. Build

Build something real. Avoid tutorial-following as the primary activity.

### 3. Break

Intentionally push it:

- Increase load
- Feed bad inputs
- Create race conditions
- Kill processes
- Introduce failures
- Test edge cases
- Benchmark it

### 4. Fix

Debug the failure yourself.

Read documentation, source code, papers, issues, and implementation details **only when necessary**.

### 5. Explain

Be able to explain:

- What you built
- Why it works
- Why it failed
- What caused the failure
- What you changed
- What tradeoffs you made

### 6. Document

Turn the work into artifacts:

- README
- Architecture diagram
- Benchmarks
- Failure analysis
- Design decisions
- Lessons learned

### 7. Publish

Put the work into the world:

- GitHub
- Technical article
- LinkedIn/X post
- Demo/video

### 8. Repeat

Take what you learned and attack the next problem.

**The important principle was:**

> **Don't optimize for consuming knowledge. Optimize for producing evidence that you understand it.**

And the more complete version we discussed was:

**Learn → Build → Measure → Explain → Publish → Discuss → Improve → Repeat**

For your AI Infrastructure direction, I'd use the first loop as the **day-to-day engineering loop**, and the second as the **project/lifecycle loop**.