# Danh mục tính năng

Danh mục nhỏ gọn về các khả năng chính của pi và nơi nên đọc tiếp.

## Lệnh

### Lệnh tương tác

| Tính năng | Điểm vào | Ghi chú |
|-----------|----------|---------|
| Xác thực | `/login`, `/logout` | Nhà cung cấp đăng ký OAuth |
| Chọn model | `/model`, `/scoped-models` | Chọn hoặc chuyển model |
| Điều khiển phiên | `/new`, `/resume`, `/session` | Bắt đầu, mở lại, kiểm tra |
| Điều hướng cây | `/tree`, `/fork` | Phân nhánh và xem lại lịch sử |
| Nén ngữ cảnh | `/compact` | Tóm tắt ngữ cảnh cũ thủ công |
| Xuất/chia sẻ | `/copy`, `/export`, `/share` | Dùng lại hoặc công khai kết quả |
| Tải lại runtime | `/reload` | Tải lại extensions, skills, prompts, và ngữ cảnh |
| Khám phá | `/hotkeys`, `/changelog`, `/quit` | Học hỏi và thoát |

### Cờ CLI

| Tính năng | Ví dụ |
|-----------|-------|
| Tiếp tục phiên | `pi -c` |
| Duyệt phiên | `pi -r` |
| Chế độ tạm thời | `pi --no-session` |
| Chọn tệp phiên | `pi --session <path>` |
| Fork phiên | `pi --fork <path>` |
| Print mode | `pi -p "Tóm tắt nội dung này"` |
| JSON / RPC | `pi --mode json`, `pi --mode rpc` |

Đọc thêm: [01-commands](01-commands/README.md)

## Bộ nhớ & Ngữ cảnh

| Tính năng | Tệp / công cụ | Ghi chú |
|-----------|----------------|---------|
| Hướng dẫn dự án | `AGENTS.md`, `CLAUDE.md` | Hướng dẫn khởi động |
| Thay thế system prompt | `SYSTEM.md` | Ghi đè prompt mạnh nhất |
| Nối thêm system prompt | `APPEND_SYSTEM.md` | Lớp prompt bổ sung |
| Cấu hình runtime | `settings.json` | Hành vi và tải tài nguyên |
| Ngữ cảnh phiên | `/resume`, `/tree`, `/compact` | Xem lại, phân nhánh, và rút gọn lịch sử |

Đọc thêm: [02-memory](02-memory/README.md)

## Kỹ năng

| Tính năng | Ghi chú |
|-----------|---------|
| Hiển thị tiệm tiến | Metadata luôn được tải, thân theo yêu cầu |
| Lệnh kỹ năng | `/skill:name` |
| Vị trí | toàn cục, dự án, packages, settings, CLI |
| Kiểm tra hợp lệ | Chuẩn Agent Skills, cảnh báo nhẹ |
| Gói quy trình | Kỹ năng theo repo hoặc theo package có thể định nghĩa quy trình tái sử dụng |

Đọc thêm: [03-skills](03-skills/README.md)

## Tiện ích mở rộng

| Khả năng | Ví dụ |
|---------|--------|
| Công cụ tùy chỉnh | Tự động hóa theo miền cụ thể |
| Chặn sự kiện | Kiểm tra quyền, bảo vệ đường dẫn |
| Lệnh & phím tắt | `/deploy`, `ctrl+shift+p` |
| UI tùy chỉnh | widget, dialog, thành phần TUI đầy đủ |
| Điều khiển nhà cung cấp | đăng ký nhà cung cấp hoặc proxy tùy chỉnh |
| Trạng thái theo phiên | duy trì qua các entry của phiên |

Đọc thêm: [04-extensions](04-extensions/README.md)

## Giao diện

| Tính năng | Ghi chú |
|-----------|---------|
| Giao diện sẵn | `dark`, `light` |
| Giao diện tùy chỉnh | Tệp JSON |
| Số token màu yêu cầu | 51 token |
| Loại giá trị | hex, 256-color, biến, mặc định |
| Hot reload | giao diện đang dùng tự động tải lại |

Đọc thêm: [05-themes](05-themes/README.md)

## Phiên làm việc

| Tính năng | Ghi chú |
|-----------|---------|
| Định dạng lưu trữ | JSONL |
| Cấu trúc cây | `id` / `parentId` |
| Phiên bản | v1, v2, v3 tự động nâng cấp |
| Phân nhánh | `/tree` và `/fork` |
| Nén ngữ cảnh | thủ công hoặc tự động |
| Xuất | HTML hoặc gist |
| SessionManager | API cho SDK/extensions |

Đọc thêm: [06-sessions](06-sessions/README.md)

## Phím tắt

| Khu vực | Ghi chú |
|---------|---------|
| Di chuyển trong soạn thảo | Mặc định kiểu Emacs |
| Điều khiển nhập liệu | gửi, dòng mới, tự động hoàn thành |
| Điều khiển ứng dụng | ngắt, xóa, thoát, tạm dừng |
| Điều khiển phiên | mở lại, đổi tên, xóa |
| Điều khiển model | chọn, chuyển, thinking |
| Điều hướng cây | gập, mở, gắn nhãn lại |

Đọc thêm: [07-keybindings](07-keybindings/README.md)

## Nhà cung cấp & Model

| Loại | Ghi chú |
|-------|--------|
| Đăng ký | Claude Pro/Max, ChatGPT Plus/Pro, Copilot, Gemini CLI, Antigravity |
| API keys | Anthropic, OpenAI, Gemini, Mistral, Groq, Cerebras, xAI, v.v... |
| Lưu trữ xác thực | `~/.pi/agent/auth.json` |
| Cloud providers | Azure OpenAI, Amazon Bedrock, Vertex AI |
| Nhà cung cấp tùy chỉnh | `models.json` hoặc extensions |
| Thứ tự phân giải | CLI → auth.json → biến môi trường → models.json |

Đọc thêm: [08-providers](08-providers/README.md)

## Cài đặt

| Khu vực | Ví dụ |
|---------|-------|
| Mặc định model | `defaultProvider`, `defaultModel`, `defaultThinkingLevel` |
| UI | `theme`, `quietStartup`, `doubleEscapeAction` |
| Nén ngữ cảnh | `compaction.enabled`, `reserveTokens`, `keepRecentTokens` |
| Thử lại | `maxRetries`, `baseDelayMs`, `maxDelayMs` |
| Phân phối | `steeringMode`, `followUpMode`, `transport` |
| Tài nguyên | `packages`, `extensions`, `skills`, `prompts`, `themes` |

Đọc thêm: [09-settings](09-settings/README.md)

## Gói Pi

| Tính năng | Ghi chú |
|-----------|---------|
| Nguồn | npm, git, đường dẫn cục bộ |
| Phạm vi | toàn cục hoặc theo dự án |
| Manifest | `package.json` → khóa `pi` |
| Quy ước | `extensions/`, `skills/`, `prompts/`, `themes/` |
| Lọc | đường dẫn include/exclude trong settings |
| Bật/tắt | `pi config` |

Đọc thêm: [10-pi-packages](10-pi-packages/README.md)

## Mẫu Prompt

| Tính năng | Ghi chú |
|-----------|---------|
| Prompt tái sử dụng | Tệp Markdown mở rộng thành slash command |
| Tham số | `{{name}}` placeholder yêu cầu giá trị |
| Vị trí | toàn cục, dự án, packages, CLI |
| Trường hợp dùng tốt | review, giải thích, tóm tắt, chuẩn bị commit |

Đọc thêm: [11-prompt-templates](11-prompt-templates/README.md)

## Chế độ Headless

| Chế độ | Cờ | Trường hợp dùng |
|--------|-----|-----------------|
| Print | `pi -p "query"` | Văn bản một lần ra stdout |
| JSON | `pi --mode json` | Stream sự kiện JSONL |
| RPC | `pi --mode rpc --no-session` | Giao thức JSON qua stdin/stdout |
| SDK | API lập trình | Nhúng pi trong ứng dụng Node.js |

Đọc thêm: [12-headless-modes](12-headless-modes/README.md)

## Tệp hỗ trợ

| Tệp | Mục đích |
|------|----------|
| [QUICK_REFERENCE.md](QUICK_REFERENCE.md) | Bảng cứu thương |
| [README.md](README.md) | Điểm vào chính |
| [../README.md](../README.md) | Bản tiếng Anh |
