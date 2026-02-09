When you need to perform long running or multi stage tasks, create an appropriately named markdown file in the docs/tasks directory and populate that document with a markdown checklist of your tasks. As you begin a task, mark that task as in progress by using a ~ character in the appropriate checkbox (Do not create a new section for in progress items, update the existing item; add subtasks and additional details to that item as required). Once a task is complete, mark it as complete. The checklist should always reflect your current state of work.

The task list should have sufficient detail so that you can restart the task with an empty context.

The list below shows the valid states for checklist items:

- [x] Completed
- [~] In Progress
    - [x] Child item completed
    - [~] Child item in progress
    - [ ] Child item not started
- [ ] Not started
