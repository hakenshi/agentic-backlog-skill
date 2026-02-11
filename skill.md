# Agentic Backlog Skill Spec

This skill enforces backlog discipline for AI agents.

## Required behavior

- Always call project identification first.
- Never perform large code changes without linking to a task.
- Keep status transitions consistent (`todo` -> `in_progress` -> `review` -> `done`).
- When blocked, set status to `blocked` and include reason note.

## Tool contract (draft)

- `backlog.identify_project`
- `backlog.create_task`
- `backlog.update_status`
- `backlog.add_note`
- `backlog.list_tasks`
