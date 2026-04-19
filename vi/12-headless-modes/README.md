# Chế độ headless

Pi có bốn chế độ vận hành. Chế độ tương tác (interactive) là mặc định với giao diện TUI. Ba chế độ còn lại — print, JSON và RPC — chạy không cần giao diện, phù hợp cho script, CI pipeline và tích hợp vào ứng dụng khác.

## Bốn chế độ

| Chế độ | Cờ | Trường hợp dùng |
|--------|-----|-----------------|
| Interactive | *(mặc định)* | Dùng TUI bình thường |
| Print | `-p "query"` | Xuất text ra stdout rồi thoát |
| JSON | `--mode json` | Stream toàn bộ sự kiện dạng JSONL |
| RPC | `--mode rpc` | Giao thức JSON qua stdin/stdout cho tích hợp non-Node |

Ngoài ra còn có **SDK mode** để nhúng pi trực tiếp vào ứng dụng Node.js.

## Print mode

Gửi câu hỏi, in kết quả ra stdout rồi thoát. Lý tưởng cho shell script và pipe.

```bash
pi -p "Tóm tắt file này" < README.md
pi -p "Hàm này làm gì?" --model openai/gpt-4o
pi -p "Liệt kê TODO" --no-session
```

- `ctx.hasUI` là `false`
- Không lưu session trừ khi chỉ định `--session`
- Output ra stdout, lỗi ra stderr

### Kết hợp với công cụ khác

```bash
# Pipe sang clipboard
pi -p "Giải thích lỗi này" | pbcopy

# Dùng trong script
SUMMARY=$(pi -p "Tóm tắt thay đổi" < CHANGELOG.md)
echo "$SUMMARY"
```

## JSON mode

Stream toàn bộ sự kiện dạng JSONL ra stdout. Dùng khi cần truy cập có cấu trúc vào tool calls, thinking blocks và các sự kiện nội bộ.

```bash
pi -p "query" --mode json
pi -p "query" --mode json --no-session
```

- Mỗi dòng là một JSON object đầy đủ
- `ctx.hasUI` là `false`
- Hữu ích để xây UI riêng hoặc logging pipeline

## RPC mode

Biến pi thành subprocess giao tiếp qua giao thức JSON trên stdin/stdout. Đây là cách tích hợp chính cho ứng dụng không phải Node.

```bash
pi --mode rpc --no-session
```

- Đọc JSON command từ stdin
- Ghi JSON event ra stdout
- `ctx.hasUI` là `true` (các yêu cầu UI được phát dưới dạng RPC event để host xử lý)

### JSONL Framing

RPC dùng **strict LF-only JSONL framing**. Client phải tách dòng bằng `\n` only. Không dùng `readline` generic vì sẽ tách nhầm trên Unicode line separators `U+2028` và `U+2029`, gây lỗi parse.

### Ví dụ thực tế

[OpenClaw](https://shittycodingagent.ai) là ví dụ tích hợp thực tế dùng RPC mode để nhúng pi vào ứng dụng web.

### Tài liệu tham khảo

Tài liệu đầy đủ về giao thức RPC: [`docs/rpc.md`](https://github.com/badlogic/pi-mono/blob/main/packages/coding-agent/docs/rpc.md)

## SDK mode

Với ứng dụng Node.js, pi cung cấp API để tạo session và chạy ở bất kỳ chế độ nào mà không cần spawn subprocess.

```typescript
import {
  createAgentSession,
  runPrintMode,
  runRpcMode
} from "@mariozechner/pi-coding-agent";
```

### Tài liệu tham khảo

Tài liệu SDK đầy đủ: [`docs/sdk.md`](https://github.com/badlogic/pi-mono/blob/main/packages/coding-agent/docs/sdk.md)

## `ctx.hasUI` theo chế độ

| Chế độ | `ctx.hasUI` | Ý nghĩa |
|--------|-------------|---------|
| Interactive | `true` | TUI đầy đủ |
| RPC | `true` | Yêu cầu UI phát qua RPC event |
| Print | `false` | Chỉ stdout |
| JSON | `false` | Chỉ JSONL stream |

Khi viết extension, nên kiểm tra `ctx.hasUI` trước khi gọi `ctx.ui.notify()` hoặc `ctx.ui.confirm()`. Các hàm này không làm gì khi `hasUI` là `false`.

## Các pattern thường gặp

### CI pipeline

```bash
pi --no-session -p "Kiểm tra PR này về bảo mật"
```

### Tự động hóa

```bash
for f in src/*.ts; do
  pi --no-session -p "Kiểm tra $f" --mode json >> results.jsonl
done
```

### Tích hợp subprocess

```bash
pi --mode rpc --no-session --model anthropic/claude-sonnet-4-20250514
```

## Đọc tiếp

- [01-commands](../01-commands/README.md)
- [04-extensions](../04-extensions/README.md)
- [06-sessions](../06-sessions/README.md)
- Bản đầy đủ tiếng Anh: [../../12-headless-modes/README.md](../../12-headless-modes/README.md)
