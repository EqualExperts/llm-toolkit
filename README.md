# Overview

A collection of rules and commands that MAY be useful for your LLM projects.

- Be opinionated
- Be brief
- Be specific
- Break down rules and prompts so as to avoid adding unnecessary context

## Rules

A curated collection of rules for LLM-assisted development. Rules are expressed as plain Markdown (`.md`) files to remain tool-agnostic and work across different AI coding assistants.

### Contributing New Rules

To contribute new rules:

1. Use the template format in `prompts/00-rules-template.md`. Generate rules with a meta-prompt like:
   ```
   Please generate me a new rule formatted according to prompts/00-rules-template.md that summarises the principles of [topic/methodology] in under 200 lines.
   ```

2. Submit a pull request with your new rule. All PRs require review from at least one code owner.

### Using as a Git Submodule

To add this library to your existing project with prompts, you can include it as a git submodule. This creates a `library` subdirectory within your `prompts` folder containing all the rules and templates.

### Setup

1. Navigate to your project's `prompts` directory:
   ```bash
   cd prompts
   ```

2. Add this repository as a submodule named `library`:
   ```bash
   git submodule add https://github.com/EqualExperts/llm-rules.git library
   ```

3. Initialize and update the submodule:
   ```bash
   git submodule update --init --recursive
   ```

4. Commit the submodule addition:
   ```bash
   git add .gitmodules library
   git commit -m "Add llm-rules as library submodule"
   ```

## Updating the Library

To pull the latest updates from the library:

```bash
cd prompts/library
git pull origin main
cd ..
git add library
git commit -m "Update llm-rules library"
```

### Structure

After setup, your prompts directory will look something like:

```
prompts/
├── 01-optional_vendor_validation-oneshot.md
├── 01-optional_vendor_validation-todo.md
├── 02-idempotent_post_and_delete-oneshot.md
└── library/
    ├── rules/
    │   ├── domain-driven-design.md
    │   ├── hexagonal-architecture.md
    │   ├── kotest.md
    │   └── kotlin.md
    └── templates/
        ├── 00-oneshot_template.md
        ├── 00-rules-template.md
        └── 00-todo_template.md
```

## Commands

Two interactive commands are provided to help with work item management:

- **/clarify**: Transform vague ideas into well-defined, actionable work items with clear scope and acceptance criteria
- **/breakdown**: Decompose large work items into smaller, manageable pieces using proven decomposition strategies

### Installation

**Claude Code**:

Create symbolic links to install both commands in your Claude Code environment:

```bash
ln -s "$(pwd)/commands/clarify.md" ~/.claude/commands/
ln -s "$(pwd)/commands/breakdown.md" ~/.claude/commands/
```

Or install from a submodule:

```bash
ln -s "$(pwd)/prompts/library/commands/clarify.md" ~/.claude/commands/
ln -s "$(pwd)/prompts/library/commands/breakdown.md" ~/.claude/commands/
```

**Gemini CLI**:

Create symbolic links to install both commands in your Gemini CLI environment:

```bash
ln -s "$(pwd)/commands/clarify.toml" ~/.gemini/commands/
ln -s "$(pwd)/commands/breakdown.toml" ~/.gemini/commands/
```

Or install from a submodule:

```bash
ln -s "$(pwd)/prompts/library/commands/clarify.toml" ~/.gemini/commands/
ln -s "$(pwd)/prompts/library/commands/breakdown.toml" ~/.gemini/commands/
```