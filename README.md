# Overview

A collection of rules that MAY be useful for your LLM projects.

- Be opinionated
- Be brief
- Be specific
- Break down rules and prompts so as to avoid adding unnecessary context

# Rules

Like [Awesome Cursor rules](https://github.com/PatrickJS/awesome-cursorrules), except some level of curation and review

# Contributing New Rules

To contribute new rules:

1. Use the template format in `prompts/00-rules-template.md`. Generate rules with a meta-prompt like:
   ```
   Please generate me a new rule formatted according to prompts/00-rules-template.md that summarises the principles of [topic/methodology] in under 200 lines.
   ```

2. Submit a pull request with your new rule. All PRs require review from at least one code owner.

# Using as a Git Submodule

To add this library to your existing project with prompts, you can include it as a git submodule. This creates a `library` subdirectory within your `prompts` folder containing all the rules and templates.

## Setup

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

## Structure

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

# Prompts

Prompt templates and snippets
