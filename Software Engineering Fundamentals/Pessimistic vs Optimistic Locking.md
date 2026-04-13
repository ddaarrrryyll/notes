## TL;DR
- **Pessimistic** → lock first, prevent conflicts (high contention)
- **Optimistic** → detect conflicts, retry (low contention)

Rule of thumb:
- High contention / critical correctness → Pessimistic
- Low contention / high throughput → Optimistic

---

## Mental Model

### Pessimistic Locking
"Assume conflicts will happen"
- Acquire lock before modifying
- Others must wait

### Optimistic Locking
"Assume conflicts are rare"
- No lock initially
- Validate before commit

---

## How It Works

### Pessimistic Locking
- Lock resource (row / table / distributed)
- Perform operation
- Release lock

Examples:
- SQL: `SELECT ... FOR UPDATE`
- Mutex / distributed locks

---

### Optimistic Locking
- Read data with version
- Modify
- Write with version check

Example:
UPDATE table
SET value = X, version = version + 1
WHERE id = ? AND version = old_version

- If rows affected = 0 → retry

---

## Key Invariant

Optimistic locking guarantees:

> "Update only if nothing changed since I read it"

This is implemented via:
- **Compare-And-Swap (CAS)** semantics

---

## Trade-offs

| Aspect        | Pessimistic              | Optimistic               |
|--------------|--------------------------|--------------------------|
| Contention   | Handles high             | Poor under high          |
| Latency      | Higher (blocking)        | Lower (no blocking)      |
| Throughput   | Lower                    | Higher                   |
| Deadlocks    | Possible                 | None                     |
| Complexity   | Lower                    | Retry logic required     |

---

## When to Use

### Use Pessimistic Locking
- Financial transactions (no double spend)
- Inventory updates (hot items)
- Strong correctness required

---

### Use Optimistic Locking
- User profile updates
- Read-heavy systems
- Low contention workloads
- High scalability systems

---

## Failure Modes

### Pessimistic
- Deadlocks
- Lock contention → slowdown
- Poor scalability

---

### Optimistic
- Retry storms under contention
- Starvation (constant retries)
- Requires idempotency awareness

---

## Common Pitfall

Incorrect:
UPDATE ... WHERE version < new_version

Correct:
UPDATE ... WHERE version = old_version

Reason:
- Must ensure **no intermediate writes occurred**
- Enforces strict CAS semantics

---

## Real-World Mapping

- MySQL (InnoDB) → Pessimistic (row locks)
- Redis locks → Pessimistic (distributed)
- DynamoDB → Optimistic (conditional writes)
- JPA/Hibernate → Optimistic (`@Version`)

---

## Interview Framing

1. Clarify contention level
2. Identify correctness requirements
3. Choose:
   - High contention → pessimistic
   - Low contention → optimistic
4. Mention trade-offs
5. Call out failure modes

---

## Related

- [[Transactions]]
- [[Isolation Levels]]
- [[MVCC]]
- [[Distributed Locks]]
- [[Idempotency]]
- [[Retry Strategies]]