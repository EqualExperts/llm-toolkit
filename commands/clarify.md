---
allowed-tools: AskUserQuestion, TodoWrite
argument-hint: "[optional: idea description]"
description: Interactive tool to refine vague ideas into well-defined, actionable work items
---

## Idea Refinement Workshop

Transform rough ideas into crisp, well-defined work items with clear scope and acceptance criteria.

## Usage

/clarify [optional: brief idea description]

If no description provided, ask the user to describe their idea first.

## Refinement Process

### Phase 1: Understand the Idea

Use AskUserQuestion to explore:
- What problem does this solve?
- Who benefits from it? (users, developers, operations)
- What triggered this idea? (pain point, opportunity, requirement)
- What does success look like?

Capture: problem, beneficiaries, motivation

### Phase 2: Classify Work Type

Determine the work type using AskUserQuestion if ambiguous:
- **Feature**: New functionality adding user/business value
- **Bug**: Something broken needing fixing
- **Improvement**: Enhancement to existing functionality (performance, UX, quality)
- **Spike**: Time-boxed research/exploration
- **Task**: Technical work without direct user impact (refactoring, tooling, dependencies)
- **Epic**: Large initiative needing decomposition (suggest using the breakdown command)

Capture: type, rationale

### Phase 3: Define Scope Boundaries

Use AskUserQuestion to clarify:
- What's IN scope?
- What's explicitly OUT of scope?
- What's the minimum viable version?
- What are nice-to-have additions?

Capture: scope statement, explicit exclusions, MVP definition

### Phase 4: Define Success Criteria

Establish concrete acceptance criteria using AskUserQuestion:
- **Observable outcomes**: What will be different when done?
- **Testable criteria**: How will we verify it works?
- **Definition of done**: What conditions must be met?

For bugs: current behavior, expected behavior, reproduction steps, impact
For features: user stories, success metrics, edge cases

Capture: 3-7 specific acceptance criteria, edge cases, constraints

### Phase 5: Assess Complexity and Risk

Identify challenges:
- Any unknowns or uncertainties?
- Does this touch risky/complex system parts?
- Dependencies on external systems or teams?
- What could go wrong?

Capture: risk level (low/medium/high), key uncertainties, suggested approach

### Phase 6: Determine Priority

Use AskUserQuestion based on:
- **P0 (Critical)**: Urgent, blocking other work or users
- **P1 (High)**: Important, should be done soon
- **P2 (Medium)**: Normal priority, default for most work
- **P3 (Low)**: Nice to have, can wait
- **P4 (Backlog)**: Maybe someday, low priority

Consider: impact, urgency, strategic value

Capture: priority level with rationale

### Phase 7: Craft the Work Item

**Title Format**: Specific, action-oriented, includes key context
- ✅ "Add user authentication with JWT tokens"
- ✅ "Fix crash when uploading files >10MB"
- ❌ "Auth stuff" / "Make it faster"

**Description Template for Features/Improvements:**
```markdown
## Problem/Opportunity
[What problem does this solve or opportunity does it capture?]

## Proposed Solution
[High-level approach]

## Acceptance Criteria
- [ ] [Specific, testable criterion 1]
- [ ] [Specific, testable criterion 2]
- [ ] [Specific, testable criterion 3]

## Out of Scope
- [Explicitly excluded item 1]
- [Explicitly excluded item 2]

## Notes
- [Additional context, constraints, or considerations]
```

**Description Template for Bugs:**
```markdown
## Current Behavior
[What's happening now]

## Expected Behavior
[What should happen]

## Steps to Reproduce
1. [Step 1]
2. [Step 2]
3. [Step 3]

## Impact
[Who is affected and severity]

## Acceptance Criteria
- [ ] Bug no longer occurs when following reproduction steps
- [ ] [Additional verification needed]
```

### Phase 8: Present the Refined Work Item

Present summary to user:
```
Type: [type]
Priority: [P0-P4]
Title: [title]

Description:
[Full description with acceptance criteria]
```

Ask for confirmation via AskUserQuestion.

If the work item has multiple acceptance criteria that could be tracked separately, offer to use TodoWrite to create a checklist from the acceptance criteria.

### Phase 9: Next Steps

Suggest appropriate next actions:
- If complex/uncertain: "Consider breaking this down into smaller pieces" or "Create a spike first to explore unknowns"
- If ready: "You can start working on this now"
- If dependencies: "Consider what needs to be done first and create those work items"
- If has clear acceptance criteria: "Would you like me to create a todo list to track progress?"

## Guidelines

### DO:
- ✅ Ask open-ended questions to understand user thinking
- ✅ Help articulate implicit assumptions
- ✅ Push for concrete, testable acceptance criteria
- ✅ Identify risks and uncertainties early
- ✅ Keep scope tight (suggest breaking down for large work)
- ✅ Make exclusions explicit (out of scope matters)
- ✅ Suggest spikes for high-uncertainty work

### DON'T:
- ❌ Accept vague criteria ("make it better", "improve performance")
- ❌ Skip understanding the "why"
- ❌ Let scope creep into the definition
- ❌ Create work items without clear success criteria
- ❌ Assume you understand without asking
- ❌ Create epics without suggesting decomposition

## Workflow Summary

1. **Understand**: What's the idea? What problem does it solve?
2. **Classify**: What type of work?
3. **Scope**: What's in/out? What's MVP?
4. **Success**: What are concrete acceptance criteria?
5. **Risk**: What could go wrong? Unknowns?
6. **Priority**: How urgent and important?
7. **Craft**: Write clear title and description
8. **Present**: Show refined work item and get confirmation
9. **Next**: Suggest next steps (breakdown, spike, start work, create todos)

Good refinement saves time later. A well-defined work item is a joy to work on.
