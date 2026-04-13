# Sessions

Pi stores sessions as JSONL (JSON Lines) files with a tree structure. Every entry is an object with a `type`, and most entries also carry `id` and `parentId` so pi can branch in place without splitting history across multiple files.

## Session Management

### CLI Options

```bash
pi -c, --continue         # Continue most recent session
pi -r, --resume           # Browse and select previous sessions
pi --no-session           # Ephemeral mode (do not save)
pi --session <path>       # Open a specific session file or partial UUID
pi --fork <path>          # Fork an existing session file or partial UUID
pi --session-dir <dir>    # Override session directory
```

### In-app Commands

| Command | Purpose |
|---------|---------|
| `/new` | Start a new session |
| `/resume` | Pick from previous sessions |
| `/session` | Show current session path, tokens, and cost |
| `/tree` | Navigate the current session tree |
| `/fork` | Create a new session from the current branch |
| `/compact [prompt]` | Compact context manually |
| `/export [file]` | Export to HTML |
| `/share` | Upload as a private GitHub gist |

## File Location

Session files live under:

```text
~/.pi/agent/sessions/--<path>--/<timestamp>_<uuid>.jsonl
```

`<path>` is derived from the working directory with slashes replaced.

Example idea:

```text
~/.pi/agent/sessions/
└── --d-code-pidoc--/
    └── 2026-04-13T18-30-00_abcd1234.jsonl
```

You can override storage with:
- `--session-dir <dir>`
- `sessionDir` in `settings.json`

CLI flag wins over settings.

## Session Versions

Pi understands and migrates older sessions automatically.

| Version | Meaning |
|---------|---------|
| v1 | Linear entry sequence |
| v2 | Tree structure via `id` / `parentId` |
| v3 | `hookMessage` renamed to `custom` |

## Tree Model

Entries form a tree:
- the first entry has `parentId: null`
- each later entry points to its parent
- `/tree` can move the leaf to any earlier point
- new messages from that point create a new branch

```text
[user] ─ [assistant] ─ [user] ─ [assistant] ─┬─ [user] ← current leaf
                                             └─ [branch_summary] ─ [user]
```

## Entry Types

### Header

The first line is a session header, not a normal tree entry.

```json
{"type":"session","version":3,"id":"uuid","timestamp":"2024-12-03T14:00:00.000Z","cwd":"/path/to/project"}
```

### Message entry

A normal conversation message.

```json
{"type":"message","id":"a1b2c3d4","parentId":"prev1234","timestamp":"2024-12-03T14:00:01.000Z","message":{"role":"user","content":"Hello"}}
```

### Other important entry types

| Entry type | Purpose |
|------------|---------|
| `model_change` | User changed models |
| `thinking_level_change` | Thinking level changed |
| `compaction` | Older context summarized |
| `branch_summary` | Summary of abandoned branch during `/tree` |
| `custom` | Extension state, not sent to the LLM |
| `custom_message` | Extension message that *is* sent to the LLM |
| `label` | User bookmark/marker |
| `session_info` | Session display name |

## Message Types

Sessions store several message roles.

| Role | Purpose |
|------|---------|
| `user` | User input |
| `assistant` | Assistant response |
| `toolResult` | Result of a tool call |
| `bashExecution` | User `!cmd` / `!!cmd` execution |
| `custom` | Extension-defined visible or hidden message |
| `branchSummary` | Branch summary message |
| `compactionSummary` | Compaction summary message |

### Content blocks

Assistant and user messages can contain typed blocks:
- text
- image
- thinking
- toolCall

## `/tree` Navigation

`/tree` lets you move around the session graph without creating a new file.

### Features

- search by typing
- fold or unfold branch segments
- navigate with `Ctrl+←` / `Ctrl+→` or `Alt+←` / `Alt+→`
- page through entries with `←` / `→`
- add labels with `Shift+L`
- toggle label timestamps with `Shift+T`
- cycle filters with `Ctrl+O`

### Filter modes

`Ctrl+O` cycles through:
- `default`
- `no-tools`
- `user-only`
- `labeled-only`
- `all`

## `/fork`

`/fork` creates a **new session file** from the current branch.

Workflow:
1. choose a point to fork from
2. pi copies the relevant history into a new session
3. the selected message is placed in the editor so you can modify and continue from there

CLI form:

```bash
pi --fork <path-or-id>
```

This can fork an existing session file or a partial session UUID directly.

## Compaction

Long sessions eventually approach context limits. Compaction summarizes older messages while preserving enough recent context to continue work.

### Manual compaction

```bash
/compact
/compact Focus on recent code changes and unresolved bugs
```

### Automatic compaction

Enabled by default. It triggers when:
- context overflow happens and pi needs to recover
- pi is approaching the model context window and compacts proactively

Configure it in `/settings` or `settings.json`:

```json
{
  "compaction": {
    "enabled": true,
    "reserveTokens": 16384,
    "keepRecentTokens": 20000
  }
}
```

### Important note

Compaction is **lossy for prompt context**, but the full raw history still remains in the JSONL session file. Use `/tree` if you need to revisit older branches or pre-compaction points.

## Export & Share

### Export to HTML

```bash
/export
/export report.html
```

### Share via private gist

```bash
/share
```

Pi uploads the exported HTML to a private GitHub gist and returns a shareable link.

## Session Names

Set a display name with:

```bash
/name refactor-auth-module
```

This creates a `session_info` entry and changes how the session appears in `/resume`.

## Deleting Sessions

You can:
- remove the `.jsonl` file manually from the sessions directory
- delete from `/resume` using `Ctrl+D`

When available, pi uses the `trash` CLI for safer deletion instead of permanent removal.

## SessionManager API

For SDK and extension work, `SessionManager` is the main API.

### Static creation methods

- `SessionManager.create(cwd, sessionDir?)`
- `SessionManager.open(path, sessionDir?)`
- `SessionManager.continueRecent(cwd, sessionDir?)`
- `SessionManager.inMemory(cwd?)`
- `SessionManager.forkFrom(sourcePath, targetCwd, sessionDir?)`

### Listing methods

- `SessionManager.list(cwd, sessionDir?, onProgress?)`
- `SessionManager.listAll(onProgress?)`

### Useful instance methods

- `appendMessage(message)`
- `appendModelChange(provider, modelId)`
- `appendThinkingLevelChange(level)`
- `appendCompaction(...)`
- `appendCustomEntry(customType, data?)`
- `appendSessionInfo(name)`
- `appendLabelChange(targetId, label)`
- `getEntries()`
- `getBranch(fromId?)`
- `getTree()`
- `getChildren(parentId)`
- `getLeafId()`
- `branch(entryId)`
- `buildSessionContext()`

## Related

- [07-keybindings](../07-keybindings/README.md)
- [09-settings](../09-settings/README.md)
- Official docs: `docs/session.md`, `docs/tree.md`, `docs/compaction.md`
