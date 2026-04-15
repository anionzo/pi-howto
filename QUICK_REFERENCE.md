# Quick Reference

A compact cheat sheet for everyday pi usage.

## Install

```bash
npm install -g @mariozechner/pi-coding-agent
```

## Start

```bash
pi
/login
```

## Most Useful Commands

| Command | Purpose |
|---------|---------|
| `/model` | Select model |
| `/settings` | Configure theme, thinking, transport |
| `/resume` | Open previous session |
| `/new` | Start new session |
| `/session` | Show path, usage, cost |
| `/tree` | Navigate current session tree |
| `/fork` | Create a new session from current branch |
| `/compact` | Compact context manually |
| `/reload` | Reload extensions, skills, prompts, context |
| `/hotkeys` | Show all keyboard shortcuts |

## Essential CLI Flags

```bash
pi -c                        # continue recent session
pi -r                        # browse sessions
pi --no-session              # ephemeral mode
pi --session <path-or-id>    # open a session
pi --fork <path-or-id>       # fork a session
pi --model openai/gpt-4o     # choose model
pi --thinking high           # choose thinking level
pi -p "Summarize this"       # print mode
pi --mode rpc                # RPC mode
```

## Common Shortcuts

| Key | Action |
|-----|--------|
| `Enter` | Submit prompt |
| `Shift+Enter` | New line |
| `Escape` | Abort / cancel |
| `Ctrl+C` | Clear editor |
| `Ctrl+D` | Exit when editor empty |
| `Ctrl+G` | Open external editor |
| `Ctrl+L` | Model selector |
| `Ctrl+P` | Cycle models forward |
| `Shift+Ctrl+P` | Cycle models backward |
| `Shift+Tab` | Cycle thinking level |
| `Ctrl+T` | Toggle thinking blocks |
| `Ctrl+O` | Toggle tool output |
| `Alt+Enter` | Queue follow-up |
| `Alt+Up` | Restore queued message |

## Message Queue

| Action | Meaning |
|--------|---------|
| `Enter` while streaming | queue steer message |
| `Alt+Enter` | queue follow-up after all work completes |
| `Escape` | abort and restore queued messages |

## File Locations

| What | Path |
|------|------|
| Global config dir | `~/.pi/agent/` |
| Sessions | `~/.pi/agent/sessions/` |
| Settings | `~/.pi/agent/settings.json` |
| Auth file | `~/.pi/agent/auth.json` |
| AGENTS | `~/.pi/agent/AGENTS.md` |
| Extensions | `~/.pi/agent/extensions/` |
| Skills | `~/.pi/agent/skills/` |
| Themes | `~/.pi/agent/themes/` |
| Keybindings | `~/.pi/agent/keybindings.json` |

## Skills Workflow

```text
/skill:kn-init
  → /skill:kn-plan
  → /skill:kn-implement
  → /skill:kn-verify
  → /skill:kn-doc
  → /skill:kn-commit
```

## Package Management

```bash
pi install npm:@foo/bar
pi install git:github.com/user/repo@v1
pi list
pi update
pi remove npm:@foo/bar
pi config
```

## Context Files

| File | Purpose |
|------|---------|
| `AGENTS.md` | Project instructions |
| `KNOWNS.md` | Canonical repo memory / workflow guidance |
| `SYSTEM.md` | Replace system prompt |
| `APPEND_SYSTEM.md` | Append to system prompt |
| `settings.json` | Runtime behavior and resource loading |

## Read Next

- [README.md](README.md)
- [01-commands](01-commands/README.md)
- [06-sessions](06-sessions/README.md)
- [09-settings](09-settings/README.md)
