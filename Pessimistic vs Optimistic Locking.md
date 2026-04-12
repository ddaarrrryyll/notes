## TL;DR
- Use **Pessimistic Locking** when conflicts are **likely** and cost of retry is high
- Use **Optimistic Locking** when conflicts are **rare** and throughput matters

Rule of thumb:
- High contention → Pessimistic
- Low contention → Optimistic
## Mental Model

### Pessimistic Locking
"Assume conflict will happen → prevent it upfront"
- Lock resource before access
- Others must wait
### Optimistic Locking
"Assume conflict is rare → detect and resolve later"
- No lock initially
- Validate before commit (version/timestamp)
## How It Works

### Pessimistic Locking
- Acquire lock (row/table/distributed)
- Perform operation
- Release lock

Common primitives:
- SQL: `SELECT ... FOR UPDATE`
- Mutex / distributed locks (e.g. Redis)
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
## Trade-offs

| Aspect     | Pessimistic Locking     | Optimistic Locking         |
| ---------- | ----------------------- | -------------------------- |
| Contention | Handles high contention | Poor under high contention |
| Latency    | Higher (blocking)       | Lower (no blocking)        |
| Throughput | Lower                   | Higher                     |
| Deadlocks  | Possible                | None                       |
| Complexity | Simpler mentally        | Requires retry logic       |
## When to Use
### Use Pessimistic Locking when:
- Financial transactions (no double spend)
- Inventory decrement (high contention SKUs)
- Critical invariants must not break
---
### Use Optimistic Locking when:
- User profile updates
- Low contention systems
- Read-heavy workloads
- High scalability requirements

## Pitfalls

### Pessimistic Locking
- Deadlocks
- Lock contention → system slowdown
- Poor scalability
---
### Optimistic Locking
- Retry storms under high contention
- Starvation (one request keeps failing)
- Requires idempotency awareness
## Real-World Examples
- MySQL InnoDB → Pessimistic (row locks)
- JPA/Hibernate → Optimistic (@Version)
- Redis locks → Pessimistic (distributed)
- DynamoDB → Optimistic (conditional writes)

## Related Topics
- [[Transactions]]
- [[Isolation Levels]]
- [[Deadlocks]]
- [[Distributed Locks]]
- [[Idempotency]]
- [[Retry Strategies]]