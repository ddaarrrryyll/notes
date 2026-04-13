## TL;DR
- Ensures a class has **exactly one instance per process**
- Provides a **global access point** to that instance

Use when:
- You need a single shared resource within a runtime (e.g. config, logger)

---

## Mental Model

"One object, globally accessible, within a single application runtime"

- Created once
- Reused everywhere
- Lives for the lifetime of the process

---

## Structure

Typical implementation ensures:
1. Private constructor
2. Static instance holder
3. Global access method

---

## Example (Conceptual)

- Logger
- Configuration manager
- In-memory cache (per process)
- Thread pool manager

---

## Key Properties

- Single instance per process
- Lazy or eager initialization
- Globally accessible
- Shared mutable state possible (important risk)

---

## Why it exists

Avoid:
- Multiple inconsistent instances
- Expensive repeated initialization
- Shared resource duplication

---

## Trade-offs

| Aspect        | Benefit                         | Risk                              |
|--------------|----------------------------------|-----------------------------------|
| Simplicity   | Easy global access              | Hidden dependencies              |
| Memory       | Single shared instance          | Long-lived state accumulation    |
| Testing      | Centralized control             | Hard to mock / replace           |
| Concurrency  | Shared coordination point       | Requires thread safety handling  |

---

## Common Implementations

### Eager Initialization
- Instance created at startup

Pros:
- Simple
Cons:
- Wastes resources if unused

---

### Lazy Initialization
- Created when first accessed

Pros:
- Efficient
Cons:
- Thread safety complexity

---

### Thread-safe Singleton
- Uses locking or language primitives

---

## Failure Modes / Pitfalls

### 1. Hidden Global State
- Becomes implicit dependency across system
- Hard to trace side effects

---

### 2. Concurrency Issues
- Race conditions during initialization (if not protected)

---

### 3. Testing Difficulty
- Hard to isolate tests
- State persists between tests

---

### 4. Misuse as “Global Variable”
- Often abused as a shortcut for architecture design

---

## Common Misconceptions

### “Singleton means only one instance in the whole system”
❌ Incorrect  
✔ Only within a single process/runtime

---

### “Singleton makes system stateless/stateful”
❌ Incorrect  
✔ It is orthogonal to system architecture

---

### “Singleton is always good practice”
❌ Incorrect  
✔ Often replaced by dependency injection in modern systems

---

## Real-world usage

- Logging frameworks
- Configuration loaders
- Metrics collectors
- Thread pools (conceptually single manager)

---

## Better Modern Alternatives

- Dependency Injection (preferred in large systems)
- Explicit lifecycle management
- Container-managed singletons (framework-level)

---

## Interview Framing

1. Define singleton (single instance per process)
2. Explain use case (shared resource)
3. Mention trade-offs (global state issues)
4. Highlight concurrency concerns
5. Mention DI as modern alternative

---

## Related

- [[Stateful vs Stateless Systems]]
- [[Dependency Injection]]
- [[Concurrency Control]]