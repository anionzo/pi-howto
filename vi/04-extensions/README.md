# Extensions

Extensions là module TypeScript mở rộng pi.

## Extensions có thể làm gì?

- đăng ký custom tools
- chặn / sửa tool calls
- thêm slash commands
- thêm keyboard shortcuts
- thêm CLI flags
- hook vào lifecycle events
- tạo custom UI
- lưu state qua session entries
- thêm custom providers

## Vị trí đặt extension

- `~/.pi/agent/extensions/*.ts`
- `~/.pi/agent/extensions/*/index.ts`
- `.pi/extensions/*.ts`
- `.pi/extensions/*/index.ts`
- `-e` / `--extension <path>` để test nhanh

## Imports hay dùng

- `@mariozechner/pi-coding-agent`
- `@sinclair/typebox`
- `@mariozechner/pi-ai`
- `@mariozechner/pi-tui`

## API quan trọng

- `pi.on()`
- `pi.registerTool()`
- `pi.registerCommand()`
- `pi.registerShortcut()`
- `pi.registerFlag()`
- `pi.sendMessage()`
- `pi.sendUserMessage()`
- `pi.appendEntry()`
- `pi.registerProvider()`

## Events nổi bật

- `session_start`
- `resources_discover`
- `input`
- `before_agent_start`
- `tool_call`
- `tool_result`
- `session_before_compact`
- `session_before_tree`
- `model_select`

## UI helpers

- `ctx.ui.notify()`
- `ctx.ui.confirm()`
- `ctx.ui.input()`
- `ctx.ui.select()`
- `ctx.ui.setStatus()`
- `ctx.ui.setWidget()`
- `ctx.ui.custom()`

Xem bản đầy đủ: [../../04-extensions/README.md](../../04-extensions/README.md)
