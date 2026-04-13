# Themes

Themes là file JSON định nghĩa màu cho TUI của pi.

## Built-in themes

- `dark`
- `light`

## Vị trí load theme

- `~/.pi/agent/themes/*.json`
- `.pi/themes/*.json`
- package `themes/`
- `settings.json` → `themes`
- CLI `--theme <path>`

## Chọn theme

```json
{
  "theme": "my-theme"
}
```

Hoặc chọn qua `/settings`.

## Hot reload

Khi chỉnh file theme đang active, pi tự reload ngay.

## Theme format

- `name`
- `vars` (optional)
- `colors` (bắt buộc đủ 51 tokens)
- `$schema` (khuyên dùng)

## Các nhóm token

- Core UI (11)
- Backgrounds & Content (11)
- Markdown (10)
- Tool Diffs (3)
- Syntax Highlighting (9)
- Thinking Level Borders (6)
- Bash Mode (1)

## Kiểu giá trị màu

- hex: `#ff0000`
- 256-color: `39`
- variable: `"primary"`
- default terminal color: `""`

Xem bản đầy đủ: [../../05-themes/README.md](../../05-themes/README.md)
