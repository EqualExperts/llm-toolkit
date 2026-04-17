# Working Conversations

## Two types of conversation

The single most useful distinction in LLM-assisted development: **architecture conversations** and **implementation conversations** serve different purposes, need different context, and produce different outputs. Mixing them up is the most common way to waste time.

### Architecture conversations

**When:** You're deciding what to build, how it fits together, where the boundaries are, what the trade-offs look like.

**Where:** Against the control plane (or the root of a monorepo).

**What you produce:** Documents — architecture diagrams, decision records, updated conventions, interface definitions. Not code.

**How it sounds:**

> "We need to add payment processing. Looking at the architecture, it should probably be a separate package that depends on the commerce module. The question is whether the Stripe integration lives inside the payment package or alongside it. What are the trade-offs?"

**Signals you're in the right mode:**
- You're talking about boundaries, not implementations
- You're producing Markdown, not TypeScript
- You're asking "should we" not "how do we"
- The conversation could apply regardless of programming language

**Signals you've drifted:**
- You're looking at specific files
- You're writing functions
- You're debugging
- The conversation has narrowed to one module's internals

### Implementation conversations

**When:** You know what to build (the architecture conversation already happened) and you're building it.

**Where:** Against the child project that owns the code.

**What you produce:** Code — modules, tests, configuration, migrations. Guided by the architecture docs.

**How it sounds:**

> "The payment package needs a checkout session creator. It should take a basket (from the commerce module) and return a Stripe checkout URL. The interface is defined in the architecture doc. Let's build it with tests."

**Signals you're in the right mode:**
- You're referencing decisions already made
- You're producing code in specific files
- You're running tests
- The conversation is scoped to one project

**Signals you've drifted:**
- You're questioning whether this package should exist
- You're redesigning interfaces that other packages depend on
- You're making cross-cutting decisions without updating the control plane

When you catch yourself drifting, stop. If you've discovered something that changes the architecture, go have that conversation against the control plane first. Then come back.

## Starting a conversation well

The first message sets the trajectory. A good opening gives the LLM:

1. **What you're trying to accomplish** — the goal, not the steps
2. **What's already decided** — reference the docs, don't re-explain them
3. **What's open** — where you want input vs where you've already decided
4. **The scope** — what's in bounds for this session

**Weak opening:**
> "I need to add a feature"

**Strong opening:**
> "The admin package needs client-side routing so navigation between the sidebar and editor doesn't do a full page reload. The plan is in the architecture doc under 'Admin SPA routing.' I want to start with the catch-all Astro route and the client-side router — the data loading API can come second. Let's build it."

The strong version tells the LLM exactly where to focus, what docs to reference, and what's out of scope. It can start working immediately instead of asking clarifying questions.

## Giving feedback

LLMs learn within a conversation from your corrections. But they forget everything between conversations. The trick is to save feedback that matters:

**Ephemeral feedback** — "that variable name is confusing, rename it." Just say it. It doesn't need to persist.

**Durable feedback** — "don't mock the database in integration tests; we got burned by that last quarter." This should become a guardrail — a line in your conventions doc, a rule file, or a memory note. Otherwise you'll give the same correction in every future conversation.

The distinction: if you'd tell the same thing to a new team member on their first day, it belongs in the docs. If it's specific to this moment, just say it.

## When to start a new conversation

**Start fresh when:**
- You're switching from architecture to implementation (or vice versa)
- You're moving to a different project
- The conversation has gotten confused or gone off track
- You've been going for hours and the context is getting bloated
- You've finished a logical unit of work

**Continue when:**
- You're iterating on the same piece of work
- The context from earlier in the conversation is still relevant
- Starting fresh would mean re-explaining a lot

There's no shame in starting over. A fresh conversation with a good opening message is often faster than trying to redirect a confused one. The docs and guardrails mean you don't lose anything — the new conversation picks up the same context automatically.

## The feedback loop

After a productive session:

1. **Update the architecture docs** if anything changed structurally
2. **Add conventions** if you discovered a pattern worth repeating
3. **Record decisions** if you made any that affect other projects
4. **Save feedback** if the LLM did something you'd want it to do (or not do) next time

This takes five minutes and saves hours across future conversations. The control plane gets better every time you use it.

## Patterns that work

### The spike conversation

> "I want to explore whether [approach] would work for [problem]. This is a spike — we're not committing to the code. Build the minimum to prove or disprove the idea."

Good for: evaluating trade-offs, testing feasibility, learning a new library. The LLM builds something throwaway and you learn from the shape of it.

### The migration conversation

> "We're moving from [old] to [new]. Here's the current code. Migrate it file by file, preserving all existing tests. Don't change any public interfaces."

Good for: framework upgrades, refactors, dependency bumps. The constraint (preserve tests, preserve interfaces) keeps the LLM from getting creative where you don't want it.

### The review conversation

> "I've made these changes [diff]. Review them against our conventions and architecture. Flag anything that violates a boundary, introduces a new dependency pattern, or could cause problems I might have missed."

Good for: a second pair of eyes on your own code. The LLM catches what you've gone blind to.

### The documentation conversation

> "This project works but the docs are stale. Read the current code and update the CLAUDE.md and architecture diagrams to reflect what actually exists."

Good for: keeping the control plane honest. The LLM reads the codebase as-is and writes docs that match reality.

## What doesn't work

**"Build me an app"** — too vague, no guardrails. The LLM will produce something, but it won't match your mental model because you haven't communicated one.

**Dictating every line** — you could have typed it yourself. The LLM adds no value when it has no room to make decisions.

**Ignoring the output** — shipping code you didn't read. The LLM is not infallible. It's consistent and fast, but it makes mistakes — especially around edge cases, security boundaries, and business logic it can't see.

**Marathon sessions** — 8-hour conversations lose coherence. The context fills up, earlier decisions get contradicted, and the LLM starts working against itself. Break work into focused sessions with clear goals.
