# Bộ nhớ và tệp ngữ cảnh

Phần này giải thích cách pi nạp hướng dẫn, lưu lịch sử làm việc và dùng cài đặt để quản lý ngữ cảnh.

## “Bộ nhớ” trong pi là gì?

Trong pi, bộ nhớ chủ yếu là sự kết hợp của:

1. **Tệp hướng dẫn** — quy định agent nên hành xử thế nào
2. **Lịch sử phiên làm việc** — những gì đã diễn ra trong cuộc hội thoại
3. **Cài đặt runtime** — quyết định pi tải gì và hoạt động ra sao

## Các tệp hướng dẫn

### `AGENTS.md` / `CLAUDE.md`

Pi nạp `AGENTS.md` (hoặc `CLAUDE.md`) theo thứ tự:

1. `~/.pi/agent/AGENTS.md` (toàn cục)
2. các thư mục cha khi đi ngược từ thư mục hiện tại
3. thư mục hiện tại

Các tệp khớp sẽ được ghép vào ngữ cảnh.

Nên dùng các tệp này cho:
- quy ước của repo
- tiêu chuẩn mã nguồn
- ghi chú kiến trúc
- quy tắc an toàn
- các lệnh thường dùng của dự án

### `SYSTEM.md`

Dùng để **thay thế** toàn bộ system prompt mặc định.

| Đường dẫn | Phạm vi |
|-----------|---------|
| `~/.pi/agent/SYSTEM.md` | Toàn cục |
| `.pi/SYSTEM.md` | Theo project |

Chỉ nên dùng khi bạn thật sự muốn kiểm soát hoàn toàn hành vi nền của agent.

### `APPEND_SYSTEM.md`

Dùng để **nối thêm** hướng dẫn vào system prompt mặc định.

| Đường dẫn | Phạm vi |
|-----------|---------|
| `~/.pi/agent/APPEND_SYSTEM.md` | Toàn cục |
| `.pi/APPEND_SYSTEM.md` | Theo project |

Cách này thường an toàn hơn `SYSTEM.md` vì không ghi đè toàn bộ prompt gốc.

## Cài đặt runtime

| Đường dẫn | Phạm vi |
|-----------|---------|
| `~/.pi/agent/settings.json` | Toàn cục |
| `.pi/settings.json` | Ghi đè theo project |

Cài đặt của project sẽ ghi đè cài đặt toàn cục.

Ví dụ các thiết lập ảnh hưởng đến việc nạp ngữ cảnh:

```json
{
  "extensions": ["./.pi/extensions"],
  "skills": ["./.pi/skills"],
  "prompts": ["./.pi/prompts"],
  "theme": "dark"
}
```

## Bộ nhớ phiên làm việc

Pi lưu session dưới dạng tệp JSONL với cấu trúc phân nhánh.

- Tệp session nằm trong `~/.pi/agent/sessions/`
- Dùng `/resume`, `/tree`, `/fork` để mở lại hoặc tách nhánh lịch sử
- Dùng `/compact` để tóm tắt bớt ngữ cảnh cũ khi phiên quá dài

Một số cờ CLI hay dùng:

```bash
pi -c
pi -r
pi --no-session
pi --session <path>
pi --fork <path>
```

## Cách dùng nên ưu tiên

1. Giữ `AGENTS.md` ngắn, rõ và thực dụng.
2. Ưu tiên `APPEND_SYSTEM.md` trước khi ghi đè toàn bộ bằng `SYSTEM.md`.
3. Đưa hành vi tái sử dụng vào skills hoặc extensions thay vì dồn hết vào prompt dài.
4. Dùng `/compact` khi session kéo dài quá nhiều.
5. Giữ cấu hình riêng của project trong `.pi/settings.json`.

## Lỗi thường gặp

- Nhồi quá nhiều chính sách vào một system prompt quá dài
- Ghi đè `SYSTEM.md` dù chỉ cần nối thêm hướng dẫn
- Quên rằng project settings ghi đè global settings
- Không tận dụng `/tree` và `/fork`, làm mất các nhánh làm việc hữu ích

## Tóm tắt nhanh

| Tệp | Vai trò |
|-----|---------|
| `AGENTS.md` | Hướng dẫn và quy ước của dự án |
| `CLAUDE.md` | Tệp tương thích thay cho `AGENTS.md` |
| `SYSTEM.md` | Thay thế system prompt mặc định |
| `APPEND_SYSTEM.md` | Nối thêm hướng dẫn vào system prompt |
| `settings.json` | Cấu hình runtime |

## Đọc tiếp

- [01-commands](../01-commands/README.md)
- [03-skills](../03-skills/README.md)
- [06-sessions](../06-sessions/README.md)
- [09-settings](../09-settings/README.md)
- Bản đầy đủ tiếng Anh: [../../02-memory/README.md](../../02-memory/README.md)
