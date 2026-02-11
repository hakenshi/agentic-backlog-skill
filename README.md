# Agentic Backlog Skills

Skill collection in the `vercel-labs/agent-skills` format, focused on SCRUM workflows for AI agents.

## Boundary (important)

- Skills are documentation and operating procedures for agents.
- Skills do not own persistence or business logic.
- The backlog application/API is the source of truth.
- MCP is the communication channel agents use to understand and operate that running backlog API.

## Available skill

- `agentic-backlog-scrum`

## Structure

- `skills/agentic-backlog-scrum/SKILL.md`

## Dependency

This skill expects a local MCP server named `agentic-backlog` with backlog tools.

## Installation (skills style)

```bash
npx skills add hakenshi/agentic-backlog-skill
```

## Usage

Example prompts:

- "Use the agentic-backlog-scrum skill and keep the backlog updated while implementing"
- "Before coding, sync with backlog and tell me the active task"
- "\\agentic-backlog:add"
- "\\agentic-backlog:update"
- "\\agentic-backlog:read"
- "\\agentic-backlog:show-task login"
- "\\agentic-backlog:update-task login set status in_progress"

## Quick demo prompts

- "\\agentic-backlog:add create task 'Ship onboarding MVP' priority high"
- "\\agentic-backlog:update-task onboarding set status in_progress"
- "\\agentic-backlog:read"
- "\\agentic-backlog:board"
