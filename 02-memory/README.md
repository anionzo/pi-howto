# Memory & Context Files

Pi uses several layers to manage context, instructions, and project knowledge. In practice, you will usually work with three buckets:

1. **Context files** such as `AGENTS.md`, `SYSTEM.md`, and `APPEND_SYSTEM.md`
2. **Knowns** as the repo memory and workflow layer
3. **Runtime settings** from `settings.json`

This page explains how those layers fit together and how to use them effectively.

## Overview

| Layer | Purpose | Typical Files / Tools |
|------|---------|------------------------|
| Context files | Project instructions and prompt shaping | `AGENTS.md`, `CLAUDE.md`, `.pi/SYSTEM.md`, `.pi/APPEND_SYSTEM.md` |
| Memory layer | Tasks, docs, templates, searchable knowledge | `knowns ...`, MCP `mcp__knowns__*` |
| Runtime config | Behavior and UI configuration | `~/.pi/agent/settings.json`, `.pi/settings.json` |

## Context Loading Order

Pi loads context files at startup. This affects what instructions the model sees before doing any work.

### AGENTS.md / CLAUDE.md

Pi loads `AGENTS.md` (or `CLAUDE.md`) from:

1. `~/.pi/agent/AGENTS.md` — global instructions
2. Parent directories while walking up from the current working directory
3. Current directory

These matching files are concatenated and provided as context.

### What to put in AGENTS.md

Use `AGENTS.md` for:
- repository conventions
- architecture notes
- coding standards
- common commands
- workflow expectations
- safety rules for the project

**Example:**

```markdown
# Project Guidelines

## Architecture
- Use clean architecture
- Keep business logic out of UI

## Commands
- `npm test` - run tests
- `npm run lint` - lint code

## Rules
- Do not edit generated files manually
```

## KNOWNS.md

In this repository style, `KNOWNS.md` is the **canonical repo guidance file**.

That means:
- `KNOWNS.md` is the source of truth for repo-level agent behavior
- `AGENTS.md` is often only a compatibility shim
- if `AGENTS.md` and `KNOWNS.md` differ, follow `KNOWNS.md`

### Core Principles

From the Knowns workflow:

1. **Knowns is the source of truth** for repo memory and agent workflow
2. **Search first, then read** only the relevant docs and files
3. **Never manually edit Knowns-managed task or doc markdown**
4. **Prefer Knowns/MCP tools first**, CLI as fallback
5. **Validate before marking work complete**
6. **Do not overwrite unrelated user changes**

## Knowns as Memory Layer

Knowns acts as a shared memory system for both humans and agents.

It usually manages:
- tasks
- docs
- templates
- specs
- references
- workflow state
- reusable project knowledge

Think of Knowns as the structured operational memory of the repository.

## Knowns CLI Tools

Use the CLI when MCP tools are unavailable or when you want quick terminal inspection.

### Common Commands

```bash
knowns doc list --plain                 # List docs
knowns task list --plain                # List tasks
knowns task view <id> --plain           # View task details
knowns doc "<path>" --plain --smart    # View a doc
knowns search "query" --plain          # Search docs/tasks
knowns retrieve "query" --json         # Retrieve structured context pack
knowns validate <id-or-path>            # Validate refs and structure
```

### Typical Task Workflow

```bash
knowns task list --plain
knowns task view 8ts3xw --plain
knowns task edit 8ts3xw --status in-progress --assignee "@me"
knowns time start 8ts3xw
```

### Search vs Retrieve

| Tool | Use When |
|------|----------|
| `knowns search` | You need discovery and quick relevance checks |
| `knowns retrieve` | You need assembled context with ranked results and citations |

## Knowns MCP Tools

When available, prefer MCP tools over CLI because they are more structured and workflow-friendly.

### Common MCP operations

- `mcp__knowns__get_task` - get a task by ID
- `mcp__knowns__update_task` - update task status, plan, notes, assignee
- `mcp__knowns__create_task` - create new task
- `mcp__knowns__get_doc` - read a doc with structure awareness
- `mcp__knowns__search` - search docs and memories
- `mcp__knowns__retrieve` - retrieve structured context pack
- `mcp__knowns__validate` - validate refs and entities
- `mcp__knowns__start_time` - start time tracking for a task

### Why MCP first?

Because MCP tools:
- return structured data
- reduce fragile CLI parsing
- fit better into automated agent workflows
- make validation and updates safer

## Context Files

Besides `AGENTS.md`, pi supports system prompt files that directly affect the model prompt.

### SYSTEM.md

Use `.pi/SYSTEM.md` or `~/.pi/agent/SYSTEM.md` to **replace** the default system prompt.

| Path | Scope |
|------|-------|
| `~/.pi/agent/SYSTEM.md` | Global |
| `.pi/SYSTEM.md` | Project |

Use this when you want strong control over the entire system prompt.

### APPEND_SYSTEM.md

Use `.pi/APPEND_SYSTEM.md` or `~/.pi/agent/APPEND_SYSTEM.md` to **append** extra instructions without replacing the default prompt.

This is the safer choice when you only need to add conventions or a lightweight policy layer.

### When to use which?

| File | Use When |
|------|----------|
| `AGENTS.md` | You want repo instructions and workflow guidance |
| `SYSTEM.md` | You want to replace the default system prompt entirely |
| `APPEND_SYSTEM.md` | You want to extend the default prompt without replacing it |

## settings.json

Settings files configure runtime behavior, model defaults, UI, and resource loading.

| Path | Scope |
|------|-------|
| `~/.pi/agent/settings.json` | Global |
| `.pi/settings.json` | Project override |

Project settings override global settings.

### Useful settings related to memory/context

```json
{
  "skills": ["../.claude/skills"],
  "extensions": ["./.pi/extensions"],
  "prompts": ["./.pi/prompts"],
  "theme": "dark"
}
```

Settings commonly influence:
- which skills, prompts, themes, and extensions load
- message delivery behavior
- compaction behavior
- model selection and thinking levels
- session storage and UI behavior

For the full reference, see [09-settings](../09-settings/README.md).

## Memory Workflow Best Practices

### 1. Search first

Before reading many files, start with search.

```bash
knowns search "sessions compaction" --plain
knowns search "providers oauth" --plain
```

### 2. Read only what you need

Do not load every doc into context. Retrieve the sections relevant to the task.

### 3. Follow references

If a task or doc points to:
- `@task-<id>`
- `@doc/<path>`
- `@template/<name>`

follow those references before planning or implementing.

### 4. Use append-style progress updates

When tracking work in Knowns, use append-style notes for progress logs instead of replacing all notes.

### 5. Validate before done

Always validate refs and structure before marking a task complete.

```bash
knowns validate 8ts3xw
```

## Repo Memory Model

A common Knowns mental model:

| Type | Purpose |
|------|---------|
| Tasks | Work tracking |
| Docs | Structured project knowledge |
| Templates | Reusable formats |
| Memories | Durable operational knowledge |
| Working memory | Temporary session-scoped context |

### Durable vs Temporary Memory

- **Working memory**: short-lived investigation state, blockers, temporary findings
- **Project memory**: repo-specific conventions, decisions, patterns
- **Global memory**: stable cross-project preferences and habits

## Recommended Workflow

### Lightweight flow

```text
Search → Read relevant context → Plan → Implement → Validate → Complete
```

### Knowns-oriented flow

```text
Get task → Take ownership → Start timer → Search → Read refs → Plan → Implement → Validate → Mark done
```

## Example: Starting a Task Safely

```bash
knowns task view 8ts3xw --plain
knowns task edit 8ts3xw --status in-progress --assignee "@me"
knowns time start 8ts3xw
knowns search "memory context files" --plain
knowns validate 8ts3xw
```

## Common Mistakes

### Reading everything

Bad:
- loading every repo doc into context
- reading huge files before searching

Better:
- search first
- then read only the relevant docs and sections

### Editing Knowns-managed markdown manually

Bad:
- opening task/doc markdown and editing it directly

Better:
- use Knowns tools or CLI commands

### Confusing config and memory

- `settings.json` configures runtime behavior
- `AGENTS.md` / `SYSTEM.md` shape instructions
- Knowns stores structured project memory and workflow state

## Quick Reference

### File roles

| File | Role |
|------|------|
| `KNOWNS.md` | Canonical repo guidance |
| `AGENTS.md` | Compatibility entrypoint / project instructions |
| `SYSTEM.md` | Replace default system prompt |
| `APPEND_SYSTEM.md` | Append to system prompt |
| `settings.json` | Runtime configuration |

### Tool preference

| Prefer | For |
|--------|-----|
| MCP Knowns tools | Structured task/doc operations |
| `knowns` CLI | Manual inspection and fallback workflows |
| `read`, `grep`, `find` | Local code/docs inspection |
| `bash` | Git, tests, builds, commands |

## Related

- [03-skills](../03-skills/README.md)
- [06-sessions](../06-sessions/README.md)
- [09-settings](../09-settings/README.md)
