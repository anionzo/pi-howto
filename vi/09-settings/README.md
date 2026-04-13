# Settings

Pi dùng file JSON cho settings.

## Vị trí file

| File | Scope |
|------|-------|
| `~/.pi/agent/settings.json` | Global |
| `.pi/settings.json` | Project override |

Project settings override global settings và nested objects được merge.

## Các nhóm setting chính

### Model & Thinking
- `defaultProvider`
- `defaultModel`
- `defaultThinkingLevel`
- `hideThinkingBlock`
- `thinkingBudgets`

### UI & Display
- `theme`
- `quietStartup`
- `collapseChangelog`
- `doubleEscapeAction`
- `treeFilterMode`
- `editorPaddingX`
- `autocompleteMaxVisible`
- `showHardwareCursor`

### Compaction
- `compaction.enabled`
- `compaction.reserveTokens`
- `compaction.keepRecentTokens`

### Retry
- `retry.enabled`
- `retry.maxRetries`
- `retry.baseDelayMs`
- `retry.maxDelayMs`

### Message Delivery
- `steeringMode`
- `followUpMode`
- `transport`

### Terminal & Images
- `terminal.showImages`
- `terminal.clearOnShrink`
- `images.autoResize`
- `images.blockImages`

### Shell
- `shellPath`
- `shellCommandPrefix`
- `npmCommand`

### Resources
- `packages`
- `extensions`
- `skills`
- `prompts`
- `themes`
- `enableSkillCommands`

Xem bản đầy đủ: [../../09-settings/README.md](../../09-settings/README.md)
