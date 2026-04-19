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

### How It Works

```
Before compaction:

  [msg1] [msg2] [msg3] [msg4] [msg5] [msg6] [msg7] [msg8]
   \___________________/  \_____________________________/
     summarized              kept (recent)
                             ↑
                    firstKeptEntryId

After compaction (what LLM sees):

  [system prompt] [summary] [msg4] [msg5] [msg6] [msg7] [msg8]
```

1. Pi walks backwards from newest message, accumulating tokens until `keepRecentTokens` is reached
2. Everything older is summarized by the LLM
3. The summary replaces old messages in context
4. Full raw history remains in the JSONL session file

### When It Triggers

**Automatic** (default on):
- When `contextTokens > contextWindow - reserveTokens`
- When approaching context limit proactively

**Manual:**

```bash
/compact
/compact Focus on recent code changes and unresolved bugs
```

The optional text focuses the summary on specific topics.

### Settings

```json
{
  "compaction": {
    "enabled": true,
    "reserveTokens": 16384,
    "keepRecentTokens": 20000
  }
}
```

| Setting | Default | Description |
|---------|---------|-------------|
| `enabled` | `true` | Enable auto-compaction |
| `reserveTokens` | `16384` | Tokens reserved for LLM response |
| `keepRecentTokens` | `20000` | Recent tokens to keep (not summarized) |

**Tuning tips:**
- For large outputs such as code generation, increase `reserveTokens` to `32768`
- For shorter sessions or smaller context windows, reduce `keepRecentTokens` to `10000`
- To disable auto-compaction, set `"enabled": false` (manual `/compact` still works)

### Summary Format

Compaction summaries follow a structured format:

```markdown
## Goal
[What the user is trying to accomplish]

## Progress
### Done
- [x] Completed tasks
### In Progress
- [ ] Current work

## Key Decisions
- **Decision**: Rationale

## Next Steps
1. What should happen next

<read-files>
path/to/file1.ts
</read-files>

<modified-files>
path/to/changed.ts
</modified-files>
```

This preserves: goals, progress, decisions, file context. The agent can continue seamlessly after compaction.

### Branch Summarization

When you use `/tree` to navigate to a different branch, pi offers to summarize the branch you're leaving. This injects context from the old branch into the new one.

```
         ┌─ B ─ C ─ D (leaving → summarized)
    A ───┤
         └─ E ─ F (navigating here)
```

Configure via settings:

```json
{
  "branchSummary": {
    "reserveTokens": 16384,
    "skipPrompt": false
  }
}
```

Set `skipPrompt: true` to skip the “Summarize branch?” dialog (defaults to no summary).

### Custom Compaction via Extensions

Extensions can intercept compaction via the `session_before_compact` event:

```typescript
pi.on("session_before_compact", async (event, ctx) => {
  // Cancel compaction
  return { cancel: true };

  // Or provide custom summary
  return {
    compaction: {
      summary: "Your custom summary...",
      firstKeptEntryId: event.preparation.firstKeptEntryId,
      tokensBefore: event.preparation.tokensBefore,
    }
  };
});
```

Similarly, `session_before_tree` intercepts branch summarization.

### Recovery

Compaction is **lossy for prompt context** but the full raw history remains in the JSONL session file. Use `/tree` to revisit older branches or pre-compaction points.

## CI & Team Environments

### Ephemeral sessions in CI

```bash
# In CI, disable session saving
pi --no-session -p "query"
```

### Team-shared session directory

Store sessions in the project directory and share via settings:

```json
{
  "sessionDir": ".pi/sessions"
}
```

Add `.pi/sessions/` to `.gitignore` to avoid committing session files.

### Compaction and `/tree` navigation

When using `/tree` to navigate to a point before a compaction occurred, the full raw JSONL history is visible. However, if you continue the conversation from that point, the LLM context is **rebuilt from the compaction summary**, not from the raw messages. The summary is what the model "remembers."

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
---

[← Themes](../05-themes/README.md) | [Table of Contents](../README.md) | [Keybindings →](../07-keybindings/README.md)

### See Also

- [Keybindings](../07-keybindings/README.md)
- [Settings](../09-settings/README.md)
