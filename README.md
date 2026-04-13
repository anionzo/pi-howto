# pi-howto

<p align="center">
  <img src="assets/logo/pi-logo.svg" alt="pi logo" width="128">
</p>

> Practical documentation for using and customizing the pi coding agent.

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

## Table of Contents

| Section | Description |
|---------|-------------|
| [01-commands](01-commands/README.md) | Slash commands, CLI options, editor features, message queue |
| [02-memory](02-memory/README.md) | `AGENTS.md`, `KNOWNS.md`, `SYSTEM.md`, `APPEND_SYSTEM.md`, settings |
| [03-skills](03-skills/README.md) | Skill locations, format, validation, built-in workflow skills |
| [04-extensions](04-extensions/README.md) | TypeScript extensions, events, tools, commands, UI |
| [05-themes](05-themes/README.md) | Theme format, 51 color tokens, hot reload |
| [06-sessions](06-sessions/README.md) | JSONL session files, branching, compaction, tree navigation |
| [07-keybindings](07-keybindings/README.md) | Keyboard shortcut reference and customization |
| [08-providers](08-providers/README.md) | OAuth subscriptions, API keys, cloud providers |
| [09-settings](09-settings/README.md) | Global and project settings reference |
| [10-pi-packages](10-pi-packages/README.md) | Packaging, installation, sharing resources |

Additional references:
- [CATALOG.md](CATALOG.md) - feature catalog
- [QUICK_REFERENCE.md](QUICK_REFERENCE.md) - short cheat sheet
- [vi/](vi/README.md) - Vietnamese translation
- [index.md](index.md) - alternate index entrypoint

## Quick Start

```bash
npm install -g @mariozechner/pi-coding-agent

# API key flow
export ANTHROPIC_API_KEY=sk-ant-...
pi

# or subscription flow
pi
/login
```

## Common First Actions

```bash
/model          # pick model
/settings       # tune theme, thinking, transport
/resume         # reopen older work
/tree           # navigate current session tree
/hotkeys        # see shortcuts
```

## Feature Overview

| Feature | What it gives you | Quick entry |
|---------|-------------------|-------------|
| Commands | Interactive control over sessions and runtime | `/settings`, `/tree`, `/resume` |
| Sessions | Persistent JSONL conversation history with branching | `/fork`, `/compact` |
| Skills | Reusable capability packages | `/skill:kn-plan` |
| Extensions | Custom tools, events, commands, UI | `.pi/extensions/` |
| Themes | Visual customization with hot reload | `/settings` |
| Memory | Context and workflow files | `AGENTS.md`, `KNOWNS.md` |
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
- [x] CATALOG.md
- [x] QUICK_REFERENCE.md

## Related Links

- [Official pi repository](https://github.com/badlogic/pi-mono)
- [npm package](https://www.npmjs.com/package/@mariozechner/pi-coding-agent)
- [Agent Skills specification](https://agentskills.io)
- [Discord community](https://discord.gg/3cU7Bz4UPx)
