# Commands

Slash commands cho tương tác với pi. Type `/` in the editor to trigger command completion.

## Built-in Commands

### Authentication

| Command | Mô Tả | Ví Dụ |
|---------|-------|--------|
| `/login` | OAuth authentication - select provider and sign in | `/login` |
| `/logout` | Sign out from current provider | `/logout` |

**Supported subscriptions:** Anthropic Claude Pro/Max, OpenAI ChatGPT Plus/Pro (Codex), GitHub Copilot, Google Gemini CLI, Google Antigravity

### Model Management

| Command | Mô Tả | Ví Dụ |
|---------|-------|--------|
| `/model` | Switch to a different model - opens interactive selector | `/model` → select from list |
| `/scoped-models` | Enable/disable models for Ctrl+P cycling | `/scoped-models` → toggle models |

**Usage tips:**
- Press `Ctrl+L` to open model selector directly
- Use `Ctrl+P` / `Shift+Ctrl+P` to cycle through scoped models
- Model format: `provider/model-id` (e.g., `openai/gpt-4o`)
- Add thinking level: `sonnet:high`, `claude:medium`

### Session Management

| Command | Mô Tả | Ví Dụ |
|---------|-------|--------|
| `/resume` | Pick from previous sessions | `/resume` → browse sessions |
| `/new` | Start a new session | `/new` |
| `/name <name>` | Set session display name | `/name my-feature` |
| `/session` | Show current session info | `/session` → view details |

**`/session` output includes:**
- Session file path
- Total token usage
- Cache token usage
- Estimated cost
- Current context usage percentage

### Tree Navigation

| Command | Mô Tả | Ví Dụ |
|---------|-------|--------|
| `/tree` | Navigate session tree, jump to any point | `/tree` → select entry |
| `/fork` | Create new session from current branch | `/fork` → create copy |
| `/compact [prompt]` | Manually compact context | `/compact` or `/compact summarize` |

**`/tree` features:**
- Search by typing
- Fold/unfold branches with `Ctrl+←` / `Ctrl+→` or `Alt+←` / `Alt+→`
- Page navigation with `←` / `→`
- Filter modes (`Ctrl+O`): default → no-tools → user-only → labeled-only → all
- Label entries (`Shift+L`) as bookmarks
- Toggle timestamps (`Shift+T`)

**`/fork` workflow:**
1. Opens tree selector
2. Choose point to fork from
3. Creates new session file
4. Selected message placed in editor for modification

**Compaction:**
- Automatic: triggers on context overflow or when approaching limit
- Manual: `/compact` or `/compact <custom instructions>`
- Configurable via `/settings` or `settings.json`
- Full history preserved in JSONL - revisit via `/tree`

### Export & Share

| Command | Mô Tả | Ví Dụ |
|---------|-------|--------|
| `/copy` | Copy last assistant message to clipboard | `/copy` |
| `/export [file]` | Export session to HTML file | `/export report.html` |
| `/share` | Upload as private GitHub gist | `/share` → get link |

**Export format:** Self-contained HTML with syntax highlighting and session structure.

### Reload & Info

| Command | Mô Tả | Ví Dụ |
|---------|-------|--------|
| `/reload` | Reload keybindings, extensions, skills, prompts | `/reload` |
| `/hotkeys` | Show all keyboard shortcuts | `/hotkeys` |
| `/changelog` | Display version history | `/changelog` |
| `/settings` | Open settings UI (thinking level, theme, etc.) | `/settings` |
| `/quit` | Quit pi | `/quit` |

**Reload (`/reload`) refreshes:**
- Keybindings from `~/.pi/agent/keybindings.json`
- Extensions from extensions directories
- Skills from skills directories
- Prompt templates from prompts directories
- Context files (AGENTS.md, SYSTEM.md)

**Note:** Themes hot-reload automatically without needing `/reload`.

## Custom Commands

Extensions can register custom commands. Type `/` to see all available commands including custom ones.

**Example:** Custom command registration in extension:
```typescript
pi.registerCommand("stats", {
  description: "Show project statistics",
  execute: async (ctx) => {
    // Command implementation
  }
});
```

## Editor Features

### Input Features

| Feature | How to Use |
|---------|------------|
| File reference | Type `@` to fuzzy-search project files |
| Path completion | Press Tab to complete paths |
| Multi-line input | `Shift+Enter` (or `Ctrl+Enter` on Windows) |
| Images | `Ctrl+V` to paste (or `Alt+V` on Windows), or drag onto terminal |
| Bash commands | `!command` runs and sends output to LLM |
| Bash (silent) | `!!command` runs without sending output |

### Navigation

| Key | Action |
|-----|--------|
| `Ctrl+G` | Open external editor (uses `$VISUAL` or `$EDITOR`) |

## CLI Options

### Session Options

```bash
pi -c, --continue        # Continue most recent session
pi -r, --resume          # Browse and select from past sessions
pi --no-session          # Ephemeral mode (don't save)
pi --session <path>       # Use specific session file or ID
pi --fork <path>          # Fork specific session into new session
pi --session-dir <dir>    # Custom session storage directory
```

### Model Options

```bash
pi --provider <name>      # Provider (anthropic, openai, google, etc.)
pi --model <pattern>      # Model pattern or ID
pi --model openai/gpt-4o  # Provider prefix format
pi --model sonnet:high    # With thinking level shorthand
pi --thinking <level>     # off, minimal, low, medium, high, xhigh
pi --models "claude-*,*"  # Patterns for Ctrl+P cycling
pi --list-models          # List available models
pi --api-key <key>        # Override API key
```

### Tool Options

```bash
pi --tools read,bash,edit,write  # Enable specific tools (default)
pi --no-tools                     # Disable all built-in tools
```

**Available tools:** `read`, `bash`, `edit`, `write`, `grep`, `find`, `ls`

### Resource Options

```bash
pi -e, --extension <source>    # Load extension (repeatable)
pi --no-extensions              # Disable extension discovery
pi --skill <path>              # Load skill (repeatable)
pi --no-skills                 # Disable skill discovery
pi --prompt-template <path>    # Load prompt template (repeatable)
pi --no-prompt-templates       # Disable prompt template discovery
pi --theme <path>              # Load theme (repeatable)
pi --no-themes                 # Disable theme discovery
```

### Output Modes

```bash
pi -p, --print           # Print response and exit (non-interactive)
pi --mode json            # Output all events as JSON lines
pi --mode rpc             # RPC mode for process integration
pi --export <in> [out]    # Export session to HTML
```

### Other Options

```bash
pi --system-prompt <text>           # Replace default system prompt
pi --append-system-prompt <text>    # Append to system prompt
pi --verbose                        # Force verbose startup
pi -h, --help                       # Show help
pi -v, --version                    # Show version
```

### File Arguments

Prefix files with `@` to include in the message:

```bash
pi @prompt.md "Answer this"
pi -p @screenshot.png "What's in this image?"
pi @code.ts @test.ts "Review these files"
```

## Package Commands

```bash
pi install <source>              # Install package
pi install npm:@foo/pi-tools     # From npm
pi install git:github.com/user/repo  # From git
pi install https://...          # From URL
pi install <source> -l           # Project-local install
pi remove <source>               # Remove package
pi uninstall <source>            # Alias for remove
pi list                          # List installed packages
pi update                        # Update packages (skips pinned)
pi config                        # Enable/disable package resources
```

## Examples

### Interactive

```bash
# Interactive with initial prompt
pi "List all .ts files in src/"

# With specific model
pi --model openai/gpt-4o "Help me refactor"

# With thinking level
pi --thinking high "Solve this complex problem"

# Read-only mode
pi --tools read,grep,find,ls -p "Review the code"
```

### Non-interactive

```bash
# Print mode
pi -p "Summarize this codebase"

# With piped stdin
cat README.md | pi -p "Summarize this text"

# Different model
pi --provider openai --model gpt-4o "Help me refactor"

# Model with provider prefix
pi --model openai/gpt-4o "Help me refactor"

# Model with thinking level shorthand
pi --model sonnet:high "Solve this complex problem"
```

## Keyboard Shortcuts

Press `/hotkeys` to see the full list. Key format: `modifier+key` (e.g., `ctrl+shift+x`)

| Key | Action |
|-----|--------|
| `Ctrl+C` | Clear editor |
| `Ctrl+C` twice | Quit |
| `Escape` | Cancel/abort |
| `Escape` twice | Open `/tree` |
| `Ctrl+L` | Open model selector |
| `Ctrl+P` / `Shift+Ctrl+P` | Cycle scoped models forward/backward |
| `Shift+Tab` | Cycle thinking level |
| `Ctrl+O` | Collapse/expand tool output |
| `Ctrl+T` | Collapse/expand thinking blocks |

For full reference, see [07-keybindings](../07-keybindings/README.md).

## Message Queue

Submit messages while the agent is working:

| Key | Action |
|-----|--------|
| `Enter` | Queue a *steering* message (delivered after current turn) |
| `Alt+Enter` | Queue a *follow-up* message (delivered after all work done) |
| `Escape` | Abort and restore queued messages to editor |
| `Alt+Up` | Retrieve queued messages back to editor |

**Modes** (configurable in settings):
- `steeringMode`: `"one-at-a-time"` (default) or `"all"`
- `followUpMode`: `"one-at-a-time"` (default) or `"all"`

**Note:** On Windows Terminal, `Alt+Enter` is fullscreen by default. Remap it in terminal settings.
