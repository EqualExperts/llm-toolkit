# TypeScript Development Rules

TypeScript coding standards and best practices for modern web development. These rules ensure type safety, maintainability, and consistency across TypeScript codebases.

## Context

*Applies to:* All TypeScript files (**.ts, **.tsx, **.d.ts) in web applications, libraries, and Node.js projects
*Level:* Tactical/Operational - day-to-day development practices
*Audience:* Developers working with TypeScript codebases

## Core Principles

1. **Type Safety First:** Leverage TypeScript's type system to catch errors at compile time rather than runtime
2. **Explicit Over Implicit:** Prefer explicit type annotations and return types for better code documentation and IDE support
3. **Consistency in Conventions:** Follow established naming patterns and organizational structures for predictable codebases

## Rules

### Must Have (Critical)
Non-negotiable rules that must always be followed. Violation of these rules should block progress.

- **TS-001:** Enable strict mode in tsconfig.json with all strict flags enabled
- **TS-002:** Never use `any` type - use `unknown` for truly unknown types or proper type definitions
- **TS-003:** Use explicit return types for all public functions and class methods
- **TS-004:** Implement proper null checking - avoid non-null assertion operator (!) unless absolutely necessary

### Should Have (Important)
Strong recommendations that should be followed unless there's a compelling reason not to.

- **TS-101:** Prefer interfaces over types for object definitions and extensible contracts
- **TS-102:** Use PascalCase for type names, interfaces, and classes; camelCase for variables and functions
- **TS-103:** Create custom error types for domain-specific errors instead of throwing generic Error objects
- **TS-104:** Use readonly for immutable properties and arrays to prevent accidental mutations
- **TS-105:** Leverage discriminated unions for type-safe state management and API responses

### Could Have (Preferred)
Best practices and preferences that improve quality but are not blocking.

- **TS-201:** Use descriptive boolean variable names with auxiliary verbs (isLoading, hasError, canEdit)
- **TS-202:** Prefix React component prop interfaces with 'Props' suffix (ButtonProps, HeaderProps)
- **TS-203:** Use barrel exports (index.ts) for clean module organization
- **TS-204:** Prefer async/await over Promise chains for better readability

## Patterns & Anti-Patterns

### ✅ Do This
```typescript
// Good: Explicit interface with proper naming
interface UserProps {
  readonly id: string;
  name: string;
  isActive: boolean;
}

// Good: Custom error type
class ValidationError extends Error {
  constructor(public field: string, message: string) {
    super(message);
    this.name = 'ValidationError';
  }
}

// Good: Explicit return type
function processUser(user: UserProps): Promise<ProcessedUser> {
  // implementation
}
```

### ❌ Don't Do This
```typescript
// Bad: Using any type
function processData(data: any): any {
  return data.someProperty;
}

// Bad: Implicit types and generic naming
function process(item) {
  return item.prop;
}

// Bad: Non-descriptive boolean naming
const flag = true;
const check = false;
```

## Decision Framework

*When rules conflict:*
1. Prioritize type safety over convenience
2. Choose explicit over implicit when both are valid
3. Follow the strictest applicable rule

*When facing edge cases:*
- Document the reasoning for unusual type patterns
- Consider if a union type or generic would be more appropriate
- Consult team for complex type scenarios involving advanced TypeScript features

## Exceptions & Waivers

*Valid reasons for exceptions:*
- Third-party library integration requiring `any` types (document with comments)
- Performance-critical code where type checking overhead is measured and significant
- Migration scenarios from JavaScript (time-bounded with migration plan)

*Process for exceptions:*
1. Document the exception reason and timeline in code comments
2. Create tracking issue for resolution if temporary
3. Review exceptions quarterly for removal opportunities

## Quality Gates

- **Automated checks:** TypeScript compiler with strict mode, ESLint TypeScript rules, type coverage reports
- **Code review focus:** Proper type definitions, error handling patterns, naming consistency
- **Testing requirements:** Type tests for complex utilities, runtime validation for external data

## Related Rules

- Frontend Component Rules - TypeScript patterns for React/Vue components
- API Integration Rules - TypeScript patterns for external API consumption
- Testing Standards - TypeScript testing patterns and mocking strategies

## References

- [TypeScript Handbook](https://www.typescriptlang.org/docs/) - Official TypeScript documentation
- [TypeScript Deep Dive](https://basarat.gitbook.io/typescript/) - Advanced TypeScript patterns
- [Effective TypeScript](https://effectivetypescript.com/) - Best practices guide

---

## TL;DR

*Key Principles:*
- Type safety prevents runtime errors and improves developer experience
- Explicit types serve as living documentation and improve IDE support
- Consistent conventions make codebases predictable and maintainable

*Critical Rules:*
- Must enable strict TypeScript configuration
- Must avoid `any` type in favor of proper type definitions
- Must use explicit return types for public functions
- Always implement proper null checking

*Quick Decision Guide:*
When in doubt: Choose the option that provides better type safety and explicit documentation of intent.
