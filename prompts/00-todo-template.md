# Corrective actions

I have generated a one-shot feature in the application using a single prompt. I have reviewed the feature and found some issues. I have marked each issue with a TODO comment.

I would like you to review the source code in this repository and read all the TODO comments I have added. Group these comments into logical tasks that we can tackle as small units of work. Sometimes these tasks could span multiple TODO comments across different files. At this point you've only done a grouping _without_ writing anything to a file.

Then I want you to generate a new document called `TODO.md` in the root directory of this project. This file should contain only the list of logical tasks that you compiled in the previous step. Each entry in this file should contain the following details:

* A checkbox to indicate if the task is done or not
* The headline of the task
* A description of what the task entails
* A short prompt that instructs an LLM to address this task
* A list of affected files

Here is an example of what each item in the TODO list should look like:

```markdown
### Task 1: Update Database Schema Column Types

- [ ] Convert VARCHAR columns to TEXT in audit table schema

**Prompt**: Update the Flyway migration script `V1__Initial_audit_table.sql` to change all VARCHAR column definitions to TEXT type. The current schema uses VARCHAR with specific length constraints (VARCHAR(50), VARCHAR(100), etc.), but these should be changed to TEXT for better flexibility and consistency with PostgreSQL best practices.

**Files affected**:
- `src/main/resources/db/migration/V1__Initial_audit_table.sql`
```

## IMPORTANT!

* Each entry in `TODO.md` should be structured as a prompt that we can use to generate the code to complete that task. The prompt must have all the relevant details and context for an AI agent to complete the entire task.
* Each prompt should be followed by the complete list of files that are affected.
* Each task should be scoped correctly so it will be completed in a single shot.
* Do not make up your own TODOs! Only use the ones provided in the source code!

## Execution plan

Please add the following section to the top of the TODO.md, referencing any relevant rules:

```markdown
Consider the following rules during execution of the tasks:
- rules/rule1.md
- rules/rule2.md
- rules/rule3.md
```

Please add the following section to the end of the TODO.md:

```markdown
## Execution plan workflow

The following workflow applies when executing this TODO list:
- Execute only the **SPECIFIED TASK**
- Implement the task in **THE SIMPLEST WAY POSSIBLE**
- Run the tests, format the code, and perform static analysis if relevant.
- **Ask me to review the task once you have completed and then WAIT FOR ME**
- Mark the TODO item as complete with [X]
- Commit the changes to Git when I've approved and/or amended the code
- **STOP and await further instructions**
```
