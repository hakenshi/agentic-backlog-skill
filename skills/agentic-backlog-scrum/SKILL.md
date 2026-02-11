# Agentic Backlog Scrum

Use this skill whenever you are doing implementation work and need task tracking discipline.

This skill assumes a local MCP server named `agentic-backlog` is available.

## Core tools (CRUD)

- `backlog.create_task`
- `backlog.get_task`
- `backlog.list_tasks`
- `backlog.update_task`
- `backlog.delete_task`

## Goal

Keep a shared backlog that multiple AI agents can update consistently.

## Mandatory workflow

1. Identify project
   - Call `backlog.identify_project` first.
   - Store returned `project.id`.
2. Get current board
   - Call `backlog.get_board` and inspect WIP.
3. Ensure active task exists
   - If no suitable task exists, call `backlog.create_task`.
4. Start work
   - Move task to `in_progress` with `backlog.update_task_status` or `backlog.update_task`.
5. During work
   - Add short progress notes with `backlog.add_task_note`.
6. On completion
   - Move to `review` or `done`.
7. On blockers
   - Move to `blocked` and include reason.

## Status model

- `backlog`
- `todo`
- `in_progress`
- `blocked`
- `review`
- `done`
- `cancelled`

## Tool usage contract

Always include metadata when available:

- `source`: agent/client name (for example `opencode`, `claude-code`)
- `agent_id`: stable agent identity when available
- `session_id`: current conversation/session id when available

## Planning mode

Use `backlog.plan_from_context` for fast task drafting from plain text.

- Use `dry_run=true` to preview.
- Apply only if actions are relevant to the current project scope.

## Guardrails

- Do not perform large code changes without linking to a task.
- Keep notes objective and short.
- Prefer one active `in_progress` task per agent/session.
