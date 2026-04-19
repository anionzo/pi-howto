# Settings

Pi uses JSON settings files. Project settings override global settings, and nested objects are merged rather than replaced wholesale.

## Settings Locations

| Location | Scope |
|----------|-------|
| `~/.pi/agent/settings.json` | Global |
| `.pi/settings.json` | Project override |

You can edit these files directly or use `/settings` for common interactive options.

## Model & Thinking

| Setting | Type | Description |
|---------|------|-------------|
| `defaultProvider` | string | Default provider name |
| `defaultModel` | string | Default model ID |
| `defaultThinkingLevel` | string | `off`, `minimal`, `low`, `medium`, `high`, `xhigh` |
| `hideThinkingBlock` | boolean | Hide visible thinking blocks |
| `thinkingBudgets` | object | Custom token budgets per thinking level |

Example:

```json
{
  "defaultProvider": "anthropic",
  "defaultModel": "claude-sonnet-4-20250514",
  "defaultThinkingLevel": "medium",
  "thinkingBudgets": {
    "minimal": 1024,
    "low": 4096,
    "medium": 10240,
    "high": 32768
  }
}
```

## UI & Display

| Setting | Type | Default | Description |
|---------|------|---------|-------------|
| `theme` | string | `dark` | Built-in or custom theme |
| `quietStartup` | boolean | `false` | Hide startup header |
| `collapseChangelog` | boolean | `false` | Show condensed changelog after update |
| `doubleEscapeAction` | string | `tree` | `tree`, `fork`, or `none` |
| `treeFilterMode` | string | `default` | Default `/tree` filter mode |
| `editorPaddingX` | number | `0` | Horizontal editor padding (0-3) |
| `autocompleteMaxVisible` | number | `5` | Max autocomplete rows (3-20) |
| `showHardwareCursor` | boolean | `false` | Show terminal cursor |

## Compaction

Compaction is enabled by default.

| Setting | Type | Default | Description |
|---------|------|---------|-------------|
| `compaction.enabled` | boolean | `true` | Enable auto-compaction |
| `compaction.reserveTokens` | number | `16384` | Reserve tokens for model response |
| `compaction.keepRecentTokens` | number | `20000` | Keep recent tokens unsummarized |

```json
{
  "compaction": {
    "enabled": true,
    "reserveTokens": 16384,
    "keepRecentTokens": 20000
  }
}
```

## Branch Summary

| Setting | Type | Default | Description |
|---------|------|---------|-------------|
| `branchSummary.reserveTokens` | number | `16384` | Reserve tokens for branch summaries |
| `branchSummary.skipPrompt` | boolean | `false` | Skip the summary prompt in `/tree` |

## Retry

| Setting | Type | Default | Description |
|---------|------|---------|-------------|
| `retry.enabled` | boolean | `true` | Retry transient failures |
| `retry.maxRetries` | number | `3` | Maximum retries |
| `retry.baseDelayMs` | number | `2000` | Initial backoff delay |
| `retry.maxDelayMs` | number | `60000` | Max accepted provider delay |

```json
{
  "retry": {
    "enabled": true,
    "maxRetries": 3,
    "baseDelayMs": 2000,
    "maxDelayMs": 60000
  }
}
```

## Message Delivery

These settings control how queued messages are delivered while the agent is working.

| Setting | Type | Default | Description |
|---------|------|---------|-------------|
| `steeringMode` | string | `one-at-a-time` | `one-at-a-time` or `all` |
| `followUpMode` | string | `one-at-a-time` | `one-at-a-time` or `all` |
| `transport` | string | `sse` | `sse`, `websocket`, or `auto` |

## Terminal & Images

| Setting | Type | Default | Description |
|---------|------|---------|-------------|
| `terminal.showImages` | boolean | `true` | Render images in terminal when supported |
| `terminal.clearOnShrink` | boolean | `false` | Clear empty rows on shrink |
| `images.autoResize` | boolean | `true` | Resize images before sending |
| `images.blockImages` | boolean | `false` | Block all images from the LLM context |

## Shell

| Setting | Type | Description |
|---------|------|-------------|
| `shellPath` | string | Override shell binary |
| `shellCommandPrefix` | string | Prefix added to every bash command |
| `npmCommand` | string[] | Wrapper command for npm operations |

### `shellCommandPrefix` example

Useful in Docker or devcontainer setups — every bash tool call gets this prefix:

```json
{
  "shellCommandPrefix": "docker exec my-container"
}
```

### `npmCommand` example

Override how pi runs npm. Common use case: using `nvm`, `fnm`, or `mise` to pin a Node version:

```json
{
  "npmCommand": ["mise", "exec", "node@20", "--", "npm"]
}
```

```json
{
  "npmCommand": ["fnm", "exec", "--using=20", "npm"]
}
```

## Sessions

| Setting | Type | Description |
|---------|------|-------------|
| `sessionDir` | string | Custom session storage directory |

```json
{
  "sessionDir": ".pi/sessions"
}
```

`--session-dir` on the CLI overrides this setting.

## Model Cycling

| Setting | Type | Description |
|---------|------|-------------|
| `enabledModels` | string[] | Model patterns used by `Ctrl+P` cycling |

```json
{
  "enabledModels": ["claude-*", "gpt-4o", "gemini-2*"]
}
```

## Markdown

| Setting | Type | Default | Description |
|---------|------|---------|-------------|
| `markdown.codeBlockIndent` | string | `"  "` | Indentation for rendered code blocks |

## Resource Loading

These settings define where pi loads resources from.

| Setting | Type | Description |
|---------|------|-------------|
| `packages` | array | Packages to load resources from |
| `extensions` | string[] | Local extensions |
| `skills` | string[] | Local skills |
| `prompts` | string[] | Local prompt templates |
| `themes` | string[] | Local themes |
| `enableSkillCommands` | boolean | Register `/skill:name` commands |

### Path resolution rules

- paths in `~/.pi/agent/settings.json` resolve relative to `~/.pi/agent`
- paths in `.pi/settings.json` resolve relative to `.pi`
- absolute paths and `~` are supported
- arrays support glob patterns and exclusions
- use `!pattern` to exclude
- use `+path` to force include an exact path
- use `-path` to force exclude an exact path

### Package filtering example

```json
{
  "packages": [
    "pi-skills",
    {
      "source": "npm:my-package",
      "extensions": ["extensions/*.ts", "!extensions/legacy.ts"],
      "skills": [],
      "prompts": ["prompts/review.md"],
      "themes": ["+themes/legacy.json"]
    }
  ]
}
```

## Example Full Config

```json
{
  "defaultProvider": "anthropic",
  "defaultModel": "claude-sonnet-4-20250514",
  "defaultThinkingLevel": "medium",
  "theme": "dark",
  "compaction": {
    "enabled": true,
    "reserveTokens": 16384,
    "keepRecentTokens": 20000
  },
  "retry": {
    "enabled": true,
    "maxRetries": 3
  },
  "enabledModels": ["claude-*", "gpt-4o"],
  "packages": ["pi-skills"]
}
```

## Merge Behavior

Project settings override global settings. Nested objects are merged.

```json
// ~/.pi/agent/settings.json
{
  "theme": "dark",
  "compaction": { "enabled": true, "reserveTokens": 16384 }
}

// .pi/settings.json
{
  "compaction": { "reserveTokens": 8192 }
}
```

Result:

```json
{
  "theme": "dark",
  "compaction": { "enabled": true, "reserveTokens": 8192 }
}
```

## Applying Changes

- theme changes often apply immediately
- many other settings require `/reload`
- some session/model choices only affect new turns

## Related

- [05-themes](../05-themes/README.md)
- [08-providers](../08-providers/README.md)
- [10-pi-packages](../10-pi-packages/README.md)
