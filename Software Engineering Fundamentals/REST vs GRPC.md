## TL;DR
- REST → HTTP, JSON, resource-based, best for public APIs
- gRPC → HTTP/2, protobuf, RPC-based, best for internal services

Rule of thumb:
- External APIs → REST
- Internal microservices → gRPC

---

## Mental Model

### REST
Expose resources over HTTP.

- GET /users/123
- POST /orders

Stateless request/response model.

---

### gRPC
Call remote functions.

- GetUser(UserRequest) → User

Strongly typed service contracts.

---

## Core Differences

| Aspect | REST | gRPC |
|------|------|------|
| Model | Resource-based | RPC-based |
| Transport | HTTP/1.1 | HTTP/2 |
| Payload | JSON | Protobuf (binary) |
| Performance | Moderate | High |
| Contract | Weak | Strong |
| Streaming | Limited | Native |
| Debugging | Easy | Harder |

---

## How It Works

### REST Example

Request:
GET /users/123

Response:
{
  "id": 123,
  "name": "Alice"
}

---

### gRPC Example

Proto definition:
service UserService {
  rpc GetUser (GetUserRequest) returns (User);
}

Client call:
userService.GetUser({ id: 123 })

---

## When to Use REST

- Public APIs
- Web/mobile clients
- Third-party integrations
- Simple CRUD systems
- Easy debugging needed

---

## When to Use gRPC

- Internal microservices
- High performance systems
- Low latency requirements
- Streaming required
- Strong API contracts needed

---

## Streaming Support

REST:
- Polling
- WebSockets
- SSE

gRPC:
- Unary
- Server streaming
- Client streaming
- Bidirectional streaming

---

## Trade-offs

### REST
Pros:
- Simple
- Universal
- Easy debugging

Cons:
- Larger payloads
- No strict contracts
- Can become inconsistent

---

### gRPC
Pros:
- Fast
- Efficient
- Strong contracts
- Built-in streaming

Cons:
- Harder debugging
- Not browser-native
- More complex setup

---

## Common Misconceptions

REST is always slower:
- False — depends on implementation

gRPC replaces REST:
- False — most systems use both

REST is just JSON:
- False — REST is an architectural style

gRPC is Go-only:
- False — multi-language support

---

## Real-World Pattern

Client → REST API Gateway → Internal gRPC services

---

## Failure Modes

REST:
- Too many API calls (chatty systems)
- Inconsistent design

gRPC:
- Hard debugging
- HTTP/2 dependency
- Browser limitations

---

## Interview Framing

1. Identify consumers (external vs internal)
2. Evaluate performance needs
3. Choose:
   - REST → simplicity
   - gRPC → efficiency
4. Mention hybrid architecture if needed

---

## Rule of Thumb

REST → optimize for usability  
gRPC → optimize for performance

---

## Related

- [[Distributed Systems]]
- [[API Design]]
- [[Microservices]]
- [[HTTP/2]]
- [[Protocol Buffers]]
- [[Service Discovery]]
- [[API Gateway]]