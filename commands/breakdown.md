---
allowed-tools: AskUserQuestion, TodoWrite
argument-hint: "[work item description]"
description: Decompose large work items into smaller, manageable pieces using proven decomposition strategies
---

## Work Item Decomposition

Break down complex work items into smaller, more manageable pieces using proven decomposition strategies.

## Usage

/breakdown [work item description or title]

The user should provide a description of the work item they want to break down. If not provided, ask them to describe it.

## Decomposition Strategies

Choose the appropriate pattern based on the work:

1. **Vertical Slicing** (by user value) - Each slice delivers value, crosses all layers (UI → API → DB)
   - Best for: Feature development, user stories

2. **Horizontal Slicing** (by technical layer) - Split by DB, backend, frontend, integration
   - Best for: Technical refactoring, infrastructure work

3. **Risk-Based** - Spike/research first, then implementation with known solutions
   - Best for: Novel features, unclear requirements, technical unknowns

4. **Dependency-Based** - Identify independent work vs. sequential work
   - Best for: Large features, team coordination, parallel development

5. **Scope-Based** - MVP → Enhancement → Polish
   - Best for: Time-constrained work, iterative delivery

6. **Acceptance Criteria** - One item per major criterion
   - Best for: Well-defined requirements with clear criteria

## Process

### Step 1: Understand the Work Item

Ask the user to provide (or read if they've already provided):
- Title and description of the work
- Acceptance criteria or success conditions
- Type (feature, bug, improvement, etc.)
- Priority level
- Any known complexity indicators (size, uncertainty, risk)

### Step 2: Choose Strategy

Use AskUserQuestion to determine:
- What makes this work too large or risky?
- Are there natural boundaries or stages?
- Are there unknowns needing research first?
- Can parts be done independently?
- Is there a minimum viable scope?

Present strategy options and get user confirmation.

### Step 3: Propose Breakdown

Present decomposition plan:
- List each proposed item with title and brief description
- Explain rationale
- Show dependencies between items
- Identify which can be done in parallel
- Suggest priorities

Ask for confirmation or adjustments via AskUserQuestion.

### Step 4: Document Split Items

For each split item, create a structured description:

```markdown
## Item [N]: [Specific title]

**Type**: [type]
**Priority**: [P0-P4]
**Depends on**: [list of items this depends on, or "None"]

**Description**:
[Clear description with context from original work item]

**Acceptance Criteria**:
- [ ] [Specific criterion]
- [ ] [Specific criterion]

**Notes**:
- [Additional context or constraints]
```

### Step 5: Show Dependency Structure

Present the dependency graph:
```
Original: [Original work item title]

Split into:
1. [Title] (P0) - No dependencies
   ├─ 2. [Title] (P0) - Depends on #1
   ├─ 3. [Title] (P1) - Depends on #1
   └─ 4. [Title] (P2) - Depends on #1

Items #3 and #4 can be done in parallel after #2 completes.
```

### Step 6: Create Action Plan

Offer to create a TodoWrite checklist for tracking the split items, converting each into a trackable todo item.

Ask: "Would you like me to create a todo list to track these items?"

If yes, create todos for each item:
```
TodoWrite with:
- [Item 1 title]
- [Item 2 title]
- [Item 3 title]
etc.
```

### Step 7: Summarize

Present:
- Original work item title
- Number of new items created
- Dependency structure
- Suggested work order (based on dependencies and priorities)
- Recommended next action (start with first non-blocked item)

## Example: Risk-Based Decomposition

**Original**: "Integrate with third-party payment API"
**Analysis**: High uncertainty, unclear API behavior

**Split into**:
1. "Spike: Research payment API and create proof-of-concept" (P0)
   - Depends on: None
   - Time-boxed to 4 hours
   - Deliverable: Document findings and API behavior

2. "Implement payment API integration (core flow)" (P0)
   - Depends on: #1
   - Acceptance: Successfully process a test payment

3. "Add payment error handling and retries" (P1)
   - Depends on: #2
   - Acceptance: Handle network failures, API errors gracefully

4. "Add payment webhooks for status updates" (P2)
   - Depends on: #2
   - Acceptance: Receive and process webhook events

**Dependencies**: All depend on spike to reduce risk. #3 and #4 can be parallel after #2.

## Guidelines

### DO:
- ✅ Preserve original context in split descriptions
- ✅ Use clear, specific titles explaining what each item accomplishes
- ✅ Document dependencies reflecting true technical constraints
- ✅ Consider what can be done in parallel
- ✅ Ensure each item is independently testable
- ✅ Keep items small enough to complete in one focused session (1-4 hours)
- ✅ Explain the rationale for the chosen decomposition strategy

### DON'T:
- ❌ Create too many tiny items (overhead costs matter)
- ❌ Over-decompose simple work
- ❌ Create circular dependencies
- ❌ Split without understanding user's constraints
- ❌ Lose important context from original
- ❌ Make items dependent when they could be parallel

## Item Sizing

**Good**: 1-4 hours focused work, completable in single session, clear acceptance criteria
**Too small**: < 30 minutes, no meaningful deliverable, pure overhead
**Too large**: > 8 hours, multiple concerns mixed, hard to review, high rework risk

## Workflow Summary

1. **Understand**: Get full context on the work item
2. **Analyze**: Identify why it needs breaking down (size, risk, complexity)
3. **Strategize**: Choose appropriate decomposition strategy
4. **Propose**: Present breakdown with rationale
5. **Confirm**: Get user agreement via AskUserQuestion
6. **Document**: Create structured descriptions for each split item
7. **Dependencies**: Show dependency structure and parallel opportunities
8. **Track**: Optionally create TodoWrite checklist
9. **Summarize**: Present next steps and suggested work order

Good decomposition creates clear, manageable work. Each piece should be independently understandable and valuable.
