# Commands

Trang này tóm tắt các command quan trọng của pi.

## Built-in Commands

| Command | Ý nghĩa |
|---------|--------|
| `/login`, `/logout` | Đăng nhập / đăng xuất provider OAuth |
| `/model` | Chọn model |
| `/scoped-models` | Chọn model dùng cho `Ctrl+P` |
| `/settings` | Mở settings UI |
| `/resume` | Mở lại session cũ |
| `/new` | Tạo session mới |
| `/name <name>` | Đặt tên session |
| `/session` | Xem path, token, cost |
| `/tree` | Điều hướng cây session |
| `/fork` | Tách branch hiện tại thành session mới |
| `/compact [prompt]` | Compact context |
| `/copy` | Copy câu trả lời cuối |
| `/export [file]` | Xuất HTML |
| `/share` | Upload gist riêng tư |
| `/reload` | Reload extensions, skills, prompts, context |
| `/hotkeys` | Xem phím tắt |
| `/changelog` | Xem lịch sử version |
| `/quit` | Thoát pi |

## CLI options hay dùng

```bash
pi -c
pi -r
pi --no-session
pi --session <path-or-id>
pi --fork <path-or-id>
pi --model openai/gpt-4o
pi --thinking high
pi -p "Summarize this"
```

## Editor features

- gõ `@` để chọn file
- `Tab` để autocomplete path
- `Shift+Enter` để xuống dòng
- `!cmd` chạy lệnh và gửi output vào context
- `!!cmd` chạy lệnh nhưng không đưa output vào context
- `Ctrl+V` / `Alt+V` để dán ảnh tùy nền tảng

## Message queue

- `Enter` khi agent đang chạy: queue steer message
- `Alt+Enter`: queue follow-up
- `Escape`: abort và đưa message queued về editor
- `Alt+Up`: kéo queued message về editor

Xem bản đầy đủ: [../../01-commands/README.md](../../01-commands/README.md)
