# Themes

Themes are JSON files that define colors for the pi TUI. They control borders, message colors, markdown rendering, syntax highlighting, diff colors, and the editor border used to visualize thinking level.

## Built-in Themes

| Theme | Description |
|------|-------------|
| `dark` | Default dark theme |
| `light` | Default light theme |

Pi automatically picks a sensible default on first run based on your terminal background.

## Theme Locations

Pi loads themes from:

| Location | Scope |
|----------|-------|
| Built-in: `dark`, `light` | Bundled with pi |
| `~/.pi/agent/themes/*.json` | Global |
| `.pi/themes/*.json` | Project |
| Pi package | Shared via npm/git |
| `settings.json` `themes[]` | Additional paths |
| CLI `--theme <path>` | Explicit load |

Disable discovery with `--no-themes`.

## Selecting a Theme

### Via `/settings`

Open `/settings` and choose a theme interactively.

### Via `settings.json`

```json
{
  "theme": "my-theme"
}
```

## Hot Reload

When you edit the currently active custom theme file, pi reloads it automatically. This makes theme tuning much easier.

## Creating a Custom Theme

1. Create a file such as `~/.pi/agent/themes/my-theme.json`
2. Add `name`, optional `vars`, and all required color tokens
3. Select the theme in `/settings`
4. Tweak colors and watch pi hot-reload them

## Theme Format

```json
{
  "$schema": "https://raw.githubusercontent.com/badlogic/pi-mono/main/packages/coding-agent/src/modes/interactive/theme/theme-schema.json",
  "name": "my-theme",
  "vars": {
    "primary": "#00aaff",
    "secondary": 242
  },
  "colors": {
    "accent": "primary",
    "border": "primary",
    "borderAccent": "#00ffff",
    "borderMuted": "secondary",
    "success": "#00ff00",
    "error": "#ff0000",
    "warning": "#ffff00",
    "muted": "secondary",
    "dim": 240,
    "text": "",
    "thinkingText": "secondary",
    "selectedBg": "#2d2d30",
    "userMessageBg": "#2d2d30",
    "userMessageText": "",
    "customMessageBg": "#2d2d30",
    "customMessageText": "",
    "customMessageLabel": "primary",
    "toolPendingBg": "#1e1e2e",
    "toolSuccessBg": "#1e2e1e",
    "toolErrorBg": "#2e1e1e",
    "toolTitle": "primary",
    "toolOutput": "",
    "mdHeading": "#ffaa00",
    "mdLink": "primary",
    "mdLinkUrl": "secondary",
    "mdCode": "#00ffff",
    "mdCodeBlock": "",
    "mdCodeBlockBorder": "secondary",
    "mdQuote": "secondary",
    "mdQuoteBorder": "secondary",
    "mdHr": "secondary",
    "mdListBullet": "#00ffff",
    "toolDiffAdded": "#00ff00",
    "toolDiffRemoved": "#ff0000",
    "toolDiffContext": "secondary",
    "syntaxComment": "secondary",
    "syntaxKeyword": "primary",
    "syntaxFunction": "#00aaff",
    "syntaxVariable": "#ffaa00",
    "syntaxString": "#00ff00",
    "syntaxNumber": "#ff00ff",
    "syntaxType": "#00aaff",
    "syntaxOperator": "primary",
    "syntaxPunctuation": "secondary",
    "thinkingOff": "secondary",
    "thinkingMinimal": "primary",
    "thinkingLow": "#00aaff",
    "thinkingMedium": "#00ffff",
    "thinkingHigh": "#ff00ff",
    "thinkingXhigh": "#ff0000",
    "bashMode": "#ffaa00"
  }
}
```

### Required fields

- `name` - unique theme name
- `colors` - **all 51 required tokens**
- `vars` - optional reusable colors
- `$schema` - optional but recommended for editor completion and validation

## Color Tokens

Every theme must define all 51 tokens.

### Core UI (11)

| Token | Purpose |
|------|---------|
| `accent` | Primary accent, selected items, logo |
| `border` | Normal borders |
| `borderAccent` | Highlighted borders |
| `borderMuted` | Subtle borders |
| `success` | Success state |
| `error` | Error state |
| `warning` | Warning state |
| `muted` | Secondary text |
| `dim` | Tertiary text |
| `text` | Default terminal text |
| `thinkingText` | Thinking block text |

### Backgrounds & Content (11)

| Token | Purpose |
|------|---------|
| `selectedBg` | Selected row background |
| `userMessageBg` | User message background |
| `userMessageText` | User message text |
| `customMessageBg` | Extension message background |
| `customMessageText` | Extension message text |
| `customMessageLabel` | Extension label |
| `toolPendingBg` | Tool box while running |
| `toolSuccessBg` | Tool box success |
| `toolErrorBg` | Tool box error |
| `toolTitle` | Tool title |
| `toolOutput` | Tool output text |

### Markdown (10)

| Token | Purpose |
|------|---------|
| `mdHeading` | Headings |
| `mdLink` | Link text |
| `mdLinkUrl` | Link URL |
| `mdCode` | Inline code |
| `mdCodeBlock` | Code block text |
| `mdCodeBlockBorder` | Code block fences |
| `mdQuote` | Quote text |
| `mdQuoteBorder` | Quote border |
| `mdHr` | Horizontal rule |
| `mdListBullet` | List bullets |

### Tool Diffs (3)

| Token | Purpose |
|------|---------|
| `toolDiffAdded` | Added lines |
| `toolDiffRemoved` | Removed lines |
| `toolDiffContext` | Context lines |

### Syntax Highlighting (9)

| Token | Purpose |
|------|---------|
| `syntaxComment` | Comments |
| `syntaxKeyword` | Keywords |
| `syntaxFunction` | Function names |
| `syntaxVariable` | Variables |
| `syntaxString` | Strings |
| `syntaxNumber` | Numbers |
| `syntaxType` | Types |
| `syntaxOperator` | Operators |
| `syntaxPunctuation` | Punctuation |

### Thinking Level Borders (6)

| Token | Purpose |
|------|---------|
| `thinkingOff` | Thinking off |
| `thinkingMinimal` | Minimal thinking |
| `thinkingLow` | Low thinking |
| `thinkingMedium` | Medium thinking |
| `thinkingHigh` | High thinking |
| `thinkingXhigh` | Extra high thinking |

### Bash Mode (1)

| Token | Purpose |
|------|---------|
| `bashMode` | Editor border when command starts with `!` |

## Export Colors

Themes may also define an optional `export` section for `/export` HTML output:

```json
{
  "export": {
    "pageBg": "#18181e",
    "cardBg": "#1e1e24",
    "infoBg": "#3c3728"
  }
}
```

> **Tip:** If you frequently use `/share` to export sessions, define the `export` section so the HTML output matches your terminal theme visually.

## Color Value Formats

Four formats are supported.

| Format | Example | Meaning |
|--------|---------|---------|
| Hex | `"#ff0000"` | 24-bit RGB |
| 256-color | `39` | xterm 256-color palette index |
| Variable | `"primary"` | Reference into `vars` |
| Default | `""` | Terminal default color |

### `vars` example

```json
{
  "vars": {
    "blue": "#0066cc",
    "gray": 242
  },
  "colors": {
    "accent": "blue",
    "muted": "gray",
    "text": ""
  }
}
```

## Tips

- **Dark themes** usually work best with brighter, more saturated accents
- **Light themes** need stronger text contrast and darker borders
- Start from a consistent palette like Nord, Gruvbox, or Tokyo Night
- Test markdown, tool diffs, long wrapped output, and thinking blocks
- On VS Code terminals, set `terminal.integrated.minimumContrastRatio = 1` to get accurate colors. VS Code normally adjusts contrast automatically and overrides theme colors — this setting disables that override

## Using Themes in Extensions

If an extension renders markdown or custom UI, it can reuse pi theme information.

```typescript
import { getMarkdownTheme } from "@mariozechner/pi-coding-agent";

const mdTheme = getMarkdownTheme();
```
---

[← Extensions](../04-extensions/README.md) | [Table of Contents](../README.md) | [Sessions →](../06-sessions/README.md)

### See Also

- [Extensions](../04-extensions/README.md)
- [Settings](../09-settings/README.md)
