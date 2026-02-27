# Git Rules

Conventions for commit messages, branching, and pull requests that keep history readable, deployments traceable, and collaboration smooth.

## Context

*Applies to:* All projects using Git for version control
*Level:* Operational - applies to every commit and branch
*Audience:* All developers, tech leads, DevOps engineers

## Core Principles

1. *Atomic Commits:* Each commit represents one logical change — it should compile, pass tests, and be independently revertable
2. *Readable History:* Commit messages explain why, not just what; the diff already shows what changed
3. *Short-Lived Branches:* Integrate frequently to reduce merge conflicts and keep context fresh
4. *Protect Main:* The main/trunk branch must always be in a deployable state
5. *Traceability:* Every commit must link to the work item or issue it addresses

## Rules

### Must Have (Critical)

- *RULE-001:* Use Conventional Commits format: `type(scope): description` — types are `feat`, `fix`, `chore`, `docs`, `refactor`, `test`, `ci`
- *RULE-002:* Write commit message subjects in imperative present tense: "add user validation" not "added" or "adds user validation"
- *RULE-003:* Keep the subject line under 72 characters; use the body for context on why the change was made
- *RULE-004:* Reference the work item in the commit body or footer: `Refs: #123` or `Closes: #123`
- *RULE-005:* Never commit directly to `main`/`master`/`trunk`; all changes must go through a pull request
- *RULE-006:* All CI checks must pass before merging a pull request

### Should Have (Important)

- *RULE-101:* Name branches descriptively using the pattern `type/short-description`: `feat/order-cancellation`, `fix/null-pointer-checkout`
- *RULE-102:* Keep pull requests small and focused — a PR should be reviewable in under 30 minutes
- *RULE-103:* Squash WIP/fixup commits before merging; the merged history should contain only logical, atomic commits
- *RULE-104:* Write a PR description explaining what changed, why, and how to test or verify it
- *RULE-105:* Delete branches after merging

### Could Have (Preferred)

- *RULE-201:* Use signed commits where project security policy requires it
- *RULE-202:* Tag releases using semantic versioning (`v1.2.3`) with a brief annotation describing the release
- *RULE-203:* Maintain a CHANGELOG following Keep a Changelog conventions, updated as part of the release process

## Patterns & Anti-Patterns

### ✅ Do This

```
feat(orders): add cancellation window validation

Orders can only be cancelled within 24 hours of placement.
Validation applied in the domain layer before persistence.

Closes: #247
```

```
fix(auth): regenerate session ID on successful login

Prevents session fixation attacks by issuing a new session token
after authentication is confirmed.

Refs: #312
```

### ❌ Don't Do This

```
fix bug          // no type, no scope, no description
WIP              // not a logical commit — squash it
fixed everything // past tense, vague, no reference
updated files    // meaningless
FINAL v3         // not a commit message
```

## Decision Framework

*When rules conflict:*
1. A passing build and deployable main take priority over branch naming or message format conventions
2. A slightly over-sized but coherent PR is better than splitting changes artificially across multiple PRs
3. When in doubt about commit granularity, prefer smaller commits — they are easier to revert and bisect

*When facing edge cases:*
- Database migrations: commit schema changes and code changes separately to enable staged deployment
- Dependency upgrades: one commit per package aids bisect and rollback
- Emergency hotfixes: may proceed with post-merge async review, but document the exception in the PR

## Exceptions & Waivers

*Valid reasons for exceptions:*
- Emergency production fixes with an explicit commitment to post-merge review
- Initial repository setup (a single "initial commit" is acceptable)
- Automated dependency update PRs from bots (commit format may differ from Conventional Commits)

*Process for exceptions:*
1. Note the exception and reason in the PR description or commit body
2. Ensure a follow-up task exists for any deferred cleanup
3. Get tech lead acknowledgement for any bypass of CI gates

## Quality Gates

- *Automated checks:* Commit message linting (commitlint), branch protection rules enforcing PR review and CI passage, no direct push to protected branches
- *Code review focus:* Commit atomicity, message quality and work item reference, PR description completeness, branch name convention
- *Testing requirements:* Every PR must include passing tests; test changes should be committed alongside the code they test

## Related Rules

- rules/task-execution.md - Commit workflow for TODO-driven development
- rules/testing-principles.md - Tests must pass before a commit is merged
- rules/platform/dotnet.rules.md - Platform-specific quality gate tooling

---

## TL;DR

*Key Principles:*
- Atomic commits, each independently revertable and linked to a work item
- Conventional Commits format keeps history parseable by humans and tools alike
- Short-lived branches integrated through pull requests keep main always deployable

*Critical Rules:*
- Must use Conventional Commits format with imperative present tense (RULE-001, RULE-002)
- Must keep subject line under 72 characters (RULE-003)
- Must reference the work item in every commit (RULE-004)
- Must never commit directly to main — use pull requests (RULE-005)
- Must pass all CI checks before merging (RULE-006)

*Quick Decision Guide:*
When in doubt: Would reverting this commit alone leave the codebase in a valid, deployable state? If not, split the commit.
