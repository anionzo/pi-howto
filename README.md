# pi-howto

<p align="center">
  <img src="assets/logo/pi-logo.svg" alt="pi logo" width="128">
</p>

> Practical documentation for using and customizing the pi coding agent.

Pi is a minimal terminal coding harness. This repo organizes the core concepts, day-one usage, and deeper customization paths into a guide you can navigate progressively.

## What This Repo Covers

This guide explains the main parts of pi:
- interactive commands
- memory and context files
- skills
- extensions
- themes
- session management
- keybindings
- providers and models
- settings
- pi packages
- prompt templates

## Table of Contents

| Section | Description |
|---------|-------------|
| [01-commands](01-commands/README.md) | Commands, common shortcuts, editor features, message queue |
| [02-memory](02-memory/README.md) | `AGENTS.md`, `CLAUDE.md`, `SYSTEM.md`, `APPEND_SYSTEM.md`, settings |
| [03-skills](03-skills/README.md) | Skill locations, format, validation, built-in workflow skills |
| [04-extensions](04-extensions/README.md) | TypeScript extensions, events, tools, commands, UI |
| [05-themes](05-themes/README.md) | Theme format, 51 color tokens, hot reload |
| [06-sessions](06-sessions/README.md) | JSONL session files, branching, compaction, tree navigation |
| [07-keybindings](07-keybindings/README.md) | Keyboard shortcut reference and customization |
| [08-providers](08-providers/README.md) | OAuth subscriptions, API keys, cloud providers |
| [09-settings](09-settings/README.md) | Global and project settings reference |
| [10-pi-packages](10-pi-packages/README.md) | Packaging, installation, sharing resources |
| [11-prompt-templates](11-prompt-templates/README.md) | Reusable prompt macros, arguments, examples |

Additional references:
- [CATALOG.md](CATALOG.md) - feature catalog
- [QUICK_REFERENCE.md](QUICK_REFERENCE.md) - short cheat sheet
- [vi/](vi/README.md) - Vietnamese translation
- [index.md](index.md) - alternate index entrypoint

## Quick Start

```bash
npm install -g @mariozechner/pi-coding-agent

# start pi
pi

# log in and pick your provider
/login
```

## Common First Actions

```bash
/model          # pick a model
/settings       # tune theme, thinking, transport
/new            # start a fresh session
/resume         # reopen older work
/tree           # navigate the current session tree
/hotkeys        # see shortcuts
```

## Feature Overview

| Feature | What it gives you | Quick entry |
|---------|-------------------|-------------|
| Commands | Interactive control over sessions and runtime | `/login`, `/model`, `/settings` |
| Sessions | Persistent JSONL conversation history with branching | `/resume`, `/tree`, `/fork`, `/compact` |
| Memory | Project instructions and prompt shaping | `AGENTS.md`, `CLAUDE.md`, `SYSTEM.md`, `APPEND_SYSTEM.md` |
| Skills | Reusable capability packages | `/skill:name` |
| Extensions | Custom tools, events, commands, and UI | `.pi/extensions/` |
| Themes | Visual customization with hot reload | `/settings` |
| Prompt Templates | Reusable prompt macros for repeatable tasks | `.pi/prompts/`, `/template-name` |
| Packages | Shareable bundles of extensions/skills/prompts/themes | `pi install ...` |

## Documentation Status

### English
- [x] 01-commands
- [x] 02-memory
- [x] 03-skills
- [x] 04-extensions
- [x] 05-themes
- [x] 06-sessions
- [x] 07-keybindings
- [x] 08-providers
- [x] 09-settings
- [x] 10-pi-packages
- [x] 11-prompt-templates
- [x] CATALOG.md
- [x] QUICK_REFERENCE.md

### Vietnamese
- [x] README
- [x] 01-commands
- [x] 02-memory
- [x] 03-skills
- [x] 04-extensions
- [x] 05-themes
- [x] 06-sessions
- [x] 07-keybindings
- [x] 08-providers
- [x] 09-settings
- [x] 10-pi-packages
- [x] 11-prompt-templates
- [x] CATALOG.md
- [x] QUICK_REFERENCE.md

## Related Links

- [Official pi repository](https://github.com/badlogic/pi-mono)
- [npm package](https://www.npmjs.com/package/@mariozechner/pi-coding-agent)
- [Agent Skills specification](https://agentskills.io)
- [Discord community](https://discord.gg/3cU7Bz4UPx)
