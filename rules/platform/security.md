# Security Rules

Practical security guidelines for backend services and APIs. Focused on preventing the most common and impactful vulnerabilities rather than exhaustive compliance checklists.

## Context

*Applies to:* Backend APIs, web services, any system handling user data or authentication
*Level:* Critical/Operational - security rules are non-negotiable
*Audience:* Backend developers, architects, security reviewers

## Core Principles

1. *Defence in Depth:* No single control is sufficient; layer security across transport, authentication, authorisation, and input handling
2. *Least Privilege:* Every component, user, and process has only the permissions it needs and nothing more
3. *Fail Securely:* On error or ambiguity, default to denying access — never granting it
4. *Never Trust Input:* All data from clients or external systems must be validated and sanitised before use
5. *Secrets Are Not Code:* Credentials, keys, and tokens must never appear in source code or logs

## Rules

### Must Have (Critical)

- *RULE-001:* Never store secrets, credentials, or API keys in source code or version control; use environment variables or a secrets manager (Vault, AWS Secrets Manager, Azure Key Vault)
- *RULE-002:* Enforce HTTPS everywhere; reject or redirect HTTP requests; enable HSTS in production
- *RULE-003:* Authenticate every non-public endpoint using a proven standard (OAuth 2.0, JWT with short expiry, OpenID Connect)
- *RULE-004:* Authorise every request explicitly — authentication proves identity, authorisation proves permission; they are not the same check
- *RULE-005:* Validate and sanitise all input at system boundaries (length, type, format, range); reject invalid input with 400 or 422
- *RULE-006:* Never log sensitive data: passwords, tokens, PII, payment card numbers, or cryptographic keys

### Should Have (Important)

- *RULE-101:* Set security response headers: `Content-Security-Policy`, `X-Content-Type-Options: nosniff`, `X-Frame-Options: DENY`, `Referrer-Policy`
- *RULE-102:* Implement rate limiting and brute-force protection on authentication and sensitive write endpoints
- *RULE-103:* Use parameterised queries or an ORM for all database access; never concatenate user input into SQL or shell commands
- *RULE-104:* Hash passwords using a modern adaptive algorithm (bcrypt, Argon2, scrypt); never store plaintext or use MD5/SHA1
- *RULE-105:* Configure CORS explicitly; never use wildcard `*` origins in production for credentialed requests
- *RULE-106:* Rotate secrets on a defined schedule; revoke and rotate immediately on suspected compromise

### Could Have (Preferred)

- *RULE-201:* Produce audit logs for security-relevant events: login, logout, permission changes, administrative actions, data export
- *RULE-202:* Use automated dependency scanning (Dependabot, Snyk, OWASP Dependency-Check) to surface known CVEs in third-party packages
- *RULE-203:* Apply the OWASP Top 10 as a design review checklist; conduct threat modelling on new features that handle sensitive data

## Patterns & Anti-Patterns

### ✅ Do This

```csharp
// Secrets from environment, not source code
var connectionString = Environment.GetEnvironmentVariable("DB_CONNECTION_STRING");

// ORM handles parameterisation, no injection risk
var user = await db.Users
    .Where(u => u.Email == email)
    .FirstOrDefaultAsync();

// Log the event, not the secret
_logger.LogInformation("Authentication succeeded for user {UserId}", userId);
```

### ❌ Don't Do This

```csharp
// Hardcoded secret in source
var connectionString = "Server=prod-db;Password=P@ssw0rd123!";

// SQL injection vulnerability
var sql = $"SELECT * FROM Users WHERE email = '{email}'";

// Logging sensitive data
_logger.LogDebug("Login: email={Email} password={Password}", email, password);
```

## Decision Framework

*When rules conflict:*
1. Security over convenience — an inconvenient security control is preferable to a breach
2. When performance and security conflict, measure the actual overhead before relaxing any security control
3. Treat input from internal services with the same suspicion as input from public clients

*When facing edge cases:*
- Internal-only APIs still require authentication; "internal network" is not a security boundary
- Test environments must use test credentials, never production secrets
- Audit logs must be append-only and protected from modification by application-layer code

## Exceptions & Waivers

*Valid reasons for exceptions:*
- Public read-only endpoints do not require authentication (but still require input validation and rate limiting)
- Legacy system integration with a fixed, documented security posture and explicit compensating controls
- Performance-critical paths where a specific control has a measured unacceptable overhead

*Process for exceptions:*
1. Document the security risk and the compensating controls in place
2. Obtain sign-off from the tech lead or nominated security champion
3. Schedule a mandatory review date; exceptions must not become permanent by default

## Quality Gates

- *Automated checks:* SAST scanning in CI, dependency vulnerability scanning, secret detection in git history (git-secrets, trufflehog), DAST against staging environments
- *Code review focus:* No hardcoded secrets, input validated at all entry points, all SQL parameterised, logging checked for sensitive data, auth/authz applied to every non-public endpoint
- *Testing requirements:* Security tests covering authentication bypass attempts, authorisation failures (access another user's resource), and injection with malformed input

## Related Rules

- rules/api-design.md - Input validation and error response design at API boundaries
- rules/platform/dotnet.rules.md - .NET-specific security implementation patterns
- rules/testing-principles.md - Security scenario test requirements

---

## TL;DR

*Key Principles:*
- Secrets never in code; validate all input; authenticate and authorise every request
- Log events, not secrets — sensitive data must never appear in logs or error messages
- Defence in depth: HTTPS, security headers, rate limiting, and parameterised queries together

*Critical Rules:*
- Must never store secrets in source code or version control (RULE-001)
- Must enforce HTTPS and enable HSTS in production (RULE-002)
- Must authenticate all non-public endpoints (RULE-003)
- Must authorise every request — authentication is not authorisation (RULE-004)
- Must validate and sanitise all input at system boundaries (RULE-005)
- Must never log passwords, tokens, or PII (RULE-006)

*Quick Decision Guide:*
When in doubt: Deny by default. An overly restrictive control can be relaxed deliberately; a data breach cannot be undone.
