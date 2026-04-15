# pi-howto 🇻🇳

<p align="center">
  <img src="../assets/logo/pi-logo.svg" alt="pi logo" width="128">
</p>

> Tài liệu thực hành để sử dụng và tùy biến pi coding agent bằng tiếng Việt.

Pi là một công cụ hỗ trợ lập trình trên terminal theo hướng tối giản. Bộ tài liệu này sắp xếp nội dung theo hướng đi từ nhập môn đến tùy biến sâu để bạn vào việc nhanh mà vẫn có chỗ đào sâu khi cần.

## Repo này bao quát những gì?

Bộ tài liệu này giải thích các phần chính của pi:
- lệnh tương tác
- tệp ngữ cảnh và cài đặt liên quan bộ nhớ phiên
- kỹ năng
- tiện ích mở rộng
- giao diện
- quản lý phiên làm việc
- phím tắt
- nhà cung cấp và mô hình
- cài đặt
- gói pi
- mẫu prompt

## Mục lục

| Phần | Mô tả |
|------|-------|
| [01-commands](01-commands/README.md) | Lệnh, phím tắt quan trọng, tính năng trình soạn thảo, hàng đợi tin nhắn |
| [02-memory](02-memory/README.md) | `AGENTS.md`, `CLAUDE.md`, `SYSTEM.md`, `APPEND_SYSTEM.md`, settings |
| [03-skills](03-skills/README.md) | Vị trí kỹ năng, định dạng, kiểm tra hợp lệ, các workflow skill |
| [04-extensions](04-extensions/README.md) | Tiện ích mở rộng TypeScript, sự kiện, công cụ, lệnh, UI |
| [05-themes](05-themes/README.md) | Định dạng giao diện, 51 color token, hot reload |
| [06-sessions](06-sessions/README.md) | Tệp JSONL, phân nhánh, compaction, cây phiên |
| [07-keybindings](07-keybindings/README.md) | Tham chiếu phím tắt và cách tùy biến |
| [08-providers](08-providers/README.md) | OAuth subscription, API key, cloud provider |
| [09-settings](09-settings/README.md) | Cài đặt toàn cục và theo project |
| [10-pi-packages](10-pi-packages/README.md) | Đóng gói, cài đặt, chia sẻ tài nguyên |
| [11-prompt-templates](11-prompt-templates/README.md) | Macro prompt tái sử dụng, tham số, ví dụ |

Tham khảo thêm:
- [CATALOG.md](CATALOG.md)
- [QUICK_REFERENCE.md](QUICK_REFERENCE.md)
- [Tiếng Anh](../README.md)

## Bắt đầu nhanh

```bash
npm install -g @mariozechner/pi-coding-agent

# chạy pi
pi

# đăng nhập và chọn nhà cung cấp bạn muốn dùng
/login
```

## Những thao tác đầu tiên nên biết

```bash
/model          # chọn model
/settings       # chỉnh theme, thinking, transport
/new            # tạo session mới
/resume         # mở lại việc cũ
/tree           # điều hướng cây session hiện tại
/hotkeys        # xem phím tắt
```

## Tổng quan tính năng

| Tính năng | Giá trị mang lại | Lối vào nhanh |
|-----------|------------------|---------------|
| Lệnh | Điều khiển phiên làm việc và runtime | `/login`, `/model`, `/settings` |
| Phiên | Lưu lịch sử hội thoại dạng JSONL có phân nhánh | `/resume`, `/tree`, `/fork`, `/compact` |
| Ngữ cảnh | Hướng dẫn dự án và định hình prompt | `AGENTS.md`, `CLAUDE.md`, `SYSTEM.md`, `APPEND_SYSTEM.md` |
| Kỹ năng | Gói khả năng tái sử dụng | `/skill:name` |
| Tiện ích mở rộng | Công cụ, sự kiện, lệnh và UI tùy chỉnh | `.pi/extensions/` |
| Giao diện | Tùy biến phần nhìn với hot reload | `/settings` |
| Mẫu prompt | Macro prompt cho tác vụ lặp lại | `.pi/prompts/`, `/ten-template` |
| Gói pi | Gói chia sẻ extensions, skills, prompts và themes | `pi install ...` |

## Trạng thái tài liệu tiếng Việt

- [x] README
- [x] 01-commands
- [x] 02-memory
- [x] 03-skills
- [x] 04-extensions
- [x] 05-themes
- [x] 06-sessions
- [x] 07-keybindings
- [x] 08-providers
- [x] 09-settings
- [x] 10-pi-packages
- [x] 11-prompt-templates
- [x] CATALOG.md
- [x] QUICK_REFERENCE.md
