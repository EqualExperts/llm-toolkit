# The Control Plane

## What it is

A control plane is a repository that contains **no application code**. It holds the architecture, conventions, orchestration, and context that tie your projects together. It's the single place where you answer "how does this whole system work?" — for yourself, for your team, and for your LLM.

If you have one project, the control plane is a set of docs in that project's root. If you have multiple projects, it's a separate repo that references them.

The name comes from networking — a control plane manages routing and policy while the data plane moves the actual packets. Same idea: the control plane manages decisions and structure while the child projects handle the actual work.

## Why it works

Without a control plane, context lives in people's heads. Every LLM conversation starts with "let me explain the project..." and every new team member asks the same questions the last one did.

With a control plane:
- The LLM reads the architecture doc and knows how the system fits together
- Coding conventions are followed automatically because they're in the context
- Decisions are recorded once and don't need re-debating
- New conversations pick up where old ones left off (structurally, not literally)
- New team members orient themselves in hours, not weeks

## What goes in it

### 1. System context (CLAUDE.md or equivalent)

The root-level context file is the most important document in the project. It loads automatically at the start of every LLM conversation and gives the assistant a complete mental model.

A good system context includes:
- **What the project is** — one paragraph, plain language
- **Core principles** — 3-5 non-negotiable design values
- **Architecture overview** — how the pieces fit together
- **Package/project list** — what exists, what each thing does
- **Conventions reference** — link to the full conventions doc
- **Commands** — how to build, test, run

What it should NOT include:
- Implementation details that change frequently
- Long code samples
- Meeting notes or status updates

Keep it under 200 lines. It loads into every conversation — bloat costs you context window.

### 2. Architecture documents

Use whatever notation your team understands. The [C4 model](https://c4model.com) works well because its levels map naturally to LLM conversation scope:

- **L1 System Context** — the system in relation to users and external systems. Read this first in any architecture conversation.
- **L2 Containers** — all deployable units and their relationships. This is the control plane's home territory.
- **L3 Components** — internal structure of a single container. Lives in the child project, not the control plane.

Each diagram is a Markdown file with a diagram (Mermaid, PlantUML, or just ASCII boxes) and a few paragraphs of explanation.

### 3. Conventions

A single `conventions.md` that defines:
- Language and framework versions
- Directory structure expectations
- Naming patterns
- Testing requirements
- How to handle errors, logging, configuration
- What goes in a commit message
- PR process

Be specific. "Write good tests" is useless. "Every public function has at least one happy-path test and one error-path test, using the Arrange-Act-Assert pattern" is a guardrail.

### 4. Decision records

Architecture Decision Records (ADRs) capture **why** you chose what you chose. They're short documents with:
- Context (what's the situation)
- Decision (what we chose)
- Consequences (what follows from the choice)

These are invaluable for LLMs. Without them, the assistant might suggest an approach you've already considered and rejected. With them, it understands the constraints.

### 5. repos.conf — the project registry

For multi-project setups, a `repos.conf` file lists every child repository the control plane manages. It's a plain text file, one repo per line:

```
# Format: <git-url>  <directory-name>
git@github.com:myorg/user-service.git      user-service
git@github.com:myorg/api-gateway.git       api-gateway
git@github.com:myorg/web-client.git        web-client
git@github.com:myorg/shared-types.git      shared-types

# Internal / private
git@github.com:myorg/billing-service.git   billing-service
git@github.com:myorg/admin-portal.git      admin-portal
```

This file is the single source of truth for "what repos exist in this system." The Makefile reads it. Architecture docs reference it. When you add a new service, you add one line here and the orchestration picks it up automatically.

Use comments (`#`) to group repos by concern — public vs private, core vs supporting, services vs libraries. The grouping is for humans; the tooling treats every uncommented line the same.

### 6. Orchestration (Makefile)

The Makefile reads `repos.conf` and operates across every child repo:

```makefile
REPOS := $(shell grep -v '^\#' repos.conf | grep -v '^$$' | awk '{print $$2}')

get:       ## Clone missing repos, pull existing ones
	@for dir in $(REPOS); do \
	  if [ -d "projects/$$dir" ]; then \
	    echo "Pulling $$dir..."; \
	    git -C projects/$$dir pull --ff-only; \
	  else \
	    url=$$(grep "$$dir$$" repos.conf | awk '{print $$1}'); \
	    echo "Cloning $$dir..."; \
	    git clone $$url projects/$$dir; \
	  fi; \
	done

build:     ## Build all projects
	@for dir in $(REPOS); do \
	  echo "Building $$dir..."; \
	  $(MAKE) -C projects/$$dir build || exit 1; \
	done

test:      ## Test all projects
	@for dir in $(REPOS); do \
	  echo "Testing $$dir..."; \
	  $(MAKE) -C projects/$$dir test || exit 1; \
	done

status:    ## Show git status across all projects
	@for dir in $(REPOS); do \
	  echo "=== $$dir ==="; \
	  git -C projects/$$dir status --short --branch; \
	done
```

The power of `repos.conf` + `Makefile` together:

- **`make get`** — one command clones every repo on a fresh machine or pulls updates on an existing one. New team member? Clone the control plane, run `make get`, they have everything.
- **`make build`** / **`make test`** — runs across the whole system. Catches cross-project breakage early. Stops at the first failure so you don't waste time.
- **`make status`** — instant view of what's changed where. Uncommitted work, branches ahead/behind, repos on the wrong branch. Invaluable when you've been bouncing between projects.
- **Adding a new repo** — add one line to `repos.conf`. The Makefile, the LLM's context, and any developer who runs `make get` all pick it up.

This isn't a build system — it's a coordination tool. Each child project has its own build process (npm, cargo, gradle, whatever). The Makefile just knows how to find them and call `make build` in each one. The convention that every project has a Makefile with `build`, `test`, and `run` targets is what makes this work.

### 7. Child project context

Each child project gets its own context file (CLAUDE.md or equivalent) that covers:
- What this project does (in one paragraph)
- Its architecture (L3 level)
- How to build, test, run
- Key design decisions specific to this project
- What it depends on and what depends on it

This file loads when you open a conversation against that project. It shouldn't repeat what's in the control plane — it should complement it.

## Getting started

### Step 1: Create the control plane

```bash
mkdir my-control-plane
cd my-control-plane
git init
```

### Step 2: Write the system context

Start with what you know. You don't need perfect architecture docs on day one. Write:
- What you're building and why
- What tech you've chosen and why
- What exists so far
- What you're planning to build next

This document will grow as you make decisions. That's the point.

### Step 3: Have the architecture conversation

Open a conversation against the control plane and say something like:

> "I'm building [description]. I've written a system context in CLAUDE.md. I'd like to work through the architecture — what are the major components, what are the boundaries between them, and what decisions should I make before writing any code?"

This conversation produces documents, not code. At the end of it you should have:
- An updated system context
- At least an L1 and L2 architecture diagram
- A conventions doc (even if it's short)
- A list of child projects/packages with one-line descriptions

### Step 4: Start a child project

Create the first child project as its own repo. Give it a CLAUDE.md that references the control plane's architecture. Then open a conversation against the child project:

> "This is the [name] package from the architecture we defined. It should [responsibility]. Let's build the foundation — types, interfaces, and a basic implementation with tests."

### Step 5: Iterate

The control plane evolves as you build. Each decision you make in a child project that affects other projects gets recorded in the control plane. Architecture diagrams update to reflect reality. Conventions grow as you discover patterns.

The control plane is a living document, not a planning exercise.

## Common mistakes

**Over-planning** — spending days on architecture docs before writing any code. Write enough to start, then refine as you learn. The LLM is fast enough that changing direction is cheap.

**Under-documenting** — skipping the context file because "I know what this does." You do, today. You won't in three months. The LLM never does without it.

**Mixing concerns** — having architecture conversations in implementation sessions or vice versa. Keep them separate. Different context, different mode of thinking.

**Giant context files** — cramming everything into one document. If your CLAUDE.md is 500 lines, split it. Link to detailed docs instead of inlining them. Context window is finite and every line costs.

**Not updating** — writing docs once and never touching them again. If the architecture changed but the docs didn't, the LLM will work against the old architecture. Stale docs are worse than no docs.
