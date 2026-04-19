# Tiện ích mở rộng

Tiện ích mở rộng (extensions) là các module TypeScript giúp mở rộng hành vi của pi. Chúng có thể đăng ký công cụ mới, thêm lệnh, móc vào vòng đời chạy của agent và dựng giao diện tùy chỉnh.

## Tiện ích mở rộng có thể làm gì?

- đăng ký công cụ mới bằng `pi.registerTool()`
- chặn hoặc sửa `tool_call`
- thêm slash command như `/deploy` hoặc `/review`
- thêm phím tắt và cờ CLI
- móc vào các sự kiện của session, agent, tool và model
- dựng UI tùy chỉnh bằng thông báo, hộp thoại, widget hoặc TUI component
- lưu trạng thái vào session
- đăng ký provider tùy chỉnh hoặc ghi đè cấu hình provider

## Bắt đầu nhanh

Tạo file `~/.pi/agent/extensions/my-extension.ts`:

```typescript
import type { ExtensionAPI } from "@mariozechner/pi-coding-agent";

export default function (pi: ExtensionAPI) {
  pi.registerCommand("hello", {
    description: "Say hello",
    handler: async (_args, ctx) => {
      ctx.ui.notify("Hello!", "success");
    }
  });
}
```

Chạy thử:

```bash
pi -e ~/.pi/agent/extensions/my-extension.ts
```

Hoặc đặt đúng thư mục rồi chạy `pi` bình thường và dùng `/reload` sau khi sửa file.

## Vị trí nạp extension

> **Bảo mật:** Extensions chạy như mã hệ thống thật. Chỉ cài từ nguồn bạn tin tưởng.

- `~/.pi/agent/extensions/*.ts`
- `~/.pi/agent/extensions/*/index.ts`
- `.pi/extensions/*.ts`
- `.pi/extensions/*/index.ts`
- `--extension <path>` hoặc `-e`
- package pi có khai báo `extensions`

Bạn cũng có thể thêm đường dẫn trong `settings.json` qua trường `extensions`.

## Các package import hay dùng

- `@mariozechner/pi-coding-agent`
- `@sinclair/typebox`
- `@mariozechner/pi-ai`
- `@mariozechner/pi-tui`
- các package built-in của Node như `node:fs`, `node:path`

## API quan trọng

- `pi.on()` — đăng ký handler cho sự kiện
- `pi.registerTool()` — tạo tool cho model gọi
- `pi.registerCommand()` — tạo slash command
- `pi.registerShortcut()` — tạo phím tắt
- `pi.registerFlag()` — thêm cờ CLI
- `pi.sendMessage()` / `pi.sendUserMessage()` — chèn message vào session
- `pi.appendEntry()` — lưu trạng thái vào session
- `pi.registerProvider()` — thêm provider tùy chỉnh

## Các sự kiện nổi bật

### Sự kiện tài nguyên
- `resources_discover`

### Sự kiện session
- `session_start`
- `session_before_fork`
- `session_before_compact`
- `session_before_tree`
- `session_shutdown`

### Sự kiện agent

> **`event.source` trong sự kiện `input`:** Kiểm tra `event.source` để phân biệt input từ `"interactive"`, `"rpc"` hay `"extension"`. Quan trọng khi xây extension chạy cả ở headless lẫn interactive mode.

- `input` — biến đổi input trước khi mở rộng skill/template. `event.source` là `"interactive"`, `"rpc"` hoặc `"extension"`
- `before_agent_start`
- `agent_start`
- `turn_start`
- `context`
- `before_provider_request`
- `agent_end`

### Sự kiện tool
- `tool_execution_start`
- `tool_call`
- `tool_result`
- `tool_execution_end`
- `user_bash`

## Hỗ trợ UI

Trong handler, bạn thường dùng `ctx.ui` để dựng giao diện:

- `ctx.ui.notify()`
- `ctx.ui.confirm()`
- `ctx.ui.input()`
- `ctx.ui.select()`
- `ctx.ui.setStatus()`
- `ctx.ui.setWidget()`
- `ctx.ui.custom()`

## Ghi nhớ quan trọng

- Đặt canonical state vào session để việc phân nhánh và `/reload` vẫn đúng
- Với tool sửa file, dùng `withFileMutationQueue()` (import từ `@mariozechner/pi-coding-agent`) để phối hợp với `edit` và `write`
- Tôn trọng `ctx.signal` để xử lý hủy tác vụ đúng cách
- Khi phát triển thường xuyên, dùng thư mục auto-discover + `/reload` sẽ tiện hơn khởi động lại pi

### Hành vi `ctx` theo chế độ

| Phương thức | Interactive | RPC | Print |
|-------------|-------------|-----|-------|
| `ctx.hasUI` | `true` | `true` | `false` |
| `ctx.shutdown()` | trì hoãn đến idle | trì hoãn đến idle tiếp theo | không làm gì |
| `ctx.ui.notify()` | hiện trong TUI | phát RPC request | không làm gì |

## Extension cộng đồng phổ biến

Danh sách các extension đáng thử. Cài bằng `pi install`.

### Quản lý task & phối hợp

| Extension | Cài đặt | Mô tả |
|-----------|---------|-------|
| **[@tintinweb/pi-tasks](https://github.com/tintinweb/pi-tasks)** | `pi install npm:@tintinweb/pi-tasks` | Theo dõi task kiểu Claude Code với 7 tool cho LLM, widget hiển thị liên tục, quản lý dependency, auto-cascade qua DAG. Hỗ trợ chia sẻ task list giữa các session. |
| **[@tintinweb/pi-subagents](https://github.com/tintinweb/pi-subagents)** | `pi install npm:@tintinweb/pi-subagents` | Tạo subagent chạy nền từ pi. Kết hợp với `pi-tasks` để chạy task song song qua `TaskExecute`. |

### Hội thoại phụ

| Extension | Cài đặt | Mô tả |
|-----------|---------|-------|
| **[pi-btw](https://github.com/dbachelder/pi-btw)** | `pi install npm:pi-btw` | `/btw` mở cuộc hội thoại phụ song song, không gián đoạn agent chính. Chạy như sub-session thật với đầy đủ tool. Hỗ trợ tangent thread, override model/thinking, inject kết quả về session chính. |

### Tăng cường prompt

| Extension | Cài đặt | Mô tả |
|-----------|---------|-------|
| **[pi-augment](https://github.com/sting8k/pi-augment)** | `pi install npm:pi-augment` | `/augment` viết lại prompt mạnh hơn trước khi gửi. Tự nhận diện intent (implement, debug, refactor, review...), chọn chế độ rewrite, điều chỉnh mức effort. Nhận biết model family (Claude vs GPT). |

### UI & Tích hợp

| Extension | Cài đặt | Mô tả |
|-----------|---------|-------|
| **[@alasano/pi-panels](https://github.com/alasano/house-of-pi)** | `pi install npm:@alasano/pi-panels` | Panel trạng thái bên dưới editor — git info, context usage, Spotify đang phát. |
| **[@alasano/pi-mouse](https://github.com/alasano/house-of-pi)** | `pi install npm:@alasano/pi-mouse` | Chuột ASCII sống phía trên editor, chạy theo con trỏ khi bạn gõ. |
| **[@alasano/pi-linear](https://github.com/alasano/house-of-pi)** | `pi install npm:@alasano/pi-linear` | Tích hợp Linear với 55+ tool cho issues, projects, documents, comments. Hỗ trợ multi-workspace. |

> **Chia sẻ session:** Gói `pi-share-hf` cho phép chia sẻ session lên Hugging Face thay vì GitHub Gist. Cài qua `pi install npm:pi-share-hf`.

---

[← Skills](../03-skills/README.md) | [Mục lục](../README.md) | [Giao diện →](../05-themes/README.md)

### Xem thêm

- [Skills](../03-skills/README.md)
- [Giao diện](../05-themes/README.md)
- [Phiên làm việc](../06-sessions/README.md)
- [Chế độ Headless](../12-headless-modes/README.md)

Bản tiếng Anh: [../../04-extensions/README.md](../../04-extensions/README.md)
