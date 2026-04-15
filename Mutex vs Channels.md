# Mutex vs Channel

---

## TL;DR
- **Mutex** → shared memory + explicit locking (protect critical section)
- **Channel** → message passing + ownership transfer (coordinate via communication)

Rule of thumb:
- Shared state → Mutex
- Communication / coordination → Channel

---

## Mental Model

### Mutex (Shared Memory Model)
“Everyone fights over the same memory, so we lock access”

- Threads/goroutines share memory
- Only one holder enters critical section at a time
- Explicit lock/unlock required

Think:
- “Protect this variable”

---

### Channel (Message Passing Model)
“Don’t share memory — communicate instead”

- Goroutines send messages to each other
- Data is passed, not shared
- Synchronization happens implicitly via send/receive

Think:
- “Send data ownership to someone else”

---

## Core Differences

| Aspect | Mutex | Channel |
|------|------|------|
| Concurrency model | Shared memory | Message passing |
| Synchronization | Explicit locking | Implicit via communication |
| Data access | Shared variable | Passed messages |
| Risk | Deadlocks, race conditions | Deadlocks (blocking sends/receives) |
| Design style | Low-level control | Higher-level coordination |
| Ownership | Shared | Transferred |

---

## How It Works

### Mutex

Flow:
- Acquire lock
- Access shared state
- Release lock

Conceptually:
- Lock → critical section → unlock

Used for:
- Counters
- Shared maps
- Caches
- Any mutable shared state

---

### Channel

Flow:
- Sender sends value into channel
- Receiver reads value from channel

Conceptually:
- Producer → Channel → Consumer

Used for:
- Pipelines
- Worker pools
- Event-driven systems
- Coordination between goroutines

---

## Design Philosophy

### Mutex
- Focus: protecting data
- Control: programmer-managed
- Error-prone under complexity

---

### Channel
- Focus: communication
- Control: runtime-managed synchronization
- Encourages structured concurrency

---

## When to Use Mutex

Use Mutex when:
- Multiple goroutines modify shared state
- You need fine-grained performance control
- Simple critical section protection is enough

Examples:
- Incrementing counters
- Updating shared maps
- In-memory caches

---

## When to Use Channel

Use Channel when:
- You want goroutines to coordinate workflows
- Data flows through stages (pipelines)
- You want ownership transfer semantics
- You want cleaner concurrency design

Examples:
- Worker pools
- Producer-consumer systems
- Event pipelines

---

## Trade-offs

### Mutex
Pros:
- Fast
- Simple for small critical sections
- Low overhead

Cons:
- Easy to misuse
- Deadlocks possible
- Hard to scale reasoning

---

### Channel
Pros:
- Clear coordination model
- Encourages safe communication
- Good for pipelines

Cons:
- Can still deadlock
- Harder to debug flow issues
- Overkill for simple shared state

---

## Common Misconceptions

### “Channels are always better than mutexes”
❌ Incorrect

- Mutex is simpler and faster for shared state
- Channels are not a universal replacement

---

### “Mutex avoids all concurrency issues”
❌ Incorrect

- Still possible:
  - deadlocks
  - contention bottlenecks

---

### “Channels don’t have locks”
❌ Incorrect

- Channels internally use synchronization mechanisms
- They just abstract it away

---

## Real-World Pattern

### Hybrid usage (very common)

- Mutex for shared state protection
- Channels for coordination between workers

Example:
- Worker pool:
  - channel distributes jobs
  - mutex protects shared metrics

---

## Failure Modes

### Mutex
- Deadlocks (lock ordering issues)
- Contention bottlenecks
- Race conditions if misused

---

### Channel
- Blocking sends/receives causing deadlocks
- Unbuffered channel bottlenecks
- Complex flow debugging

---

## Interview Framing

1. Clarify problem type:
   - shared state → mutex
   - coordination → channel
2. Choose model:
   - low-level control → mutex
   - structured concurrency → channel
3. Mention trade-offs
4. Highlight deadlock risks in both

---

## Rule of Thumb

- Mutex → “protect memory”
- Channel → “coordinate execution”

---

## Related

- [[Concurrency]]
- [[Goroutines]]
- [[Deadlocks]]
- [[Producer Consumer Pattern]]
- [[Thread Safety]]