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