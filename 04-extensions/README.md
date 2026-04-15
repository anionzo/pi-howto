# Extensions

Extensions are TypeScript modules that extend pi's behavior. They can subscribe to lifecycle events, register custom tools callable by the LLM, add commands, shortcuts, flags, and render custom UI.

## What Extensions Can Do

Extensions are the main escape hatch when the built-in harness is not enough.

- Register **custom tools** with `pi.registerTool()`
- Intercept or block **tool calls**
- Add **slash commands** such as `/deploy` or `/review`
- Register **keyboard shortcuts** and **CLI flags**
- Hook into **session**, **agent**, **tool**, and **model** lifecycle events
- Build **custom UI** with notifications, dialogs, widgets, overlays, and TUI components
- Persist **extension state** across restarts
- Add **custom providers** or override provider configuration
- Implement workflows such as plan mode, permission gates, git checkpointing, remote execution, and more

## Quick Start

Create `~/.pi/agent/extensions/my-extension.ts`:

```typescript
import type { ExtensionAPI } from "@mariozechner/pi-coding-agent";
import { Type } from "@sinclair/typebox";

export default function (pi: ExtensionAPI) {
  pi.on("session_start", async (_event, ctx) => {
    ctx.ui.notify("Extension loaded", "info");
  });

  pi.on("tool_call", async (event, _ctx) => {
    if (event.toolName === "bash" && event.input.command?.includes("rm -rf")) {
      return { block: true, reason: "Blocked dangerous command" };
    }
  });

  pi.registerTool({
    name: "greet",
    label: "Greet",
    description: "Greet someone by name",
    parameters: Type.Object({
      name: Type.String({ description: "Name to greet" })
    }),
    async execute(_toolCallId, params) {
      return {
        content: [{ type: "text", text: `Hello, ${params.name}!` }],
        details: {}
      };
    }
  });

  pi.registerCommand("hello", {
    description: "Say hello",
    handler: async (args, ctx) => {
      ctx.ui.notify(`Hello ${args || "world"}!`, "success");
    }
  });
}
```

Test it with:

```bash
# one-off load from the exact file path
pi -e ~/.pi/agent/extensions/my-extension.ts

# or just start pi normally (auto-discovers that location)
pi
# then use /reload after edits
```

## Extension Locations

> **Security:** Extensions execute arbitrary code with your full system permissions. Only install extensions you trust.

| Location | Scope | Reload behavior |
|----------|-------|-----------------|
| `~/.pi/agent/extensions/*.ts` | Global | Reloadable with `/reload` |
| `~/.pi/agent/extensions/*/index.ts` | Global | Reloadable with `/reload` |
| `.pi/extensions/*.ts` | Project | Reloadable with `/reload` |
| `.pi/extensions/*/index.ts` | Project | Reloadable with `/reload` |
| `--extension <path>` / `-e` | Current run only | Good for quick tests |
| Pi package | Shared via npm/git | Managed by package config |

Additional paths can be declared in `settings.json`:

```json
{
  "extensions": [
    "/path/to/local/extension.ts",
    "/path/to/local/extension-dir"
  ],
  "packages": [
    "npm:@foo/pi-tools@1.0.0",
    "git:github.com/user/repo@v1"
  ]
}
```

## Available Imports

| Package | Purpose |
|---------|---------|
| `@mariozechner/pi-coding-agent` | Extension types and helpers |
| `@sinclair/typebox` | Tool parameter schemas |
| `@mariozechner/pi-ai` | AI helpers such as `StringEnum` |
| `@mariozechner/pi-tui` | TUI components for custom UI |
| Node built-ins | `node:fs`, `node:path`, `node:child_process`, etc. |

Extensions are loaded through `jiti`, so TypeScript works without a build step.

## Extension Styles

### 1. Single file

Best for small extensions.

```text
~/.pi/agent/extensions/
└── my-extension.ts
```

### 2. Directory with `index.ts`

Best for multi-file extensions.

```text
~/.pi/agent/extensions/
└── my-extension/
    ├── index.ts
    ├── tools.ts
    └── utils.ts
```

### 3. Package with dependencies

Best when you need npm dependencies.

```text
~/.pi/agent/extensions/
└── my-extension/
    ├── package.json
    ├── node_modules/
    └── src/
        └── index.ts
```

```json
{
  "name": "my-extension",
  "dependencies": {
    "chalk": "^5.0.0"
  },
  "pi": {
    "extensions": ["./src/index.ts"]
  }
}
```

## Lifecycle Overview

```text
startup
  ├─ session_start
  ├─ resources_discover
  └─ user prompt
       ├─ input
       ├─ before_agent_start
       ├─ agent_start
       ├─ turn_start
       ├─ context
       ├─ before_provider_request
       ├─ tool_execution_start
       ├─ tool_call
       ├─ tool_result
       ├─ tool_execution_end
       ├─ turn_end
       └─ agent_end
```

## Important Events

### Resource Events

| Event | Use case |
|------|----------|
| `resources_discover` | Contribute additional skill, prompt, or theme paths |

### Session Events

| Event | Use case |
|------|----------|
| `session_start` | Reconstruct extension state when session starts or reloads |
| `session_before_switch` | Confirm `/new` or `/resume` |
| `session_before_fork` | Control `/fork` behavior |
| `session_before_compact` | Cancel or customize compaction |
| `session_compact` | React after compaction |
| `session_before_tree` | Customize or block `/tree` navigation |
| `session_tree` | React after branch switch |
| `session_shutdown` | Cleanup resources or persist state |

### Agent Events

| Event | Use case |
|------|----------|
| `input` | Transform raw input before skill/template expansion |
| `before_agent_start` | Inject messages or modify system prompt |
| `agent_start` / `agent_end` | Track a whole user prompt lifecycle |
| `turn_start` / `turn_end` | Observe each LLM turn |
| `message_start` / `message_update` / `message_end` | Handle message lifecycle |
| `context` | Filter or modify messages before provider call |
| `before_provider_request` | Inspect or patch provider payload |
| `model_select` | Respond to model changes |

### Tool Events

| Event | Use case |
|------|----------|
| `tool_execution_start` | Observe tool execution lifecycle |
| `tool_call` | Inspect, mutate, or block tool inputs |
| `tool_result` | Patch result output before it reaches the UI/LLM |
| `tool_execution_end` | Observe final completion |
| `user_bash` | Intercept `!cmd` and `!!cmd` |

## Extension API Methods

### `pi.registerTool()`

Register a custom tool callable by the LLM.

```typescript
import { Type } from "@sinclair/typebox";

pi.registerTool({
  name: "my_tool",
  label: "My Tool",
  description: "Summarize a text",
  parameters: Type.Object({
    text: Type.String()
  }),
  async execute(_toolCallId, params, signal, onUpdate, ctx) {
    onUpdate?.({
      content: [{ type: "text", text: "Working..." }],
      details: { progress: 50 }
    });

    return {
      content: [{ type: "text", text: params.text.toUpperCase() }],
      details: {}
    };
  }
});
```

Useful options:
- `promptSnippet` - one-line tool entry in the default system prompt
- `promptGuidelines` - extra bullets added to prompt guidelines when tool is active
- `prepareArguments()` - compatibility shim before schema validation
- `renderCall()` / `renderResult()` - custom UI rendering

### `pi.registerCommand()`

Add a custom slash command.

```typescript
pi.registerCommand("stats", {
  description: "Show session statistics",
  handler: async (_args, ctx) => {
    const count = ctx.sessionManager.getEntries().length;
    ctx.ui.notify(`${count} entries`, "info");
  }
});
```

### `pi.registerShortcut()`

```typescript
pi.registerShortcut("ctrl+shift+p", {
  description: "Toggle custom mode",
  handler: async (ctx) => {
    ctx.ui.notify("Toggled", "info");
  }
});
```

### `pi.registerFlag()`

```typescript
pi.registerFlag("plan", {
  description: "Start with plan mode enabled",
  type: "boolean",
  default: false
});
```

### `pi.sendMessage()` and `pi.sendUserMessage()`

Use these to inject custom messages or synthetic user messages.

```typescript
pi.sendMessage({
  customType: "my-extension",
  content: "Extra context",
  display: true
}, { deliverAs: "steer", triggerTurn: true });

pi.sendUserMessage("Focus on error handling", { deliverAs: "followUp" });
```

### `pi.appendEntry()`

Persist extension state in the session file.

```typescript
pi.appendEntry("my-state", { count: 42 });
```

### Tool and model helpers

- `pi.getActiveTools()` / `pi.getAllTools()` / `pi.setActiveTools()`
- `pi.setModel(model)`
- `pi.getThinkingLevel()` / `pi.setThinkingLevel(level)`
- `pi.registerProvider(name, config)`
- `pi.unregisterProvider(name)`

## Extension Context

Most handlers receive `ctx: ExtensionContext`.

### Useful fields and methods

| Field / method | Purpose |
|----------------|---------|
| `ctx.ui` | Notifications, selects, confirms, custom UI |
| `ctx.hasUI` | Whether the current mode supports UI |
| `ctx.cwd` | Current working directory |
| `ctx.sessionManager` | Read-only session access |
| `ctx.modelRegistry` / `ctx.model` | Model access |
| `ctx.signal` | Abort signal for nested async work |
| `ctx.getContextUsage()` | Estimate current context usage |
| `ctx.compact()` | Trigger compaction |
| `ctx.abort()` | Abort current work |
| `ctx.shutdown()` | Graceful shutdown |

Command handlers receive `ExtensionCommandContext`, which also includes:
- `ctx.waitForIdle()`
- `ctx.newSession()`
- `ctx.fork()`
- `ctx.navigateTree()`
- `ctx.switchSession()`
- `ctx.reload()`

## Custom UI

Extensions can build UI ranging from simple notifications to full TUI components.

### Basic UI methods

```typescript
ctx.ui.notify("Saved", "success");
const ok = await ctx.ui.confirm("Dangerous action", "Continue?");
const value = await ctx.ui.input("Project name", "Enter a value");
const choice = await ctx.ui.select("Pick one", ["dev", "staging", "prod"]);
```

### Status lines and widgets

```typescript
ctx.ui.setStatus("deploy", "● deploying");
ctx.ui.setWidget("deploy", ["Build: done", "Deploy: pending"]);
ctx.ui.setTitle("Custom Mode");
```

### Full custom component

```typescript
ctx.ui.custom(myComponent);
```

Use `@mariozechner/pi-tui` when you need full rendering control.

## State Management

For branch-safe state, store canonical state in tool result `details` and reconstruct it on `session_start`.

```typescript
export default function (pi: ExtensionAPI) {
  let items: string[] = [];

  pi.on("session_start", async (_event, ctx) => {
    items = [];
    for (const entry of ctx.sessionManager.getBranch()) {
      if (entry.type === "message" && entry.message.role === "toolResult") {
        if (entry.message.toolName === "todo_tool") {
          items = entry.message.details?.items ?? [];
        }
      }
    }
  });
}
```

## Best Practices

### Keep auto-reload friendly

For normal development, put extensions in auto-discovered locations and use `/reload` instead of restarting pi.

### Use the file mutation queue for mutating tools

If your custom tool edits files, use `withFileMutationQueue()` so it cooperates with built-in `edit` and `write` during parallel tool execution.

### Respect cancellation

Use `ctx.signal` and abort-aware APIs such as `fetch(..., { signal: ctx.signal })`.

### Prefer additive session state

Store state in session entries rather than hidden globals so branching and reloads remain correct.

## Example Use Cases

- Permission gates for `rm -rf`, `sudo`, or production deploys
- Path protection for `.env`, `node_modules`, or generated files
- Custom compaction and summarization strategies
- Todo systems or plan mode
- Browser automation or CI triggers
- Remote command execution via SSH
- Games while waiting (`snake.ts`, Doom, etc.)

## Related

- [03-skills](../03-skills/README.md)
- [05-themes](../05-themes/README.md)
- [06-sessions](../06-sessions/README.md)
- Official examples: `examples/extensions/` in the pi repository
