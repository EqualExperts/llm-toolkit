# Getting Started

A step-by-step guide to creating your first control plane. By the end of this page you'll have a working project structure with architecture docs, conventions, and a system context that loads into every future conversation.

## What you need

### An LLM coding assistant

This process requires an LLM that can:
- Read and write files on your local machine
- Hold a long conversation (the bootstrap takes 30-60 minutes)
- Work with a context file that loads automatically per project

**Tools that work well:**

| Tool | Context file | Notes |
|------|-------------|-------|
| [Claude Code](https://claude.ai/code) (CLI / desktop / IDE) | `CLAUDE.md` | Reads CLAUDE.md automatically. Supports memory, hooks, and background agents. |
| [Cursor](https://cursor.sh) | `.cursorrules` | Popular IDE-based option. Copy the bootstrap prompt into .cursorrules. |
| [Windsurf](https://codeium.com/windsurf) | `.windsurfrules` | Similar to Cursor. |
| [Aider](https://aider.chat) | Convention files | Works with any model via API keys. |
| [Gemini CLI](https://github.com/google-gemini/gemini-cli) | `.gemini/` | Google's CLI tool. |

**Model capability matters.** The bootstrap conversation is an architecture conversation — it requires reasoning about trade-offs, understanding system design, and producing structured documents. Use the strongest model available to you. A frontier model (Claude Opus/Sonnet, GPT-4o, Gemini 2.5 Pro) will produce significantly better results than a smaller model. You can use lighter models for routine implementation work later.

### Git

The control plane is a git repository. You need git installed.

### A project idea

You need to know — at least roughly — what you're building. You don't need a complete specification. "I'm building a booking system for a small hotel chain" is enough to start. The bootstrap conversation will help you work through the rest.

## Step by step

### 1. Create a folder and initialise git

```bash
mkdir my-project
cd my-project
git init
```

### 2. Copy the bootstrap prompt

Copy the bootstrap prompt from this toolkit into the context file your tool expects:

**Claude Code:**
```bash
cp path/to/llm-toolkit/prompts/00-bootstrap-control-plane.md CLAUDE.md
```

**Cursor:**
```bash
cp path/to/llm-toolkit/prompts/00-bootstrap-control-plane.md .cursorrules
```

**Other tools:** copy the content into whatever file your tool reads as project context.

### 3. Start a conversation

Open your LLM tool in the project folder:

```bash
claude          # Claude Code CLI
cursor .        # Cursor IDE
code .          # VS Code with Copilot/extension
```

### 4. Say this

> "Let's set up a control plane for my project."

That's it. The bootstrap prompt takes over from here. It will:

1. **Ask you about your project** — what you're building, who it's for, where you are (greenfield, migration, etc.)
2. **Walk through architecture decisions** — tech stack, boundaries, deployment model, standards
3. **Produce the documents** — CLAUDE.md, conventions, architecture diagrams, a first ADR
4. **Replace itself** — the bootstrap prompt disappears and your real system context takes its place

### 5. Review what it produced

You'll end up with something like:

```
my-project/
├── CLAUDE.md                          # Your system context (the bootstrap is gone)
├── docs/
│   ├── conventions.md                 # Development standards
│   ├── architecture/
│   │   ├── L1-system-context.md       # System in its environment
│   │   └── L2-containers.md           # Deployable units
│   └── adr/
│       └── 001-tech-stack.md          # First architecture decision
├── Makefile                           # Build/test/orchestration
└── repos.conf                         # Child repo list (if multi-repo)
```

Read each file. Correct anything that doesn't match your intent. These documents are the foundation — every future conversation builds on them.

### 6. Commit

```bash
git add -A
git commit -m "Bootstrap control plane"
```

### 7. Start building

The bootstrap conversation will suggest which child project to create first and what to say to start the implementation conversation. Open a new conversation against that project and go.

## Setting up a team

### Shared control plane

The control plane repo should be accessible to everyone on the team — it's the single source of truth for architecture and conventions. Push it to your team's Git host (GitHub, GitLab, etc.) so everyone works against the same context.

When someone updates an architecture doc or convention, the change flows to every team member's next conversation automatically (they pull the repo, the updated CLAUDE.md loads).

### LLM access for the team

Each developer needs their own LLM access. The specifics depend on your tool:

**Claude Code:**
- Each developer needs a Claude account (Pro, Team, or Enterprise)
- Team and Enterprise plans include admin controls, usage visibility, and SSO
- Claude Code is free to use with any Claude plan — the cost is in the API usage / subscription
- Enterprise plans add: zero data retention, SCIM provisioning, audit logs

**API-based tools (Aider, custom setups):**
- Provision API keys per developer (Anthropic, OpenAI, Google, etc.)
- Set usage limits per key to control costs
- Consider a shared billing account with individual key tracking

**IDE-based tools (Cursor, Windsurf):**
- Each developer needs a subscription to the tool
- Some offer team/business tiers with centralised billing

### What to standardise, what to leave flexible

**Standardise:**
- The control plane itself (everyone reads the same architecture docs)
- The conventions (everyone follows the same standards)
- The context file format (everyone uses CLAUDE.md, or .cursorrules, etc.)

**Leave flexible:**
- Which LLM tool each developer uses — the context files are plain Markdown, they work everywhere
- Which model for implementation work — some tasks need a frontier model, some are fine with a lighter one
- Personal workflow preferences — some people talk to the LLM more, some less

### Cost expectations

LLM usage costs vary by model, provider, and intensity of use. As a rough guide:

- A developer using Claude Code actively for 6-8 hours of development work uses roughly $20-60/day of API calls depending on the model and how much context is loaded
- Subscription-based tools (Claude Pro at $20/month, Cursor Pro at $20/month) include a monthly allocation that covers moderate use
- Enterprise/Team plans with API access are billed by usage — budget $200-500/developer/month for active use

The control plane approach reduces costs over time because the context docs mean less re-explanation, shorter conversations, and fewer false starts.

## What happens next

After the bootstrap, your development rhythm becomes:

1. **Architecture conversation** (against the control plane) — when you're adding a new component, making a cross-cutting decision, or changing a boundary
2. **Implementation conversation** (against a child project) — when you're building features, fixing bugs, writing tests
3. **Update the control plane** — after any session that changed something structural

The guides on [conversations](03-conversations.md) cover this rhythm in detail.

## Troubleshooting

**"The LLM didn't ask me anything, it just started generating files"**

Your tool might not be loading the bootstrap prompt as context. Check that the file is named correctly for your tool (CLAUDE.md for Claude Code, .cursorrules for Cursor, etc.) and that you're opening the conversation from the project folder.

**"The architecture docs don't match what I want"**

That's expected — review and correct them. The bootstrap is a starting point, not a finished product. Edit the files directly or tell the LLM what to change.

**"I don't know enough about my project to answer the questions"**

That's fine. Say "I don't know yet" or "let's defer that decision." The bootstrap prompt is designed to handle TBDs. You can fill in the gaps later as you learn more.

**"My project already exists and has code"**

The bootstrap works for existing projects too. When it asks what you're building, describe what already exists. It will produce a control plane that documents the current state rather than designing from scratch. Point it at existing code if it needs to understand the current architecture.
