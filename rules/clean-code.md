# Clean Code Principles

A comprehensive guide to writing clean, maintainable, and readable code based on Robert C. Martin's "Clean Code" principles. These rules focus on creating code that clearly expresses intent, is easy to understand, modify, and maintain over time.

## Context

Provide guidelines for writing code that prioritizes readability, simplicity, and maintainability over cleverness or optimization.

*Applies to:* All software development projects, code reviews, refactoring efforts
*Level:* Tactical/Operational - direct application in daily coding practices
*Audience:* All developers, from junior to senior levels

## Core Principles

1. *Clarity over Cleverness:* Code should be written to be read and understood by humans first, machines second
2. *Express Intent:* Names, functions, and structure should clearly communicate what the code does and why
3. *Simplicity:* Prefer simple, straightforward solutions over complex, over-engineered ones
4. *Single Responsibility:* Each unit of code should have one clear purpose and reason to change
5. *Leave it Better:* Always improve code when you touch it, following the Boy Scout Rule

## Rules

### Must Have (Critical)

- *RULE-001:* Use meaningful, pronounceable names for variables, functions, classes, and modules
- *RULE-002:* Functions must be small (ideally <20 lines) and do one thing well
- *RULE-003:* Eliminate code duplication - follow the DRY principle strictly
- *RULE-004:* Remove commented-out code and dead code before committing
- *RULE-005:* Use consistent formatting and indentation throughout the codebase

### Should Have (Important)

- *RULE-101:* Functions should have no more than 3 parameters; use objects for more complex parameter sets
- *RULE-102:* Avoid deep nesting (>3 levels) - extract methods or use early returns
- *RULE-103:* Write self-documenting code that reduces need for comments
- *RULE-104:* Use intention-revealing names rather than mental mapping (avoid single-letter variables except for short loops)
- *RULE-105:* Keep classes small and focused on a single responsibility

### Could Have (Preferred)

- *RULE-201:* Prefer composition over inheritance
- *RULE-202:* Use descriptive error messages and proper exception handling
- *RULE-203:* Order functions by level of abstraction (high-level first, details later)
- *RULE-204:* Minimize dependencies between modules and classes
- *RULE-205:* Use consistent verb/noun naming conventions (getUser, calculateTotal, isValid)

## Patterns & Anti-Patterns

### ✅ Do This

```javascript
function calculateMonthlyPayment(principal, interestRate, termInYears) {
    const monthlyRate = interestRate / 12;
    const numberOfPayments = termInYears * 12;

    return (principal * monthlyRate) / (1 - Math.pow(1 + monthlyRate, -numberOfPayments));
}

const userAccount = {
    id: userId,
    email: userEmail,
    isActive: true
};
```

### ❌ Don't Do This

```javascript
function calc(p, r, t) {
    // Calculate monthly payment
    let mr = r / 12;
    let n = t * 12;
    return (p * mr) / (1 - Math.pow(1 + mr, -n));
}

// TODO: fix this later
// const oldUserData = getUser(id);
const u = { id: uid, e: ue, a: true };
```

## Decision Framework

*When rules conflict:*
1. Prioritize readability and maintainability over performance optimizations
2. Choose the solution that makes the code's intent most clear
3. Consider the team's collective understanding and skill level

*When facing edge cases:*
- Ask "Would a new team member understand this code in 6 months?"
- Prefer explicit over implicit behavior
- When in doubt, err on the side of being more verbose and clear

## Exceptions & Waivers

*Valid reasons for exceptions:*
- Performance-critical code paths with demonstrated bottlenecks (document trade-offs)
- Legacy system integration requiring specific patterns (temporary, with migration plan)
- Third-party API constraints that force specific implementations

*Process for exceptions:*
1. Document the exception, rationale, and performance/business justification
2. Add technical debt tracking item for future refactoring
3. Review exceptions quarterly to assess if constraints still apply

## Quality Gates

- *Automated checks:* Linting rules for naming conventions, function length, complexity metrics
- *Code review focus:* Readability, naming clarity, function size, duplication elimination
- *Testing requirements:* Unit tests must be as clean and readable as production code

## References

- [Clean Code by Robert C. Martin](https://www.amazon.co.uk/Clean-Code-Handbook-Software-Craftsmanship/dp/0132350882)
- [Refactoring by Martin Fowler](https://refactoring.com/)
- [The Pragmatic Programmer](https://pragprog.com/titles/tpp20/the-pragmatic-programmer-20th-anniversary-edition/)

---

## TL;DR

*Key Principles:*
- Write code for humans to read, not just machines to execute
- Use clear, descriptive names that express intent without requiring comments
- Keep functions small, focused, and doing only one thing well

*Critical Rules:*
- Must use meaningful names for all variables, functions, and classes
- Must keep functions under 20 lines and eliminate all code duplication
- Must remove dead code and maintain consistent formatting

*Quick Decision Guide:*
When in doubt: Choose the option that makes the code's intent most obvious to a future reader.
