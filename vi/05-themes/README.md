# Giao diện

Giao diện (themes) là các file JSON định nghĩa màu cho TUI của pi. Chúng điều khiển viền, màu tin nhắn, markdown, syntax highlighting, diff và màu viền editor theo mức thinking.

## Giao diện có sẵn

- `dark`
- `light`

Lần chạy đầu tiên, pi có thể chọn mặc định phù hợp theo nền terminal.

## Vị trí nạp theme

Pi nạp theme từ:

- built-in: `dark`, `light`
- `~/.pi/agent/themes/*.json`
- `.pi/themes/*.json`
- package pi
- mảng `themes` trong `settings.json`
- `--theme <path>` trên CLI

Dùng `--no-themes` để tắt tự động phát hiện.

## Chọn giao diện

Cách phổ biến nhất là chọn qua `/settings`.

Hoặc đặt trong `settings.json`:

```json
{
  "theme": "my-theme"
}
```

## Hot reload

Khi bạn chỉnh file theme tùy chỉnh đang được dùng, pi sẽ tự nạp lại ngay. Điều này rất tiện để tinh chỉnh màu trực tiếp.

## Định dạng theme

```json
{
  "$schema": "https://raw.githubusercontent.com/badlogic/pi-mono/main/packages/coding-agent/src/modes/interactive/theme/theme-schema.json",
  "name": "my-theme",
  "vars": {
    "primary": "#00aaff"
  },
  "colors": {
    "accent": "primary",
    "border": "primary",
    "text": ""
  }
}
```

Các trường chính:
- `name` — tên theme, bắt buộc và phải duy nhất
- `vars` — bảng màu tái sử dụng, không bắt buộc
- `colors` — bắt buộc, phải khai báo đủ 51 token
- `$schema` — không bắt buộc nhưng nên có để editor hỗ trợ tốt hơn

## Nhóm token màu

Một theme đầy đủ phải có đủ 51 token, chia thành các nhóm:

- Core UI
- Backgrounds & Content
- Markdown
- Tool Diffs
- Syntax Highlighting
- Thinking Level Borders
- Bash Mode

Ngoài ra có thể có thêm phần `export` để điều khiển màu khi dùng `/export` sang HTML.

## Kiểu giá trị màu

Pi hỗ trợ bốn kiểu giá trị:

- hex: `"#ff0000"`
- 256-color: `39`
- biến trong `vars`: `"primary"`
- màu mặc định của terminal: `""`

## Mẹo sử dụng

- theme nền tối thường cần màu nhấn sáng hơn
- theme nền sáng cần độ tương phản chữ mạnh hơn
- nên thử với markdown, diff, output dài và thinking blocks
- nếu dùng VS Code terminal, đặt `terminal.integrated.minimumContrastRatio = 1` để thấy màu chính xác. VS Code tự điều chỉnh contrast và ghi đè màu theme — setting này tắt cơ chế đó
- nếu hay dùng `/share` để xuất session, nên khai báo phần `export` trong theme JSON để HTML export nhìn khớp với giao diện terminal
---

[← Extensions](../04-extensions/README.md) | [Mục lục](../README.md) | [Phiên làm việc →](../06-sessions/README.md)

### Xem thêm

- [Extensions](../04-extensions/README.md)
- [Cài đặt](../09-settings/README.md)

Bản tiếng Anh: [../../05-themes/README.md](../../05-themes/README.md)
