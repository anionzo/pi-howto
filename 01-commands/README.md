# Commands

This chapter is the fastest path to using pi effectively.

## What is pi? When should you use it?

Pi is a minimal terminal coding harness.

Use pi when you want to:
- work directly in your codebase through conversation
- drive common actions with slash commands and shortcuts
- keep session history so you can resume, branch, and compact work
- customize the experience instead of being locked into one workflow

## Quick Start

```bash
npm install -g @mariozechner/pi-coding-agent
pi
/login
```

> `/login` is the most provider-neutral way to get started. Pi supports multiple providers such as OpenAI, Anthropic, Gemini, and GitHub Copilot.

## Typical First Workflow

1. Open a terminal and run `pi`
2. Use `/login` to authenticate and choose a provider
3. Start a fresh session with `/new` or reopen previous work with `/resume`
4. Use `/model` to pick a model
5. Ask pi to help with your first task

## Core Slash Commands

| Command | What it does |
|---------|---------------|
| `/login`, `/logout` | Sign in or out using supported OAuth flows |
| `/model` | Open the model picker |
| `/scoped-models` | Choose which models participate in `Ctrl+P` cycling |
| `/settings` | Open runtime settings |
| `/resume` | Reopen an older session |
| `/new` | Start a new session |
| `/name <name>` | Rename the current session |
| `/session` | Show path, token usage, and cost |
| `/tree` | Navigate the current session tree |
| `/fork` | Create a new session from the current branch |
| `/compact [prompt]` | Compact older context |
| `/copy` | Copy the last assistant response |
| `/export [file]` | Export the session to HTML |
| `/share` | Upload the session as a private GitHub gist |
| `/reload` | Reload keybindings, extensions, skills, prompts, and context files |
| `/hotkeys` | Show keyboard shortcuts |
| `/changelog` | Show version history |
| `/quit` | Quit pi |

## Important Keyboard Shortcuts

- `Enter` when pi is idle: submit your prompt
- `Enter` while the agent is working: queue a steering message for delivery after the current turn finishes its tool calls
- `Alt+Enter`: queue a follow-up message after the agent finishes all current work
- `Ctrl+L`: open the model picker
- `Ctrl+P` / `Shift+Ctrl+P`: cycle through scoped models

## Editor Features

- type `@` to search project files
- press `Tab` to complete paths
- use `Shift+Enter` to add a new line
- use `!command` to run bash and send output back to the model
- use `!!command` to run bash without sending output back
- paste images with `Ctrl+V` or `Alt+V` depending on platform

## Message Queue

- `Enter` while the agent is working: queue a steering message
- `Alt+Enter`: queue a follow-up message
- `Escape`: abort current work and restore queued messages to the editor
- `Alt+Up`: pull queued messages back into the editor

## Useful CLI Options

```bash
pi -c                          # continue recent session
pi -r                          # browse sessions
pi --no-session                # ephemeral mode
pi --session <path-or-id>      # open specific session
pi --fork <path-or-id>         # fork a session
pi --model openai/gpt-4o       # choose model
pi --thinking high              # set thinking level (off/minimal/low/medium/high/xhigh)
pi -p "Summarize this"          # print mode (text to stdout, then exit)
pi --mode json                  # JSON event stream (JSONL to stdout)
pi --mode rpc                   # RPC headless mode (JSON protocol via stdin/stdout)
pi --no-skills                  # disable skill discovery
pi --no-themes                  # disable theme discovery
pi --no-prompt-templates        # disable prompt template discovery
pi --list-models                # list available models
pi --list-models claude         # filter models by query
```

> **Note on `ctx.hasUI`:** In print mode and JSON mode, `ctx.hasUI` is `false`. In interactive and RPC mode, it is `true`. This is relevant for extension authors who need to know whether UI methods like `ctx.ui.notify()` will work. See [12-headless-modes](../12-headless-modes/README.md) for details.

## Common Pitfalls

- **Provider confusion:** pi is not tied to a single provider. Keep intro docs provider-neutral and use `/login` first.
- **Too much too early:** you do not need to learn skills, extensions, or themes before using the basic workflow.
- **Session sprawl:** use `/resume` or `/tree` before creating more sessions than you need.
- **Shortcut confusion:** `Enter` and `Alt+Enter` do different things while the agent is still working.

## Read Next

- [02-memory](../02-memory/README.md)
- [06-sessions](../06-sessions/README.md)
- [07-keybindings](../07-keybindings/README.md)
- [12-headless-modes](../12-headless-modes/README.md)
