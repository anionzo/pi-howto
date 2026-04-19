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
| `/model` | Select a model |
| `/settings` | Configure theme, thinking, and transport |
| `/resume` | Open a previous session |
| `/new` | Start a new session |
| `/session` | Show path, usage, and cost |
| `/tree` | Navigate the current session tree |
| `/fork` | Create a new session from the current branch |
| `/compact` | Compact older context manually |
| `/reload` | Reload extensions, skills, prompts, and context |
| `/hotkeys` | Show all keyboard shortcuts |

## Essential CLI Flags

```bash
pi -c                        # continue recent session
pi -r                        # browse sessions
pi --no-session              # ephemeral mode
pi --session <path-or-id>    # open a session
pi --fork <path-or-id>       # fork a session
pi --model openai/gpt-4o     # choose model
pi --thinking high           # choose thinking level (off/minimal/low/medium/high/xhigh)
pi -p "Summarize this"       # print mode
pi --mode json               # JSON event stream
pi --mode rpc                # RPC mode (JSON protocol via stdin/stdout)
pi --no-skills               # disable skill discovery
pi --no-themes               # disable theme discovery
pi --no-prompt-templates     # disable prompt template discovery
pi --list-models claude      # list models matching query
```

## Operating Modes

| Mode | Command | `ctx.hasUI` |
|------|---------|-------------|
| Interactive | `pi` | `true` |
| Print | `pi -p "query"` | `false` |
| JSON | `pi -p "query" --mode json` | `false` |
| RPC | `pi --mode rpc` | `true` |

## Common Shortcuts

| Key | Action |
|-----|--------|
| `Enter` | Submit prompt when pi is idle |
| `Shift+Enter` | New line |
| `Escape` | Abort / cancel |
| `Ctrl+C` | Clear editor |
| `Ctrl+D` | Exit when editor is empty |
| `Ctrl+G` | Open external editor |
| `Ctrl+L` | Open model selector |
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
| `Enter` while streaming | queue a steering message |
| `Alt+Enter` | queue a follow-up after all current work completes |
| `Escape` | abort and restore queued messages |

## File Locations

| What | Path |
|------|------|
| Global config dir | `~/.pi/agent/` |
| Sessions | `~/.pi/agent/sessions/` |
| Settings | `~/.pi/agent/settings.json` |
| Auth file | `~/.pi/agent/auth.json` |
| Project instructions | `~/.pi/agent/AGENTS.md` |
| Extensions | `~/.pi/agent/extensions/` |
| Skills | `~/.pi/agent/skills/` |
| Themes | `~/.pi/agent/themes/` |
| Keybindings | `~/.pi/agent/keybindings.json` |

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
| `CLAUDE.md` | Compatibility alternative to `AGENTS.md` |
| `SYSTEM.md` | Replace the default system prompt |
| `APPEND_SYSTEM.md` | Append to the default system prompt |
| `settings.json` | Runtime behavior and resource loading |

## Read Next

- [README.md](README.md)
- [01-commands](01-commands/README.md)
- [06-sessions](06-sessions/README.md)
- [09-settings](09-settings/README.md)
- [12-headless-modes](12-headless-modes/README.md)
