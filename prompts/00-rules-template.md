# Instructions for LLM Rule Generation

When using this template to generate rules, follow these guidelines:

1. **Strictly follow the template structure** - Use the sections provided below as your framework
2. **Omit irrelevant sections** - If a section doesn't apply (e.g., Patterns & Anti-Patterns for non-code topics), remove it entirely
3. **Be concise** - Keep the total output under 200 lines to preserve context window efficiency
4. **Prioritize clarity** - Use clear, actionable language. Avoid ambiguity
5. **Number rules consistently** - Use the RULE-XXX format with appropriate ranges (001-099 for Must Have, 101-199 for Should Have, 201-299 for Could Have)
6. **Focus on the essential** - Capture the core principles and critical rules first, then add supporting details only if space permits
7. **Make it scannable** - Use formatting (bold, lists, code blocks) to enable quick comprehension
8. **Keep examples brief** - Code examples should be minimal yet illustrative
9. **Ensure TL;DR is truly concise** - The TL;DR should capture the essence in under 30 seconds of reading

---

# [Rules Topic/Domain Name]

Brief description of the rules domain and its scope. What aspect of development, architecture, or methodology do these rules govern? Keep this concise and focused on the purpose and boundaries of these rules.

## Context

Provide the situational context where these rules apply. This helps the LLM understand when and why to apply these rules.

*Applies to:* [Project types, layers, components, or scenarios where these rules are relevant]
*Level:* [Strategic/Tactical/Operational - helps prioritize rule application]
*Audience:* [Developers/Architects/Product Team - who should follow these rules]

## Core Principles

List the fundamental principles that underpin all the detailed rules. These are the "why" behind the rules and help with decision-making when specific rules don't cover a scenario.

1. *Principle Name:* Brief explanation of the principle and its importance
2. *Principle Name:* Brief explanation of the principle and its importance
3. *Principle Name:* Brief explanation of the principle and its importance

## Rules

### Must Have (Critical)
Non-negotiable rules that must always be followed. Violation of these rules should block progress.

- *RULE-001:* Description of the rule with clear, actionable guidance
- *RULE-002:* Description of the rule with clear, actionable guidance
- *RULE-003:* Description of the rule with clear, actionable guidance

### Should Have (Important)
Strong recommendations that should be followed unless there's a compelling reason not to.

- *RULE-101:* Description of the rule with clear, actionable guidance
- *RULE-102:* Description of the rule with clear, actionable guidance
- *RULE-103:* Description of the rule with clear, actionable guidance

### Could Have (Preferred)
Best practices and preferences that improve quality but are not blocking.

- *RULE-201:* Description of the rule with clear, actionable guidance
- *RULE-202:* Description of the rule with clear, actionable guidance
- *RULE-203:* Description of the rule with clear, actionable guidance

## Patterns & Anti-Patterns

### ✅ Do This
Concrete examples of what good implementation looks like

// Example of good practice in code block
// Clear, concise code example

### ❌ Don't Do This
Concrete examples of what to avoid

// Example of anti-pattern in code block
// Clear example of what not to do

## Decision Framework

Provide guidance for making decisions when rules conflict or when faced with novel situations

*When rules conflict:*
1. Step 1 for resolution
2. Step 2 for resolution
3. Step 3 for resolution

*When facing edge cases:*
- Guideline 1
- Guideline 2
- Guideline 3

## Exceptions & Waivers

Define when and how these rules can be broken

*Valid reasons for exceptions:*
- Reason 1 (with approval process if needed)
- Reason 2 (with approval process if needed)
- Reason 3 (with approval process if needed)

*Process for exceptions:*
1. Document the exception and rationale
2. [Additional approval steps if needed]
3. [Time-bound review if applicable]

## Quality Gates

Define how adherence to these rules should be verified

- *Automated checks:* What can be validated through tooling
- *Code review focus:* What reviewers should specifically look for
- *Testing requirements:* How rule compliance should be tested

## Related Rules

Reference other rules files that complement or interact with these rules

- rules/related-rules-1.md - Brief description of relationship
- rules/related-rules-2.md - Brief description of relationship
- rules/related-rules-3.md - Brief description of relationship

## References

Links to external resources, standards, or documentation that inform these rules

- [Resource 1](url) - Brief description
- [Resource 2](url) - Brief description
- [Resource 3](url) - Brief description

---

## TL;DR

Ultra-concise summary of the most critical rules and principles. This section should be scannable in under 30 seconds and capture the essence of all rules above.

*Key Principles:*
- Principle 1 in one sentence
- Principle 2 in one sentence
- Principle 3 in one sentence

*Critical Rules:*
- Must do X
- Must not do Y
- Always ensure Z

*Quick Decision Guide:*
When in doubt: [Simple decision heuristic or single guiding principle]
