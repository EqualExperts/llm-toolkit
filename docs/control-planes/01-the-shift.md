# The Shift

## What changes when you work with an LLM

The instinct is to treat an LLM coding assistant like a faster typist — you tell it what to type, it types it, you review. This works, but it's like using a car to push a shopping trolley. Technically functional, dramatically underselling the tool.

The shift is this: **you become the architect, the LLM becomes the builder.** Not because it can't make architectural decisions — it often can — but because you're the one who knows why this system exists, who it serves, and what trade-offs are acceptable. That's the job that doesn't automate well. The mechanical translation of a clear design into working code is the job that does.

This isn't about "prompt engineering." It's about engineering discipline — the same discipline that makes human teams productive — applied to a different kind of collaborator.

## Declarative over imperative

When working with a human developer, you don't dictate keystrokes. You describe what you want, explain the constraints, and trust them to find a good implementation. The same approach works dramatically better with LLMs than line-by-line instruction.

**Imperative** (what most people do):
> "Create a file called UserService.ts. Add a method called getUser that takes an id parameter of type string and returns a Promise of User. Inside the method, call the database..."

**Declarative** (what works better):
> "We need a user lookup service. It should work through the repository interface we defined in the architecture doc. Users are identified by UUID. The service shouldn't know about the database — that's the repository's concern."

The declarative version gives the LLM room to make good mechanical decisions (naming, error handling, edge cases) while keeping you in control of the ones that matter (boundaries, coupling, abstraction level).

This only works if the LLM has enough context to fill in the gaps correctly. That's where the control plane comes in.

## Trust and guardrails

Every productive working relationship is built on trust. Trust doesn't mean blind faith — it means establishing shared expectations and then not second-guessing every step.

With an LLM, trust is built through **guardrails**: project-level documents that define how things should be done. Coding standards. Architectural boundaries. Naming conventions. Testing expectations. When these are written down and loaded into context, you don't need to repeat them in every conversation. The LLM follows them because they're part of its working context, the same way a developer follows team norms after reading the contributing guide.

The pattern is:
1. **Define the guardrail once** — write it in a conventions doc, a CLAUDE.md, or a rule file
2. **Load it automatically** — the LLM reads it at the start of every conversation
3. **Trust it** — don't re-state what's already written
4. **Correct when needed** — save the correction as a new guardrail so it sticks

This is the same pattern good engineering teams use. The difference is that an LLM actually reads the docs.

## Human in the loop — but as a partner

There's a spectrum of how people work with LLMs:

**Micromanager** — dictates every line, reviews every character, doesn't trust the output. Gets exactly what they'd have written themselves, slower.

**Rubber stamper** — accepts everything, doesn't review, ships whatever comes out. Gets working code until it doesn't, then can't debug it because they never understood it.

**Partner** — sets direction, defines constraints, reviews at the level that matters (architecture, behaviour, edge cases), delegates the mechanical work. Catches the LLM's blind spots (it can't see your users, your ops setup, your team's pain points) while letting it handle what it's good at (consistent implementation, broad pattern knowledge, untiring attention to boilerplate).

The partner model is what this toolkit is designed to support. You're not supervising a junior developer. You're collaborating with a very fast, very well-read colleague who has no memory of yesterday and no understanding of your business — unless you write it down.

## Everyone doing what they're best at

Humans are good at:
- Understanding why a system exists and who it serves
- Making trade-off decisions with incomplete information
- Recognising when something feels wrong before they can articulate why
- Navigating ambiguity, politics, and competing priorities
- Knowing what to build next

LLMs are good at:
- Translating a clear specification into consistent code
- Applying patterns across a large codebase without fatigue
- Holding many files in working memory simultaneously
- Following written conventions reliably (more reliably than most humans)
- Producing boilerplate, tests, migrations, and glue code at speed

The shift isn't about replacing developers. It's about freeing developers to spend their time on the work that actually requires human judgment — and having the mechanical work done quickly, consistently, and at a quality level set by the guardrails you've defined.

## Central documents boost everyone

Here's the thing people miss: the documents you write to give the LLM context are **also the best onboarding docs your team has ever had.**

A clear architecture document, a conventions guide, a decision log — these help every new team member, every returning-from-holiday developer, and every future version of you who's forgotten why that module exists. The LLM is just the forcing function that makes you write them.

Projects that adopt this approach consistently report that the documentation isn't overhead — it's the most valuable artefact of the process. It makes the humans faster too.

## Bringing people on the journey

If you're reading this and thinking "I just want to know what to type" — that's fine. Start with the [control plane guide](02-control-plane.md) and follow the steps. The structure will teach you the thinking as you use it.

If you're reading this and thinking "this sounds like a lot of upfront work" — it is, the first time. But the documents you write in the first hour save hundreds of hours of re-explanation across every conversation that follows. The ROI is fast and it compounds.

If you're reading this and thinking "I already know how to code, I don't need this" — you're right, you don't need it to write code. You need it to write code *with an LLM* at a level that's worth the disruption to your existing workflow. Without structure, the LLM is a novelty. With structure, it's a multiplier.
