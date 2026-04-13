# Feature Catalog

A compact catalog of the main pi capabilities and where to read more.

## Commands

### Interactive commands

| Feature | Entry point | Notes |
|--------|-------------|-------|
| Authentication | `/login`, `/logout` | OAuth subscription providers |
| Model selection | `/model`, `/scoped-models` | Pick or cycle models |
| Session control | `/new`, `/resume`, `/session` | Start, reopen, inspect |
| Tree navigation | `/tree`, `/fork` | Branch and revisit history |
| Compaction | `/compact` | Manually summarize old context |
| Export/share | `/copy`, `/export`, `/share` | Reuse or publish output |
| Runtime reload | `/reload` | Reload extensions, skills, prompts, context |
| Discovery | `/hotkeys`, `/changelog`, `/quit` | Learn and exit |

### CLI controls

| Feature | Examples |
|--------|----------|
| Continue session | `pi -c` |
| Browse sessions | `pi -r` |
| Ephemeral mode | `pi --no-session` |
| Pick session file | `pi --session <path>` |
| Fork a session | `pi --fork <path>` |
| Print mode | `pi -p "Summarize this"` |
| JSON / RPC | `pi --mode json`, `pi --mode rpc` |

Read more: [01-commands](01-commands/README.md)

## Memory & Context

| Feature | Files / tools | Notes |
|--------|----------------|-------|
| Repo instructions | `AGENTS.md`, `CLAUDE.md` | Startup guidance |
| Canonical repo memory | `KNOWNS.md` | Source of truth in Knowns-style repos |
| Replace system prompt | `SYSTEM.md` | Strongest prompt override |
| Append system prompt | `APPEND_SYSTEM.md` | Additive prompt layer |
| Runtime config | `settings.json` | Behavior and resource loading |
| Task/doc memory | `knowns ...`, `mcp__knowns__*` | Structured workflow memory |

Read more: [02-memory](02-memory/README.md)

## Skills

| Feature | Notes |
|--------|-------|
| Progressive disclosure | Metadata always loaded, body on demand |
| Skill command | `/skill:name` |
| Locations | global, project, packages, settings, CLI |
| Validation | Agent Skills standard, lenient warnings |
| Built-in workflow | `kn-init`, `kn-plan`, `kn-implement`, `kn-verify`, etc. |

Read more: [03-skills](03-skills/README.md)

## Extensions

| Capability | Example |
|-----------|---------|
| Custom tools | domain-specific automation |
| Event interception | permission gates, path protection |
| Commands & shortcuts | `/deploy`, `ctrl+shift+p` |
| Custom UI | widgets, dialogs, full TUI components |
| Provider control | register custom providers or proxies |
| Session-aware state | persisted via session entries |

Read more: [04-extensions](04-extensions/README.md)

## Themes

| Feature | Notes |
|--------|-------|
| Built-in themes | `dark`, `light` |
| Custom themes | JSON files |
| Required tokens | 51 total |
| Value types | hex, 256-color, variable, default |
| Hot reload | active theme reloads automatically |

Read more: [05-themes](05-themes/README.md)

## Sessions

| Feature | Notes |
|--------|-------|
| Storage format | JSONL |
| Tree structure | `id` / `parentId` |
| Versioning | v1, v2, v3 auto migration |
| Branching | `/tree` and `/fork` |
| Compaction | manual or automatic |
| Export | HTML or gist |
| SessionManager | API for SDK/extensions |

Read more: [06-sessions](06-sessions/README.md)

## Keybindings

| Area | Notes |
|------|-------|
| Editor movement | Emacs-like defaults available |
| Input control | submit, new line, autocomplete |
| App control | interrupt, clear, exit, suspend |
| Session control | resume, rename, delete |
| Model control | select, cycle, thinking |
| Tree navigation | fold, unfold, relabel |

Read more: [07-keybindings](07-keybindings/README.md)

## Providers & Models

| Category | Notes |
|---------|-------|
| Subscriptions | Claude Pro/Max, ChatGPT Plus/Pro, Copilot, Gemini CLI, Antigravity |
| API keys | Anthropic, OpenAI, Gemini, Mistral, Groq, Cerebras, xAI, etc. |
| Auth storage | `~/.pi/agent/auth.json` |
| Cloud providers | Azure OpenAI, Amazon Bedrock, Vertex AI |
| Custom providers | `models.json` or extensions |
| Resolution order | CLI → auth.json → env → models.json |

Read more: [08-providers](08-providers/README.md)

## Settings

| Area | Examples |
|------|----------|
| Model defaults | `defaultProvider`, `defaultModel`, `defaultThinkingLevel` |
| UI | `theme`, `quietStartup`, `doubleEscapeAction` |
| Compaction | `compaction.enabled`, `reserveTokens`, `keepRecentTokens` |
| Retry | `maxRetries`, `baseDelayMs`, `maxDelayMs` |
| Delivery | `steeringMode`, `followUpMode`, `transport` |
| Resources | `packages`, `extensions`, `skills`, `prompts`, `themes` |

Read more: [09-settings](09-settings/README.md)

## Pi Packages

| Feature | Notes |
|--------|-------|
| Sources | npm, git, local paths |
| Scope | global or project-local |
| Manifest | `package.json` → `pi` key |
| Conventions | `extensions/`, `skills/`, `prompts/`, `themes/` |
| Filtering | include/exclude paths in settings |
| Enable/disable | `pi config` |

Read more: [10-pi-packages](10-pi-packages/README.md)

## Supporting Files

| File | Purpose |
|------|---------|
| [QUICK_REFERENCE.md](QUICK_REFERENCE.md) | Cheat sheet |
| [README.md](README.md) | Main entry point |
| [vi/README.md](vi/README.md) | Vietnamese version |
