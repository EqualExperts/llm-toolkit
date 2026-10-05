# Surgical Changes

Rules governing how an AI assistant modifies existing code. The goal is a diff in which every changed line traces directly to the request, so reviewers can verify intent quickly and unrelated behaviour is not altered as a side effect.

## Context

*Applies to:* Any task where an AI assistant edits an existing codebase, including bug fixes, features and requested refactors
*Level:* Operational - applies to every edit
*Audience:* Developers configuring AI assistants; the assistant itself; reviewers of AI-generated changes

## Core Principles

1. *Traceable Diffs:* Every changed line should be explainable by the request
2. *Clean Up Your Own Mess Only:* Remove what your change made obsolete; leave pre-existing code alone
3. *Local Consistency:* Match the conventions already in the file, not the ones you prefer
4. *Surface, Don't Fix:* Report problems outside the request instead of silently changing them
5. *Ask Before Choosing:* Ambiguity that changes the diff is a reason to ask, not to guess

## Rules

### Must Have (Critical)

- *RULE-001:* Change only the code required by the request; do not reformat, rename, reorder or reword comments in adjacent code
- *RULE-002:* Do not refactor or rewrite working code outside the request, even where it falls short of other rules in this toolkit; report it instead (RULE-103)
- *RULE-003:* Remove imports, variables, functions, files and configuration that your own change made unused
- *RULE-004:* Do not delete pre-existing dead code, commented-out code or TODOs outside the request; mention them instead
- *RULE-005:* When a request has more than one plausible interpretation that would produce a different diff, present the interpretations and ask before editing

### Should Have (Important)

- *RULE-101:* Match the existing style of the code being changed — naming, error handling, file layout and test structure — even where a different convention would be preferred
- *RULE-102:* Before editing, state which files are expected to change; if more turn out to be necessary, say which and why
- *RULE-103:* After the change, list any unrelated issues noticed (bugs, dead code, security concerns) as a short follow-up list
- *RULE-104:* Prefer the smallest change that meets the acceptance criteria; if a broader change would be materially better, propose it rather than doing it
- *RULE-105:* Limit changes to generated and lock files to those produced by the tooling the change requires

### Could Have (Preferred)

- *RULE-201:* When asked for both a behavioural change and a mechanical one (renames, formatting), keep them in separate commits
- *RULE-202:* When a change touches shared code, name the callers or modules affected

## Patterns & Anti-Patterns

Request: "Fix the crash when `discount` is null."

### ✅ Do This

```diff
 function applyDiscount(order, discount) {
+  if (discount == null) return order.total;
   return order.total * (1 - discount.rate);
 }
```

Noted, not changed: `legacyDiscount()` in the same file appears unused.

### ❌ Don't Do This

```diff
-function applyDiscount(order, discount) {
-  return order.total * (1 - discount.rate);
+const applyDiscount = (order: Order, discount?: Discount): number => {
+  // Refactored for clarity
+  return discount ? order.total * (1 - discount.rate) : order.total;
 }
-
-function legacyDiscount(order) { ... }
```

Style changed, types added, function rewritten and unrelated code deleted to fix a one-line bug.

## Decision Framework

*When rules conflict:*
1. Within the request's scope, the other rules in this toolkit apply in full to the code you write
2. Outside the request's scope, this rule takes precedence: report, don't change. For example, rules/clean-code.md RULE-004 (remove dead code) applies to code you write or are already changing; pre-existing dead code elsewhere is reported under RULE-004 of this rule
3. A security issue found outside scope is reported prominently and immediately, but fixed only when the user agrees

*When facing edge cases:*
- If the fix genuinely requires changing adjacent code, change it and say why
- If matching existing style would introduce a defect, follow the safer pattern and point out the inconsistency
- If unsure whether a line is in scope, ask whether a reviewer could connect it to the request

## Exceptions & Waivers

*Valid reasons for exceptions:*
- The request is itself a refactor, cleanup or formatting task; its stated scope defines what may change
- Repository tooling (formatters, pre-commit hooks) rewrites lines automatically as part of the required workflow

*Process for exceptions:*
1. State the broader scope before starting
2. Keep mechanical and behavioural changes in separate commits where practical

## Quality Gates

- *Automated checks:* Formatters and linters run in CI, so formatting never needs manual changes in unrelated code
- *Code review focus:* Every hunk traces to the request; no unexplained renames, deletions or style changes; out-of-scope findings appear in the PR description, not the diff
- *Testing requirements:* Existing tests pass unchanged unless the requested behaviour change requires updating them

## Related Rules

- rules/code-quality.md - Its "Don't remove unrelated code" and "Don't invent changes" guidance is the baseline this rule makes specific
- rules/clean-code.md - Applies to code written within scope; see the Decision Framework for RULE-004
- rules/task-execution.md - Complements "implement in the simplest way possible"
- rules/git-rules.md - Small, focused PRs (RULE-102) depend on small, focused diffs

## References

- [andrej-karpathy-skills](https://github.com/multica-ai/andrej-karpathy-skills) - MIT-licensed guidelines for reducing common LLM coding mistakes; the "Surgical Changes" principle this rule adapts

---

## TL;DR

*Key Principles:*
- Every changed line should trace to the request
- Clean up what your change orphaned; leave everything else alone
- Match the file's existing style

*Critical Rules:*
- Must not touch adjacent code beyond the request (RULE-001, RULE-002)
- Must remove only what your own change made unused (RULE-003, RULE-004)
- Must ask when interpretations would produce different diffs (RULE-005)

*Quick Decision Guide:*
When in doubt: if a reviewer couldn't connect a line to the request, don't change it — mention it.
