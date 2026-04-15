# Hệ thống lệnh

Chương này giúp bạn **vào việc nhanh** với pi theo luồng ít ma sát nhất.

## Pi là gì? Dùng khi nào?

Pi là một công cụ hỗ trợ lập trình trên terminal theo hướng tối giản.

Bạn nên dùng pi khi cần:
- làm việc trực tiếp trong mã nguồn bằng hội thoại
- thao tác nhanh qua lệnh `/...` và phím tắt
- giữ lịch sử phiên làm việc để quay lại, tách nhánh, tiếp tục
- tùy biến theo workflow riêng thay vì bị ép theo một quy trình cố định

## Khởi động nhanh (tối thiểu)

```bash
npm install -g @mariozechner/pi-coding-agent
pi
/login
```

> `/login` là cách trung lập nhà cung cấp. Pi hỗ trợ nhiều nhà cung cấp (OpenAI, Anthropic, Gemini, GitHub Copilot...).

## Luồng làm việc cơ bản cho người mới

1. Mở terminal và chạy `pi`
2. Dùng `/login` để xác thực và chọn nhà cung cấp
3. Bắt đầu phiên mới với `/new` hoặc mở lại phiên cũ bằng `/resume`
4. Dùng `/model` để chọn mô hình phù hợp
5. Bắt đầu yêu cầu công việc đầu tiên

## Lệnh có sẵn quan trọng

| Lệnh | Ý nghĩa |
|------|---------|
| `/login`, `/logout` | Đăng nhập / đăng xuất qua OAuth |
| `/model` | Chọn mô hình |
| `/scoped-models` | Chọn tập mô hình dùng cho `Ctrl+P` |
| `/settings` | Mở giao diện cài đặt |
| `/resume` | Mở lại phiên cũ |
| `/new` | Tạo phiên mới |
| `/name <name>` | Đặt tên phiên |
| `/session` | Xem đường dẫn, token, chi phí |
| `/tree` | Điều hướng cây phiên |
| `/fork` | Tách nhánh hiện tại thành phiên mới |
| `/compact [prompt]` | Nén ngữ cảnh |
| `/copy` | Sao chép câu trả lời cuối |
| `/export [file]` | Xuất HTML |
| `/share` | Tải lên gist riêng tư |
| `/reload` | Nạp lại extension, skill, prompt, context |
| `/hotkeys` | Xem phím tắt |
| `/changelog` | Xem lịch sử phiên bản |
| `/quit` | Thoát pi |

## Tùy chọn CLI hay dùng

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

## Phím tắt tương tác quan trọng

- `Enter` khi pi đang rảnh: gửi prompt ngay
- `Enter` khi agent đang chạy: xếp hàng tin nhắn điều hướng (steering) để gửi sau khi lượt hiện tại hoàn tất các tool call
- `Alt+Enter`: xếp hàng tin nhắn tiếp nối (follow-up), gửi sau khi agent hoàn tất toàn bộ công việc hiện tại
- `Ctrl+L`: mở bảng chọn mô hình
- `Ctrl+P` / `Shift+Ctrl+P`: chuyển nhanh giữa các mô hình đã chọn phạm vi

## Tính năng trình soạn thảo

- gõ `@` để chọn tệp
- `Tab` để tự động hoàn thành đường dẫn
- `Shift+Enter` để xuống dòng
- `!cmd` chạy lệnh và gửi đầu ra vào ngữ cảnh
- `!!cmd` chạy lệnh nhưng không gửi đầu ra vào ngữ cảnh
- `Ctrl+V` / `Alt+V` để dán ảnh (tùy nền tảng)

## Hàng đợi tin nhắn

- `Enter` khi agent đang chạy: xếp hàng tin nhắn điều hướng
- `Alt+Enter`: xếp hàng tin nhắn tiếp nối
- `Escape`: hủy xử lý và đưa tin nhắn đã xếp về trình soạn thảo
- `Alt+Up`: kéo tin nhắn đã xếp về trình soạn thảo

## Lỗi thường gặp và lưu ý

- **Hiểu nhầm nhà cung cấp:** Pi không chỉ dùng được một nhà cung cấp. Phần nhập môn nên ưu tiên `/login` để tránh thiên lệch.
- **Quá tải thông tin từ đầu:** Không cần học skills/extensions/themes trong chương này; để sang các chương mở rộng.
- **Quên quản lý phiên:** Nếu đang làm việc dở, dùng `/resume` hoặc `/tree` thay vì mở phiên mới liên tục.
- **Nhầm loại tin nhắn khi agent đang chạy:** `Enter` và `Alt+Enter` có hành vi khác nhau; nên dùng đúng ngữ cảnh.

## Đọc tiếp

- [02-memory](../02-memory/README.md)
- [06-sessions](../06-sessions/README.md)
- [07-keybindings](../07-keybindings/README.md)
- Bản đầy đủ tiếng Anh: [../../01-commands/README.md](../../01-commands/README.md)
