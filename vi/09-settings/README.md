# Cài đặt

Pi dùng file JSON cho cài đặt. Cài đặt theo project sẽ ghi đè cài đặt toàn cục, còn object lồng nhau sẽ được merge thay vì thay toàn bộ.

## Vị trí file

| File | Phạm vi |
|------|---------|
| `~/.pi/agent/settings.json` | Toàn cục |
| `.pi/settings.json` | Ghi đè theo project |

Bạn có thể sửa trực tiếp hoặc dùng `/settings` cho các tùy chọn thường gặp.

## Các nhóm cài đặt chính

### Model và thinking
- `defaultProvider`
- `defaultModel`
- `defaultThinkingLevel`
- `hideThinkingBlock`
- `thinkingBudgets`

### Giao diện và hiển thị
- `theme`
- `quietStartup`
- `collapseChangelog`
- `doubleEscapeAction`
- `treeFilterMode`
- `editorPaddingX`
- `autocompleteMaxVisible`
- `showHardwareCursor`

### Compaction và branch summary
- `compaction.enabled`
- `compaction.reserveTokens`
- `compaction.keepRecentTokens`
- `branchSummary.reserveTokens`
- `branchSummary.skipPrompt`

### Retry và message delivery
- `retry.enabled`
- `retry.maxRetries`
- `retry.baseDelayMs`
- `retry.maxDelayMs`
- `steeringMode`
- `followUpMode`
- `transport`

### Terminal, ảnh và shell
- `terminal.showImages`
- `terminal.clearOnShrink`
- `images.autoResize`
- `images.blockImages`
- `shellPath`
- `shellCommandPrefix`
- `npmCommand`

### Session và tài nguyên
- `sessionDir`
- `enabledModels`
- `packages`
- `extensions`
- `skills`
- `prompts`
- `themes`
- `enableSkillCommands`

## Ví dụ cấu hình

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
  "enabledModels": ["claude-*", "gpt-4o"]
}
```

## Quy tắc merge

Ví dụ:

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

Kết quả thực tế:

```json
{
  "theme": "dark",
  "compaction": { "enabled": true, "reserveTokens": 8192 }
}
```

## Áp dụng thay đổi

- nhiều thay đổi sẽ có hiệu lực sau `/reload`
- theme thường cập nhật ngay
- một số lựa chọn chỉ ảnh hưởng tới các lượt làm việc mới

## Đọc tiếp

- [05-themes](../05-themes/README.md)
- [08-providers](../08-providers/README.md)
- [10-pi-packages](../10-pi-packages/README.md)
- Bản đầy đủ tiếng Anh: [../../09-settings/README.md](../../09-settings/README.md)
