# Testing Principles

Practical guidelines for writing tests that provide confidence, enable refactoring, and serve as living documentation. Tests that are hard to write indicate design problems; tests that are hard to read indicate maintenance problems.

## Context

*Applies to:* All software projects with automated test suites
*Level:* Tactical/Operational - guides daily test writing and test suite design
*Audience:* All developers writing or reviewing tests

## Core Principles

1. *Tests as Documentation:* Tests should communicate intent and describe expected behaviour in plain terms
2. *Test Behaviour, Not Implementation:* Test what code does, not how it does it; tests should survive refactoring
3. *Fast Feedback:* Tests must run quickly; slow tests are ignored or skipped
4. *Isolation:* Tests should be independent; running in any order must produce the same result
5. *Test the Right Thing:* Unit tests for logic, integration tests for wiring, end-to-end tests for critical paths only

## Rules

### Must Have (Critical)

- *RULE-001:* Each test must have a single, clear assertion about one specific behaviour
- *RULE-002:* Tests must be independent and not rely on execution order or shared mutable state
- *RULE-003:* Use descriptive test names that state the scenario and expected outcome (e.g., `returns_404_when_user_not_found`)
- *RULE-004:* Follow Arrange-Act-Assert (or Given-When-Then) structure; keep each section visually distinct
- *RULE-005:* Tests must pass consistently; flaky tests must be fixed or deleted immediately

### Should Have (Important)

- *RULE-101:* Follow the test pyramid: more unit tests than integration tests, more integration tests than end-to-end tests
- *RULE-102:* Use test doubles (stubs, mocks, fakes) only for external dependencies and side effects; avoid mocking domain logic
- *RULE-103:* Keep test setup minimal; long `beforeEach` blocks hide what matters and increase cognitive load
- *RULE-104:* Test edge cases and failure paths, not just the happy path
- *RULE-105:* Treat test code with the same quality standards as production code — naming, duplication, complexity

### Could Have (Preferred)

- *RULE-201:* Use parameterised/data-driven tests to cover multiple scenarios without duplication
- *RULE-202:* Write tests before or alongside implementation to drive design (TDD where practical)
- *RULE-203:* Apply mutation testing on critical domain logic to verify test quality, not just coverage percentage

## Patterns & Anti-Patterns

### ✅ Do This

```typescript
describe('OrderService', () => {
  it('should reject order when stock is insufficient', () => {
    // Arrange
    const inventory = stubInventory({ available: 2 });
    const service = new OrderService(inventory);

    // Act
    const result = service.place({ quantity: 5 });

    // Assert
    expect(result).toEqual(Err('insufficient_stock'));
  });
});
```

### ❌ Don't Do This

```typescript
// Unclear name, tests implementation detail, mocks domain logic
it('test1', () => {
  const mockCalc = jest.fn().mockReturnValue(42);
  service.calc = mockCalc;
  service.run();
  expect(mockCalc).toHaveBeenCalled(); // tests that a method was called, not what happened
});
```

## Decision Framework

*When rules conflict:*
1. Confidence over coverage — a meaningful test on a critical path beats 100% line coverage of trivial getters
2. Readability over brevity — a slightly longer test that clearly communicates intent is better than a terse one
3. Delete a flaky test before disabling it; a disabled test rots

*When facing edge cases:*
- If a test is hard to write, the code under test probably has too many dependencies — refactor the code first
- If tests break on every refactor, they are testing implementation details — refactor the tests
- Contract tests are preferable to extensive mocking of external services

## Exceptions & Waivers

*Valid reasons for exceptions:*
- Exploratory/spike code with an explicit time-box and delete date
- Performance or load tests that by nature cannot be fast
- Legacy code without test hooks — add characterisation tests first, refactor second

*Process for exceptions:*
1. Document why the test cannot follow the rule
2. Create a follow-up task to address the constraint
3. Do not merge production logic without at least a characterisation test

## Quality Gates

- *Automated checks:* CI must run all tests on every push; fail the build on test failure or agreed coverage drop
- *Code review focus:* Test names describe behaviour, no shared mutable state, assertions are meaningful, test doubles used appropriately
- *Testing requirements:* New behaviour requires accompanying tests; bug fixes require a regression test that reproduces the bug

## Related Rules

- rules/solid-principles.md - SOLID design makes code testable
- rules/clean-code.md - Clean code principles apply equally to test code
- rules/task-execution.md - Tests must pass before approving a TODO item

---

## TL;DR

*Key Principles:*
- Test behaviour, not implementation — tests should survive refactoring
- One clear assertion per test, named to describe scenario and outcome
- Fast, independent, consistent — flaky or slow tests are a liability

*Critical Rules:*
- Must have a single clear assertion per test (RULE-001)
- Must be independent with no shared mutable state (RULE-002)
- Must use descriptive names stating scenario and expected outcome (RULE-003)
- Must follow Arrange-Act-Assert structure (RULE-004)
- Must fix or delete flaky tests immediately (RULE-005)

*Quick Decision Guide:*
When in doubt: If a test breaks when you refactor without changing behaviour, it is testing implementation. If a test is hard to write, the code has a design problem.
