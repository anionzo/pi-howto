# Headless Modes

Pi has four operating modes. Interactive mode is the default TUI experience. The other three — print, JSON, and RPC — run without the interactive interface, making pi usable in scripts, CI pipelines, and as a subprocess in other applications.

## The Four Modes

| Mode | Flag | Use case |
|------|------|----------|
| Interactive | *(default)* | Normal TUI usage |
| Print | `-p "query"` | One-shot text output to stdout |
| JSON | `--mode json` | Stream all events as JSONL |
| RPC | `--mode rpc` | JSON protocol via stdin/stdout for non-Node integrations |

There is also an **SDK mode** for embedding pi directly in Node.js applications.

## Print Mode

Print mode sends a query, prints the text response to stdout, then exits. Ideal for shell scripts and piping.

```bash
pi -p "Summarize this file" < README.md
pi -p "What does this function do?" --model openai/gpt-4o
pi -p "List all TODO comments" --no-session
```

- `ctx.hasUI` is `false` in print mode
- No session is saved by default unless `--session` is specified
- Output goes to stdout; errors go to stderr

### Combining with other tools

```bash
# Pipe output to clipboard
pi -p "Explain this error" | pbcopy

# Use in a script
SUMMARY=$(pi -p "Summarize changes" < CHANGELOG.md)
echo "$SUMMARY"
```

## JSON Mode

JSON mode streams all events as newline-delimited JSON (JSONL) to stdout. Use this when you need structured access to tool calls, thinking blocks, and other internal events.

```bash
pi -p "query" --mode json
pi -p "query" --mode json --no-session
```

- Each line is a complete JSON object representing an event
- `ctx.hasUI` is `false` in JSON mode
- Useful for building custom UIs or logging pipelines

## RPC Mode

RPC mode turns pi into a subprocess that communicates via JSON protocol over stdin/stdout. This is the primary integration path for non-Node applications.

```bash
pi --mode rpc --no-session
```

- Reads JSON commands from stdin
- Writes JSON events to stdout
- `ctx.hasUI` is `true` in RPC mode (UI requests are emitted as RPC events for the host to handle)

### JSONL Framing

RPC uses **strict LF-only JSONL framing**. Clients must split on `\n` only. Do not use generic `readline` implementations that also split on Unicode line separators `U+2028` and `U+2029`, as this will cause parsing errors with certain model outputs.

### Real-world Example

[OpenClaw](https://shittycodingagent.ai) is a real-world integration that uses RPC mode to embed pi in a web application.

### Reference

Full RPC protocol documentation: [`docs/rpc.md`](https://github.com/badlogic/pi-mono/blob/main/packages/coding-agent/docs/rpc.md)

## SDK Mode

For Node.js applications, pi exposes programmatic APIs to create sessions and run in any mode without spawning a subprocess.

```typescript
import {
  createAgentSession,
  runPrintMode,
  runRpcMode
} from "@mariozechner/pi-coding-agent";
```

### `runPrintMode()`

Run a one-shot query programmatically:

```typescript
await runPrintMode({
  prompt: "Summarize this codebase",
  model: "anthropic/claude-sonnet-4-20250514",
  cwd: "/path/to/project"
});
```

### `createAgentSession()`

Create a full agent session for custom integrations:

```typescript
const session = await createAgentSession({
  cwd: "/path/to/project",
  sessionDir: "/tmp/sessions"
});
```

### Reference

Full SDK documentation: [`docs/sdk.md`](https://github.com/badlogic/pi-mono/blob/main/packages/coding-agent/docs/sdk.md)

## `ctx.hasUI` by Mode

| Mode | `ctx.hasUI` | Meaning |
|------|-------------|---------|
| Interactive | `true` | Full TUI available |
| RPC | `true` | UI requests emitted as RPC events |
| Print | `false` | No UI — stdout only |
| JSON | `false` | No UI — JSONL stream only |

Extension authors should check `ctx.hasUI` before calling UI methods like `ctx.ui.notify()` or `ctx.ui.confirm()`. These are no-ops when `hasUI` is `false`.

## Common Patterns

### CI pipeline

```bash
# Run in CI without saving sessions
pi --no-session -p "Review this PR for security issues"
```

### Custom automation

```bash
# Process multiple files
for f in src/*.ts; do
  pi --no-session -p "Check $f for type errors" --mode json >> results.jsonl
done
```

### Subprocess integration

```bash
# Start as RPC subprocess
pi --mode rpc --no-session --model anthropic/claude-sonnet-4-20250514
```
---

[← Prompt Templates](../11-prompt-templates/README.md) | [Table of Contents](../README.md)

### See Also

- [Commands](../01-commands/README.md)
- [Extensions](../04-extensions/README.md)
- [Sessions](../06-sessions/README.md)
