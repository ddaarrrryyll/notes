## TL;DR  
- **Stateless** → no memory between requests → easy to scale  
- **Stateful** → retains session/state → harder to scale  
  
Rule of thumb:  
- Default to **stateless**  
- Introduce state only when necessary  
  
---  
  
## Mental Model  
  
### Stateless  
"Each request is independent"  
- No stored context in the service  
- All required data comes with the request or external store  
  
---  
  
### Stateful  
"System remembers previous interactions"  
- Stores session / connection state locally  
- Future requests depend on past interactions  
  
---  
  
## Key Insight  
  
Stateless systems still rely on state — it is just **externalized** (DB, cache, storage).  
  
---  
  
## How It Works  
  
### Stateless Architecture  
- Any instance can handle any request  
- State stored externally (DB / cache)  
  
Flow:  
Client → API → Database / Cache  
  
---  
  
### Stateful Architecture  
- Instance holds session/state in memory  
- Requests must go to the same instance  
  
Flow:  
Client → Same Server (stateful session)  
  
---  
  
## Common Confusion Traps

### 1. “Stateful means the system uses a database”
❌ Incorrect

- Almost all real systems use databases
- That does NOT make them stateful

✔ Correct meaning:
- Statefulness refers to **where request-dependent state is stored**
- DB is external shared state → does not make app stateful

---

### 2. “Preloading data makes a system stateful”
❌ Incorrect

- Preloaded config / cache / reference data is just optimization

✔ System is still stateless if:
- Any instance can serve any request
- Data is not instance-specific or session-dependent

---

### 3. “Stateless means no state exists anywhere”
❌ Incorrect

✔ Correct:
- Stateless means **no session/state is stored in the service instance**
- State is typically externalized (DB, cache, object storage)

---

### 4. “Stateful = request lifecycle holds state”
❌ Incorrect

- Request lifecycle is always temporary
- It is not what defines statefulness

✔ Correct:
- Statefulness depends on **server/instance memory persistence across requests**

---

### 5. “Sticky sessions make a system stateless”
❌ Incorrect

✔ Correct:
- Sticky sessions are a workaround for stateful behavior
- They actually **introduce stateful routing dependency**

---

### 6. “Stateless systems cannot have caching”
❌ Incorrect

✔ Correct:
- Stateless systems often rely heavily on caching
- The key is that cache is **shared or external (e.g. Redis/CDN)**, not instance-bound

---

### Mental Check

Ask:

> “If this instance dies, can another instance handle the next request without loss of correctness?”

- Yes → Stateless
- No → Stateful
---
## Trade-offs  
  
| Aspect          | Stateless                    | Stateful                      |  
|----------------|-----------------------------|-------------------------------|  
| Scalability    | Easy (horizontal)           | Hard (needs coordination)     |  
| Fault Tolerance| High                        | Lower (state loss risk)       |  
| Complexity     | Lower                       | Higher                        |  
| Performance    | Extra lookups               | Faster (local state)          |  
  
---  
  
## When to Use  
  
### Use Stateless  
- REST APIs  
- Microservices  
- High-scale systems  
- Load-balanced environments  
  
---  
  
### Use Stateful  
- WebSockets / real-time systems  
- Game servers  
- Streaming / long-lived connections  
- In-memory session-dependent logic  
  
---  
  
## Patterns  
  
### Stateless + External State (Most Common)  
- Store session in Redis / DB  
- Keep application layer stateless  
  
---  
  
### Sticky Sessions  
- Load balancer routes user to same server  
- Simplifies state, reduces scalability  
  
---  
  
### State Replication  
- Replicate state across nodes  
- Improves resilience, adds complexity  
  
---  
  
## Failure Modes  
  
### Stateless  
- Hidden coupling via database  
- Increased latency (extra network calls)  
  
---  
  
### Stateful  
- Node failure → state loss  
- Hard to autoscale  
- Requires failover / replication strategy  
  
---  
  
## Interview Framing  
  
1. Clarify:  
   - Do we need session continuity?  
2. Default to stateless design  
3. If state is required:  
   - Externalize it (preferred)  
   - Or justify stateful approach  
4. Discuss trade-offs  
5. Mention scaling implications  
  
---  
  
## Real-World Examples  
  
### Stateless  
- REST APIs (HTTP)  
- CDN / caching layers  
  
---  
  
### Stateful  
- WebSocket connections  
- Database connections  
- Multiplayer game servers  
  
---  
  
## Related  
  
- [[Load Balancing]]  
- [[Caching Strategies]]  
- [[Distributed Systems]]  
- [[Session Management]]  
- [[WebSockets]]  
- [[Consistency Models]]

---