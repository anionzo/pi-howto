# Memory & Context Files

Phần này giải thích cách pi nạp instruction và quản lý context.

## “Memory” trong pi là gì?

Trong pi, memory chủ yếu gồm:

1. **Instruction files** (định hướng hành vi agent)
2. **Session history** (lịch sử hội thoại)
3. **Runtime settings** (cấu hình load tài nguyên + hành vi)

## Instruction Files

### `AGENTS.md` / `CLAUDE.md`

Pi load `AGENTS.md` (hoặc `CLAUDE.md`) theo thứ tự:

1. `~/.pi/agent/AGENTS.md` (global)
2. các thư mục cha (đi ngược từ thư mục hiện tại)
3. thư mục hiện tại

Các file khớp sẽ được gộp vào context.

Dùng cho:
- conventions của repo
- coding standards
- ghi chú kiến trúc
- quy tắc an toàn
- lệnh thường dùng

### `SYSTEM.md`

Dùng để **thay thế** default system prompt.

| Path | Scope |
|------|-------|
| `~/.pi/agent/SYSTEM.md` | Global |
| `.pi/SYSTEM.md` | Project |

### `APPEND_SYSTEM.md`

Dùng để **nối thêm** vào default system prompt.

| Path | Scope |
|------|-------|
| `~/.pi/agent/APPEND_SYSTEM.md` | Global |
| `.pi/APPEND_SYSTEM.md` | Project |

Thường an toàn hơn `SYSTEM.md` vì không ghi đè toàn bộ prompt gốc.

## Runtime Settings

| Path | Scope |
|------|-------|
| `~/.pi/agent/settings.json` | Global |
| `.pi/settings.json` | Project override |

Project settings sẽ override global.

Ví dụ:

```json
{
  "extensions": ["./.pi/extensions"],
  "skills": ["./.pi/skills"],
  "prompts": ["./.pi/prompts"],
  "theme": "dark"
}
```

## Session Memory

Pi lưu session dạng JSONL (có tree branch).

- Session nằm ở `~/.pi/agent/sessions/`
- Dùng `/resume`, `/tree`, `/fork` để điều hướng lịch sử
- Dùng `/compact` khi context quá dài

CLI hay dùng:

```bash
pi -c
pi -r
pi --no-session
pi --session <path>
pi --fork <path>
```

## Best Practices

1. Giữ `AGENTS.md` ngắn, rõ, thực dụng.
2. Ưu tiên `APPEND_SYSTEM.md` trước khi thay hẳn bằng `SYSTEM.md`.
3. Hành vi tái sử dụng nên đưa vào skills/extensions.
4. Session dài thì compact sớm.
5. Cấu hình theo project nên đặt ở `.pi/settings.json`.

## Lỗi thường gặp

- Nhồi quá nhiều rule vào một prompt dài
- Ghi đè `SYSTEM.md` dù chỉ cần append
- Quên mất project settings override global
- Không dùng `/tree` và `/fork` nên mất nhánh làm việc hữu ích

## Quick Reference

| File | Vai trò |
|------|---------|
| `AGENTS.md` | Hướng dẫn và conventions của project |
| `CLAUDE.md` | File tương thích thay cho `AGENTS.md` |
| `SYSTEM.md` | Thay thế default system prompt |
| `APPEND_SYSTEM.md` | Nối thêm vào system prompt |
| `settings.json` | Cấu hình runtime |

Xem bản đầy đủ: [../../02-memory/README.md](../../02-memory/README.md)
