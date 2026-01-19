# SOLID Principles

Object-oriented design principles that promote maintainable, scalable, and flexible software architecture. These five principles guide class design and dependencies to create systems that are easier to understand, modify, and extend.

## Context

Provide guidelines for object-oriented design that reduce coupling, increase cohesion, and enable sustainable codebases.

*Applies to:* Object-oriented programming in languages like Java, C#, TypeScript, Python, C++
*Level:* Tactical/Operational - guides day-to-day class and module design
*Audience:* Developers and Architects designing object-oriented systems

## Core Principles

1. *Single Responsibility:* A class should have one, and only one, reason to change. Each class should focus on a single concern or responsibility.
2. *Open for Extension, Closed for Modification:* Software entities should be open for extension but closed for modification. New behavior should be added through extension, not by changing existing code.
3. *Substitutability:* Derived classes must be substitutable for their base classes without altering program correctness. Subtypes must honor the contract of their parent type.
4. *Interface Segregation:* Clients should not be forced to depend on interfaces they don't use. Many specific interfaces are better than one general-purpose interface.
5. *Dependency Inversion:* High-level modules should not depend on low-level modules. Both should depend on abstractions. Abstractions should not depend on details.

## Rules

### Must Have (Critical)

- *RULE-001:* Each class must have a single, well-defined responsibility. If you cannot describe a class's purpose in one sentence without using "and," it violates SRP.
- *RULE-002:* Always depend on abstractions (interfaces/abstract classes) rather than concrete implementations for cross-module dependencies.
- *RULE-003:* Derived classes must be fully substitutable for their base classes. Override methods must maintain preconditions, postconditions, and invariants of the parent.
- *RULE-004:* Never force implementing classes to depend on methods they don't use. Split large interfaces into focused, cohesive ones.
- *RULE-005:* Never modify existing, working classes to add new behavior. Extend functionality through inheritance, composition, or strategy patterns.

### Should Have (Important)

- *RULE-101:* Limit class dependencies to 5 or fewer. More dependencies often indicate violation of SRP or high coupling.
- *RULE-102:* Use dependency injection to provide dependencies rather than instantiating them within classes. This enables loose coupling and testability.
- *RULE-103:* Design interfaces based on client needs, not implementation capabilities. Start from how the client will use it.
- *RULE-104:* Prefer composition over inheritance when extending behavior. Inheritance should model "is-a" relationships, not just code reuse.
- *RULE-105:* Keep inheritance hierarchies shallow (3 levels or fewer). Deep hierarchies make substitutability harder to maintain.
- *RULE-106:* When opening a class for extension, use protected methods, abstract methods, or strategy patterns rather than exposing implementation details.

### Could Have (Preferred)

- *RULE-201:* Name classes and interfaces to reflect their single responsibility clearly (e.g., `UserAuthenticator`, not `UserManager`).
- *RULE-202:* Use role interfaces (e.g., `Readable`, `Closeable`) to segregate different aspects of behavior.
- *RULE-203:* Apply the "extract and override" pattern for testability rather than complex mocking frameworks.
- *RULE-204:* Document base class contracts explicitly when inheritance is used, specifying what derived classes must honor.

## Patterns & Anti-Patterns

### ✅ Do This

```typescript
// SRP: Separate concerns
class UserAuthenticator {
  authenticate(credentials: Credentials): User { }
}
class UserRepository {
  save(user: User): void { }
  findById(id: string): User { }
}

// OCP: Extend through abstraction
interface PaymentProcessor {
  process(amount: number): Receipt;
}
class CreditCardProcessor implements PaymentProcessor { }
class PayPalProcessor implements PaymentProcessor { }

// DIP: Depend on abstractions
class OrderService {
  constructor(private paymentProcessor: PaymentProcessor) { }
}

// ISP: Focused interfaces
interface Readable {
  read(): string;
}
interface Writable {
  write(data: string): void;
}
```

### ❌ Don't Do This

```typescript
// Violates SRP: Too many responsibilities
class UserManager {
  authenticate(credentials: Credentials): User { }
  save(user: User): void { }
  sendEmail(user: User, message: string): void { }
  generateReport(userId: string): Report { }
}

// Violates OCP: Must modify class to add payment types
class PaymentService {
  process(type: string, amount: number) {
    if (type === 'credit') { /* ... */ }
    else if (type === 'paypal') { /* ... */ }
  }
}

// Violates DIP: Depends on concrete implementation
class OrderService {
  private processor = new CreditCardProcessor();
}

// Violates ISP: Forces unused methods
interface Worker {
  work(): void;
  eat(): void;
  sleep(): void;
}
class Robot implements Worker {
  work() { }
  eat() { throw new Error('Robots don't eat'); }
  sleep() { throw new Error('Robots don't sleep'); }
}
```

## Decision Framework

*When rules conflict:*
1. Prioritize SRP and DIP first - they provide the foundation for the others
2. Apply OCP when you have concrete evidence of variation (not speculative flexibility)
3. Use LSP as a validation check for inheritance hierarchies
4. Apply ISP when interfaces grow beyond 3-5 methods or clients use less than 80% of methods

*When facing edge cases:*
- Premature abstraction is as harmful as none. Apply SOLID when you have real variation, not anticipated
- Utility classes with static methods can violate SOLID but may be acceptable for pure functions
- Framework/library integration may require some concrete dependencies - isolate these at boundaries

## Exceptions & Waivers

*Valid reasons for exceptions:*
- Performance-critical code where abstraction overhead is measured and significant
- Framework constraints that mandate specific inheritance patterns
- Prototyping/proof-of-concept code (must be refactored before production)
- Simple data structures or value objects with minimal behavior

*Process for exceptions:*
1. Document the specific SOLID principle being violated and why
2. Add technical debt ticket to revisit the design
3. Isolate the violation to minimize its impact on surrounding code

## Quality Gates

- *Automated checks:* Use static analysis tools to detect large classes (>300 lines often indicates SRP violation), high coupling (>7 dependencies), deep inheritance (>3 levels)
- *Code review focus:* Check that new classes have single responsibility, changes extend rather than modify, abstractions are injected, interfaces are client-focused
- *Testing requirements:* Classes following SOLID should be easily unit testable without extensive mocking. Difficulty in testing often indicates violations

## Related Rules

- rules/testing-principles.md - SOLID design enables effective unit testing
- rules/api-design.md - Interface segregation applies to public APIs
- rules/refactoring-guidelines.md - Refactoring toward SOLID principles

## References

- [Clean Architecture by Robert C. Martin](https://www.amazon.com/Clean-Architecture-Craftsmans-Software-Structure/dp/0134494164) - Comprehensive coverage of SOLID
- [SOLID Principles on Wikipedia](https://en.wikipedia.org/wiki/SOLID) - Quick reference
- [Design Principles and Design Patterns by Robert C. Martin](https://web.archive.org/web/20150906155800/http://www.objectmentor.com/resources/articles/Principles_and_Patterns.pdf) - Original paper

---

## TL;DR

*Key Principles:*
- One class, one responsibility - each class should have exactly one reason to change
- Depend on abstractions, not concretions - use interfaces and inject dependencies
- Design for extension, not modification - add new behavior through new code, not changed code

*Critical Rules:*
- Must have single responsibility per class (RULE-001)
- Must depend on abstractions for cross-module dependencies (RULE-002)
- Must ensure derived classes are substitutable for base classes (RULE-003)
- Must not force clients to depend on unused interface methods (RULE-004)
- Must extend behavior, not modify existing working code (RULE-005)

*Quick Decision Guide:*
When in doubt: If a change requires modifying multiple classes or a class for different reasons, your design likely violates SOLID. Refactor to extract responsibilities and depend on abstractions.
