# API Design Rules

Guidelines for designing HTTP APIs that are predictable, consistent, and easy for consumers to integrate with. A well-designed API is self-describing, handles errors gracefully, and evolves without breaking clients.

## Context

*Applies to:* HTTP/REST APIs, public and internal service interfaces
*Level:* Strategic/Tactical - guides API contract decisions and interface design
*Audience:* Backend developers, architects, API designers

## Core Principles

1. *Consumer First:* Design APIs from the perspective of the consumer, not the implementation
2. *Consistency:* Predictable patterns across endpoints reduce integration burden
3. *Explicit Contracts:* All inputs, outputs, and errors must be documented and versioned
4. *Fail Informatively:* Errors should tell consumers what went wrong and how to fix it
5. *Evolve Safely:* Never make breaking changes without a versioned migration path

## Rules

### Must Have (Critical)

- *RULE-001:* Use nouns for resource paths, not verbs — `/orders/{id}` not `/getOrder`
- *RULE-002:* Use correct HTTP methods: GET (read, idempotent), POST (create/action), PUT (full replace, idempotent), PATCH (partial update), DELETE (remove, idempotent)
- *RULE-003:* Return appropriate HTTP status codes: 200 OK, 201 Created, 204 No Content, 400 Bad Request, 401 Unauthorized, 403 Forbidden, 404 Not Found, 409 Conflict, 422 Unprocessable Entity, 500 Internal Server Error
- *RULE-004:* Return a consistent, structured error response body: machine-readable code, human-readable message, and correlation ID
- *RULE-005:* Version all APIs explicitly from the first release — URL path prefix (`/v1/`) is preferred for discoverability
- *RULE-006:* Paginate all collection endpoints; never return unbounded lists

### Should Have (Important)

- *RULE-101:* Use plural nouns for collections (`/users`, `/orders`) and singular identifiers for resources (`/orders/{id}`)
- *RULE-102:* Support filtering and sorting on collection endpoints via query parameters; document available parameters
- *RULE-103:* Include a correlation/request ID in all responses to support distributed tracing
- *RULE-104:* Document all endpoints with request/response schemas and example payloads (OpenAPI/Swagger)
- *RULE-105:* Return a `Location` header on 201 Created responses pointing to the newly created resource
- *RULE-106:* Use ISO 8601 for all date/time fields (`2024-03-15T09:30:00Z`); never use epoch integers in responses

### Could Have (Preferred)

- *RULE-201:* Support partial updates with PATCH using JSON Merge Patch (RFC 7396)
- *RULE-202:* Provide a health check endpoint (`/health`) returning service status and dependency states
- *RULE-203:* Include `ETag` and support conditional requests (`If-None-Match`) on frequently polled resources

## Patterns & Anti-Patterns

### ✅ Do This

```
GET    /api/v1/orders?status=pending&page=1&size=20  → 200 OK, paginated body
POST   /api/v1/orders                                → 201 Created, Location: /api/v1/orders/42
GET    /api/v1/orders/42                             → 200 OK
PATCH  /api/v1/orders/42                             → 200 OK
DELETE /api/v1/orders/42                             → 204 No Content

// Consistent error shape
{
  "code": "ORDER_NOT_FOUND",
  "message": "No order exists with id 42",
  "correlationId": "req-abc-123"
}
```

### ❌ Don't Do This

```
GET  /api/getOrders          // verb in path
POST /api/deleteOrder/42     // wrong method for deletion
GET  /api/orders             // returns all 50,000 records unbounded
POST /api/processOrder       // RPC-style, not resource-oriented

// Vague, inconsistent error
{ "error": true, "msg": "something went wrong" }
```

## Decision Framework

*When rules conflict:*
1. Consumer clarity wins — if a deviation makes the API harder to understand, don't do it
2. Consistency over purity — a slightly impure but consistent pattern is better than a correct one-off
3. Prefer 422 for input validation errors; prefer 409 for business rule conflicts (e.g., duplicate, wrong state)

*When facing edge cases:*
- For actions that don't map to CRUD (e.g., submit, approve, cancel), use a sub-resource noun: `POST /orders/42/cancellation`
- For bulk operations, use a dedicated endpoint (`POST /orders/batch`) rather than abusing single-resource endpoints
- Breaking changes require a new version; additive changes (new optional fields) do not

## Exceptions & Waivers

*Valid reasons for exceptions:*
- Legacy system integration where the API contract is externally mandated
- File upload/download endpoints where REST conventions for body shape don't apply
- Internal tooling where full REST conformance adds overhead without consumer benefit

*Process for exceptions:*
1. Document the deviation and rationale in API documentation
2. Ensure all consumers are aware of the inconsistency
3. Plan migration to a consistent pattern in the next major version

## Quality Gates

- *Automated checks:* OpenAPI linting (Spectral), schema validation in CI, contract tests against consumer expectations
- *Code review focus:* Correct HTTP verbs and status codes, consistent error shapes, pagination on collections, versioning present, no unbounded queries
- *Testing requirements:* Contract tests for all public endpoints; integration tests covering error paths, status codes, and pagination boundaries

## Related Rules

- rules/solid-principles.md - Interface Segregation applies to API surface design
- rules/platform/security.md - Authentication, authorisation, and input validation at API boundaries
- rules/testing-principles.md - Contract and integration testing requirements

---

## TL;DR

*Key Principles:*
- Design for the consumer, not the implementation — predictability reduces integration cost
- Consistency across endpoints is more valuable than perfection on any single one
- Version from day one; never make silent breaking changes

*Critical Rules:*
- Must use nouns for resource paths and correct HTTP methods (RULE-001, RULE-002)
- Must return correct HTTP status codes (RULE-003)
- Must return consistent, structured error responses with a correlation ID (RULE-004)
- Must version all APIs from first release (RULE-005)
- Must paginate all collection endpoints (RULE-006)

*Quick Decision Guide:*
When in doubt: Would a developer with no internal knowledge understand this endpoint from its URL, verb, and status code alone? If not, redesign it.
