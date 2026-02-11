# Agentic Backlog Scrum

Use this skill whenever you are doing implementation work and need task tracking discipline.

This skill assumes a local MCP server named `agentic-backlog` is available.

## Trigger phrases

Use this skill proactively when the user says things like:

- `\agentic-backlog:add`
- `\agentic-backlog:remove`
- `\agentic-backlog:update`
- `\agentic-backlog:read`
- `\agentic-backlog:board`
- `\agentic-backlog:in-progress`
- `\agentic-backlog:show-task`
- `\agentic-backlog:update-task`

Expected behavior by trigger:

- **\agentic-backlog:add** -> create a task (`backlog.create_task`)
- **\agentic-backlog:remove** -> delete only with explicit confirmation (`backlog.delete_task` + `confirm: "DELETE"`)
- **\agentic-backlog:update** -> update task fields or status (`backlog.update_task` / `backlog.update_task_status`)
- **\agentic-backlog:read** -> fetch board/task snapshot (`backlog.get_board` / `backlog.get_console_table`)
- **\agentic-backlog:board** -> return board overview (`backlog.get_board` + `backlog.get_console_table`)
- **\agentic-backlog:in-progress** -> list active WIP (`backlog.list_tasks` with `status: in_progress`)
- **\agentic-backlog:show-task** -> find a specific task by title keywords, then show it (`backlog.list_tasks` + `backlog.get_task`)
- **\agentic-backlog:update-task** -> find a specific task by title keywords, then update it (`backlog.list_tasks` + `backlog.update_task`)

Task-by-name behavior:

- Use user-provided title keywords to search current project tasks.
- Prefer exact title match; otherwise use best partial match.
- If multiple close matches exist, show top candidates and ask which one to use.
- For updates, always echo the matched task title/id before applying the change.

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
   - Refresh board before each major implementation step.
   - When reporting progress to the user, call `backlog.get_console_table`.
3. Ensure active task exists
   - If no suitable task exists, call `backlog.create_task`.
4. Start work
   - Move task to `in_progress` with `backlog.update_task_status` or `backlog.update_task`.
5. During work
   - Add short progress notes with `backlog.add_task_note`.
   - Re-read the board after each note/status transition.
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

- Default behavior is preview mode.
- Use `apply=true` to persist generated actions.
- Apply only if actions are relevant to the current project scope.

## Guardrails

- Do not perform large code changes without linking to a task.
- Keep notes objective and short.
- Prefer one active `in_progress` task per agent/session.
- Deletions must be explicit: call `backlog.delete_task` with `confirm: "DELETE"`.
- Keep updates frequent: at least one status or note update per completed sub-step.
