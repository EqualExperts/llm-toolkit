# .NET Backend Development Rules

Concise, practical rules for building ASP.NET Core and Entity Framework Core backend APIs in C# that are secure, maintainable, and performant.

## Context

Applies to: ASP.NET Core Web APIs, backend services, microservices using C# and EF Core
Level: Operational/Tactical
Audience: Backend developers, tech leads, reviewers

## Core Principles

1. Coding Conventions First: Prefer idiomatic C# and ASP.NET Core conventions for clarity and consistency.
2. Secure by Default: Enforce authentication, authorization, HTTPS, and CORS.
3. Reliability and Observability: Validate inputs, handle errors globally, and log meaningfully.
4. Performance Matters: Use async I/O, efficient EF Core queries, caching, and pagination.
5. RESTful, Versioned APIs: Predictable resource design with explicit versioning and documentation.
6. Dependency Inversion: Favor DI for testability and loose coupling.
7. Test What You Ship: Unit and integration tests are standard, not optional.

## Rules

### Must Have (Critical)
- RULE-001: Follow .NET/C# coding conventions and ASP.NET Core best practices.
- RULE-002: Enforce HTTPS; configure appropriate CORS policies.
- RULE-003: Implement authentication and authorization for protected endpoints (e.g., JWT).
- RULE-004: Implement global exception handling; return consistent error payloads with correct HTTP status codes.
- RULE-005: Validate request models using Data Annotations or FluentValidation.
- RULE-006: Use Dependency Injection for all services and data access.
- RULE-007: Design RESTful endpoints; use attribute routing.
- RULE-008: Version the API explicitly.
- RULE-009: Use async/await for all I/O-bound operations; avoid blocking calls.
- RULE-010: Use EF Core effectively; avoid N\+1 queries (e.g., via `Include`, proper projections).
- RULE-011: Implement pagination for large result sets.
- RULE-012: Provide Swagger/OpenAPI docs; add XML comments for controllers and models.
- RULE-013: Log errors and key events using the built-in logging abstractions.
- RULE-014: Maintain unit tests and integration tests for API endpoints.

### Should Have (Important)
- RULE-101: Write concise, idiomatic C#; prefer expressive syntax (null\-conditional, string interpolation, pattern matching).
- RULE-102: Prefer LINQ and lambdas for collection operations.
- RULE-103: Use `var` when the type is obvious.
- RULE-104: Leverage built\-in ASP.NET Core middleware and features before custom implementations.
- RULE-105: Apply caching using `IMemoryCache` or distributed caches when appropriate.
- RULE-106: Use action filters for cross\-cutting concerns (e.g., validation, logging).
- RULE-107: Choose between repository pattern and direct EF Core usage based on complexity; avoid unnecessary abstraction.
- RULE-108: Use AutoMapper for object mapping when mapping is non\-trivial.
- RULE-109: Use xUnit/NUnit/MSTest for unit tests with Moq/NSubstitute for mocking.

### Could Have (Preferred)
- RULE-201: Use modern C# (10\+) features where they improve clarity and safety.
- RULE-202: Implement background processing with `IHostedService`/`BackgroundService` for non\-request work.
- RULE-203: Apply distributed caching and response caching for high\-traffic endpoints.

## Patterns & Anti-Patterns

### ✅ Do This
- Asynchronous, validated, paginated query with DI, logging, and proper status codes.

```csharp
[ApiController]
[Route("api/v{version:apiVersion}/orders")]
[ApiVersion("1.0")]
public class OrdersController : ControllerBase
{
    private readonly ILogger<OrdersController> _logger;
    private readonly AppDbContext _db;

    public OrdersController(ILogger<OrdersController> logger, AppDbContext db)
    {
        _logger = logger;
        _db = db;
    }

    [HttpGet]
    public async Task<ActionResult<PagedResult<OrderDto>>> GetAsync([FromQuery] int page = 1, [FromQuery] int size = 20)
    {
        if (page < 1 || size is < 1 or > 200) return BadRequest("Invalid pagination parameters.");

        var query = _db.Orders
            .AsNoTracking()
            .OrderBy(o => o.Id)
            .Select(o => new OrderDto(o.Id, o.Number, o.Total));

        var total = await query.CountAsync();
        var items = await query.Skip((page - 1) * size).Take(size).ToListAsync();

        return Ok(new PagedResult<OrderDto>(items, total, page, size));
    }
}
```

### ❌ Don't Do This
- Blocking calls, unversioned routes, inconsistent errors, and N\+1 queries.

```csharp
[Route("api/orders")]
public class OrdersController : Controller
{
    [HttpGet]
    public IActionResult Get()
    {
        // Blocks thread, no paging, potential N+1
        var orders = new AppDbContext().Orders.ToList(); // newing DbContext, no DI
        var result = new List<object>();
        foreach (var o in orders)
        {
            var lines = new AppDbContext().OrderLines.Where(l => l.OrderId == o.Id).ToList();
            result.Add(new { o.Id, o.Number, Lines = lines });
        }
        return Ok(result); // Always 200, no error shape
    }
}
```

## Decision Framework

When rules conflict:
1. Favor security and correctness over convenience or premature optimization.
2. Prefer framework conventions and official guidance over custom patterns.
3. Choose the simplest option that meets requirements; measure before optimizing.

When facing edge cases:
- Validate assumptions with metrics and logs.
- Make behavior explicit via configuration and documentation.
- Add tests to lock in intended behavior.

## Exceptions & Waivers

Valid reasons for exceptions:
- Interoperability with legacy systems or constraints from external contracts.
- Temporary prototypes/spikes with time\-boxed scope.
- Performance\-critical hotspots justified by measurements.

Process for exceptions:
1. Document the exception, rationale, and scope in ADR or README.
2. Obtain tech lead approval.
3. Set a review date and track remediation.

## Quality Gates

- Automated checks: `dotnet format`, analyzers (nullable enabled, IDE analyzers), StyleCop/Roslyn rules, API analyzers, security linters.
- Code review focus: RESTfulness, versioning, validation, error handling, async correctness, EF Core query efficiency, logging, tests.
- Testing requirements: Unit tests for services and controllers; integration tests for endpoints and data access; security and pagination covered.

## Related Rules

- `rules/platform/security.md` \- Security hardening and secrets handling
- `rules/platform/api-versioning.md` \- API versioning strategy
- `rules/platform/testing.md` \- Testing standards and tooling

## References

- https://learn.microsoft.com/aspnet/core \- ASP.NET Core docs
- https://learn.microsoft.com/ef/core \- Entity Framework Core docs
- https://learn.microsoft.com/dotnet/csharp/fundamentals/coding-style/coding-conventions \- C# coding conventions
- https://learn.microsoft.com/aspnet/core/fundamentals/logging \- Logging in ASP.NET Core
- https://learn.microsoft.com/aspnet/core/tutorials/getting-started-with-swashbuckle \- Swagger/OpenAPI with Swashbuckle

---

## TL;DR

- Key Principles: Follow conventions; secure by default; validate, log, and fail predictably; use async and efficient EF Core; DI everywhere; test and document.
- Critical Rules: Enforce HTTPS/auth; global exception handling and validation; RESTful, versioned endpoints; async I/O; avoid N\+1; paginate; Swagger with XML comments.
- Quick Decision Guide: Prefer built\-in features and official guidance; choose the simplest secure option; measure before optimizing.
