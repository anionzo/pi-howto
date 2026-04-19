# Memory & Context Files

This section explains how pi loads instructions and manages context.

## What “memory” means in pi

In pi, memory is mostly a combination of:

1. **Instruction files** (what behavior the agent should follow)
2. **Session history** (what has happened in the current/previous chats)
3. **Runtime settings** (what to load and how pi behaves)

## Instruction Files

### `AGENTS.md` / `CLAUDE.md` / `OPENCODE.md` / `GEMINI.md`

Pi loads `AGENTS.md` at startup, and also recognizes compatibility alternatives from other harnesses: `CLAUDE.md`, `OPENCODE.md`, and `GEMINI.md`.

Loading order (all files are **concatenated in this order**, not overridden):

1. `~/.pi/agent/AGENTS.md` (global)
2. Parent directories, walking up from current working directory (ancestors)
3. Current directory

Example for a project at `/home/user/projects/myapp`:

```text
~/.pi/agent/AGENTS.md              <- load 1st (global)
/home/user/projects/AGENTS.md      <- load 2nd (ancestor)
/home/user/projects/myapp/AGENTS.md <- load 3rd (current dir)
```

All matching files are concatenated and added to context.

Use these files for:
- repository conventions
- coding standards
- architecture notes
- safety rules
- common project commands

### `SYSTEM.md`

Use this file to **replace** the default system prompt.

| Path | Scope |
|------|-------|
| `~/.pi/agent/SYSTEM.md` | Global |
| `.pi/SYSTEM.md` | Project |

Use only when you want full control of the base system behavior.

### `APPEND_SYSTEM.md`

Use this file to **append** instructions to the default system prompt.

| Path | Scope |
|------|-------|
| `~/.pi/agent/APPEND_SYSTEM.md` | Global |
| `.pi/APPEND_SYSTEM.md` | Project |

This is safer than replacing the full system prompt.

## Runtime Settings

Settings files:

| Path | Scope |
|------|-------|
| `~/.pi/agent/settings.json` | Global |
| `.pi/settings.json` | Project override |

Project settings override global settings.

Typical settings that affect context loading:

```json
{
  "extensions": ["./.pi/extensions"],
  "skills": ["./.pi/skills"],
  "prompts": ["./.pi/prompts"],
  "theme": "dark"
}
```

## Session Memory

Pi stores sessions as JSONL files (conversation history with tree branching).

- Session files are saved under `~/.pi/agent/sessions/`
- Use `/resume`, `/tree`, and `/fork` to navigate or branch history
- Use `/compact` to summarize older context when needed

Useful CLI flags:

```bash
pi -c                # continue recent session
pi -r                # choose session
pi --no-session      # ephemeral mode
pi --session <path>  # open specific session
pi --fork <path>     # fork session into a new one
```

## Recommended Usage

1. Keep `AGENTS.md` short and practical.
2. Use `APPEND_SYSTEM.md` before `SYSTEM.md` when possible.
3. Put reusable behavior in skills/extensions instead of huge prompt files.
4. Use `/compact` when sessions grow large.
5. Keep project-specific config in `.pi/settings.json`.

## Common Mistakes

- Putting too much policy in one giant system prompt
- Replacing `SYSTEM.md` when append would be enough
- Forgetting that project settings override global settings
- Ignoring session tree tools (`/tree`, `/fork`) and losing useful branches

## Quick Reference

| File | Purpose |
|------|---------|
| `AGENTS.md` | Project instructions and conventions |
| `CLAUDE.md` | Compatibility alternative to `AGENTS.md` |
| `OPENCODE.md` | Compatibility alternative (OpenCode harness) |
| `GEMINI.md` | Compatibility alternative (Gemini harness) |
| `SYSTEM.md` | Replace default system prompt |
| `APPEND_SYSTEM.md` | Append extra instructions |
| `settings.json` | Runtime configuration |

## Related

- [01-commands](../01-commands/README.md)
- [03-skills](../03-skills/README.md)
- [06-sessions](../06-sessions/README.md)
- [09-settings](../09-settings/README.md)
