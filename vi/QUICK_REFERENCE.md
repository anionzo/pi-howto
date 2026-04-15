# Tham khảo nhanh

Bảng ghi nhớ ngắn cho các thao tác pi thường dùng hằng ngày.

## Cài đặt

```bash
npm install -g @mariozechner/pi-coding-agent
```

## Bắt đầu

```bash
pi
/login
```

## Các lệnh hay dùng nhất

| Lệnh | Mục đích |
|------|----------|
| `/model` | Chọn model |
| `/settings` | Chỉnh theme, thinking và transport |
| `/resume` | Mở lại session cũ |
| `/new` | Tạo session mới |
| `/session` | Xem đường dẫn, mức dùng, chi phí |
| `/tree` | Điều hướng cây session hiện tại |
| `/fork` | Tách session mới từ nhánh hiện tại |
| `/compact` | Nén bớt ngữ cảnh cũ |
| `/reload` | Nạp lại extensions, skills, prompts và context |
| `/hotkeys` | Xem toàn bộ phím tắt |

## Các cờ CLI thiết yếu

```bash
pi -c
pi -r
pi --no-session
pi --session <path-or-id>
pi --fork <path-or-id>
pi --model openai/gpt-4o
pi --thinking high
pi -p "Tóm tắt nội dung này"
```

## Phím tắt thông dụng

| Phím | Tác dụng |
|------|----------|
| `Enter` | Gửi prompt khi pi đang rảnh |
| `Shift+Enter` | Xuống dòng |
| `Escape` | Hủy / dừng thao tác |
| `Ctrl+C` | Xóa nội dung đang gõ |
| `Ctrl+D` | Thoát khi ô nhập đang trống |
| `Ctrl+G` | Mở trình soạn thảo ngoài |
| `Ctrl+L` | Mở bảng chọn model |
| `Ctrl+P` | Chuyển model tiến tới |
| `Shift+Ctrl+P` | Chuyển model lùi lại |
| `Shift+Tab` | Đổi mức thinking |
| `Ctrl+T` | Bật/tắt thinking blocks |
| `Ctrl+O` | Bật/tắt tool output |
| `Alt+Enter` | Xếp hàng tin nhắn tiếp nối |
| `Alt+Up` | Kéo tin nhắn đã xếp về ô nhập |

## Hàng đợi tin nhắn

| Hành động | Ý nghĩa |
|-----------|---------|
| `Enter` khi agent đang chạy | xếp hàng tin nhắn điều hướng |
| `Alt+Enter` | xếp hàng tin nhắn tiếp nối sau khi agent hoàn tất công việc hiện tại |
| `Escape` | hủy và đưa tin nhắn đã xếp về ô nhập |

## Các đường dẫn quan trọng

| Mục | Đường dẫn |
|-----|-----------|
| Thư mục cấu hình toàn cục | `~/.pi/agent/` |
| Phiên làm việc | `~/.pi/agent/sessions/` |
| Cài đặt | `~/.pi/agent/settings.json` |
| Tệp xác thực | `~/.pi/agent/auth.json` |
| Hướng dẫn dự án | `~/.pi/agent/AGENTS.md` |
| Tiện ích mở rộng | `~/.pi/agent/extensions/` |
| Kỹ năng | `~/.pi/agent/skills/` |
| Giao diện | `~/.pi/agent/themes/` |
| Phím tắt | `~/.pi/agent/keybindings.json` |

## Quản lý package

```bash
pi install npm:@foo/bar
pi install git:github.com/user/repo@v1
pi list
pi update
pi remove npm:@foo/bar
pi config
```

## Các tệp ngữ cảnh

| Tệp | Vai trò |
|-----|---------|
| `AGENTS.md` | Hướng dẫn của dự án |
| `CLAUDE.md` | Tệp tương thích thay cho `AGENTS.md` |
| `SYSTEM.md` | Thay thế system prompt mặc định |
| `APPEND_SYSTEM.md` | Nối thêm vào system prompt mặc định |
| `settings.json` | Điều khiển hành vi runtime và việc nạp tài nguyên |

## Đọc tiếp

- [README.md](README.md)
- [01-commands](01-commands/README.md)
- [06-sessions](06-sessions/README.md)
- [09-settings](09-settings/README.md)
