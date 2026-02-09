# Azure Bicep Review Rules (Annotation Generation)

Rules for generating error annotations when reviewing Azure Bicep files against Microsoft best practices. Focused on precise, actionable, and minimal feedback for CI bots, code review tools, and LLM agents.

## Context

Guidance for producing annotations from a Bicep source file without modifying it.

*Applies to:* Automated Bicep reviews, CI pipelines, IDE linters, LLM-based reviewers
*Level:* Operational
*Audience:* Platform engineers, DevOps, reviewers, and automation authors

## Core Principles

1. Best-practices driven: Derive findings from Azure Bicep best practices; avoid subjective opinions.
2. Minimal and actionable: Report only the most impactful issues with concise, fix-oriented messages.
3. Precise and respectful: Pinpoint a single line per finding and respect in-source suppression signals.

## Rules

### Must Have (Critical)
Non-negotiable rules that must always be followed.

- RULE-001: Generate error annotations based on Azure Bicep best practices (see References). Do not alter the source file.
- RULE-002: Output only the top 3 most important annotations, ranked by impact and severity.
- RULE-003: Each annotation must target exactly one line (single-line range only).
- RULE-004: Do not generate notes or ancillary commentary; output errors only.
- RULE-005: If a line starts with "#disable-next-line genaiscript", ignore the next line entirely for annotation generation.

### Should Have (Important)
Strong recommendations to improve clarity and usefulness.

- RULE-101: Prefer findings that materially improve security, reliability, or correctness over stylistic nits when prioritizing top 3.
- RULE-102: Provide a concise, actionable error message (≤1 sentence) that implies the fix without extra notes.
- RULE-103: Deduplicate similar issues; if multiple rules trigger on the same line, emit a single combined message.

### Could Have (Preferred)
Helpful preferences that aren't blocking.

- RULE-201: When multiple candidate issues tie, prefer earlier lines and higher-severity categories (e.g., security > reliability > performance > style).
- RULE-202: Keep output format consistent and machine-readable for downstream tooling.
- RULE-203: Use stable identifiers (resource symbolic names, parameter names) when referencing entities in messages.

## Patterns & Anti-Patterns

### ✅ Do This
Concrete examples of good output shape.

// Single-line, error-only, concise, respects suppression
{
  "line": 42,
  "severity": "error",
  "message": "Avoid hardcoding location; use a parameter or resourceGroup().location."
}

// Suppression example: if line 10 starts with
//   #disable-next-line genaiscript
// then do not emit any annotation for line 11.

### ❌ Don't Do This
Concrete examples to avoid.

// Multi-line range or more than 3 findings
[
  { "startLine": 5, "endLine": 9, "severity": "warning", "message": "..." },
  { "line": 20, "severity": "note", "message": "..." }
]

// Emitting notes or exceeding the top-3 limit is not allowed.

## Decision Framework

*When rules conflict:*
1. Favor security/correctness issues over style or cosmetic concerns.
2. Prefer higher-severity, broadly impactful issues; then choose earliest line numbers.
3. If still tied, prefer de-duplicating into a single, clearer error message.

*When facing edge cases:*
- Empty or comment-only files: produce no findings.
- Multiple issues on the same line: emit one concise, combined error.
- More than 3 issues: apply prioritization and emit only the top 3.

## Exceptions & Waivers

*Valid reasons for exceptions:*
- Controlled experiments or diagnostics requiring more than 3 findings in a temporary debug mode.
- Contractual integration that mandates a different output format.

*Process for exceptions:*
1. Document the exception, scope, and duration in the pipeline/repo docs.
2. Obtain tech lead approval.
3. Time-box the exception and review periodically.

## Quality Gates

- Automated checks: Validate single-line ranges; enforce max of 3 errors; verify no notes are present; confirm suppression behavior.
- Code review focus: Prioritization soundness, message clarity, respect for suppression directives, consistency of output.
- Testing requirements: Unit tests for suppression, tie-breaking, de-duplication, empty files, and top-3 capping.

## Related Rules

- rules/code-quality.md - General guidance for actionable, minimal review comments
- rules/platform/dotnet.instructions.md - Example platform-specific instruction style

## References

- Azure Bicep best practices: https://learn.microsoft.com/en-us/azure/azure-resource-manager/bicep/best-practices

---

## TL;DR

Provide only the top 3 best-practice errors for a Bicep file, each tied to exactly one line, with no notes and with suppression respected.

*Key Principles:*
- Base findings on Bicep best practices.
- Keep feedback minimal and actionable.
- Respect in-source suppression and be precise.

*Critical Rules:*
- Emit errors only; no notes.
- At most 3 annotations; each is single-line.
- Ignore the next line after a "#disable-next-line genaiscript" directive.

*Quick Decision Guide:*
When in doubt, prioritize security/correctness, choose the earlier line, and keep the message short and fix-oriented.
