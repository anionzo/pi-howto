# Phím tắt

Pi cho phép tùy biến toàn bộ phím tắt qua `~/.pi/agent/keybindings.json`. Mỗi hành động có thể gán cho một hoặc nhiều tổ hợp phím.

Sau khi sửa file, dùng `/reload` để áp dụng.

## Định dạng

Phím tắt dùng dạng `modifier+key`.

### Tổ hợp bổ trợ

- `ctrl`
- `shift`
- `alt`

Ví dụ:
- `ctrl+shift+x`
- `alt+ctrl+x`
- `ctrl+1`

### Một số key phổ biến

- chữ cái: `a-z`
- chữ số: `0-9`
- phím đặc biệt: `escape`, `enter`, `tab`, `space`, `backspace`, `delete`, `home`, `end`, `pageUp`, `pageDown`, `up`, `down`, `left`, `right`
- phím chức năng: `f1` đến `f12`

## Các nhóm phím tắt chính

### Editor
- `tui.editor.cursorUp`
- `tui.editor.cursorDown`
- `tui.editor.cursorLeft`
- `tui.editor.cursorRight`
- `tui.editor.deleteWordBackward`
- `tui.editor.deleteToLineEnd`
- `tui.editor.yank`
- `tui.editor.undo`

### Input
- `tui.input.newLine`
- `tui.input.submit`
- `tui.input.tab`

### Ứng dụng
- `app.interrupt`
- `app.clear`
- `app.exit`
- `app.suspend`
- `app.editor.external`
- `app.clipboard.pasteImage`

### Phiên làm việc
- `app.session.new`
- `app.session.tree`
- `app.session.fork`
- `app.session.resume`
- `app.session.rename`
- `app.session.delete`

### Model và thinking
- `app.model.select`
- `app.model.cycleForward`
- `app.model.cycleBackward`
- `app.thinking.cycle`
- `app.thinking.toggle`

### Điều hướng cây session
- `app.tree.foldOrUp`
- `app.tree.unfoldOrDown`
- `app.tree.editLabel`
- `app.tree.toggleLabelTimestamp`

## Ví dụ cấu hình

```json
{
  "tui.editor.cursorUp": ["up", "ctrl+p"],
  "tui.editor.cursorDown": ["down", "ctrl+n"],
  "app.session.tree": ["ctrl+shift+t"]
}
```

## Xung đột phím tắt mặc định

`Ctrl+P` được gán cho cả `app.model.cycleForward` và `app.session.togglePath`. Phím nào kích hoạt phụ thuộc vào ngữ cảnh — `togglePath` chỉ hoạt động trong `/resume`, còn `cycleForward` hoạt động trong editor chính.

Một số terminal cũng dùng `Ctrl+P` cho lịch sử lệnh. Nếu bị xung đột, hãy gán lại:

```json
{
  "app.model.cycleForward": ["ctrl+shift+m"],
  "app.model.cycleBackward": ["shift+ctrl+shift+m"]
}
```

## Một số phím tắt hay dùng

- `Ctrl+L` — mở bảng chọn model
- `Ctrl+P` — chuyển model tiến tới
- `Shift+Ctrl+P` — chuyển model lùi lại
- `Shift+Tab` — đổi mức thinking
- `Ctrl+O` — bật/tắt tool output
- `Ctrl+T` — bật/tắt thinking blocks
- `Ctrl+G` — mở trình soạn thảo ngoài
- `Escape` — hủy thao tác hiện tại
---

[← Phiên làm việc](../06-sessions/README.md) | [Mục lục](../README.md) | [Nhà cung cấp →](../08-providers/README.md)

### Xem thêm

- [Phiên làm việc](../06-sessions/README.md)
- [Cài đặt](../09-settings/README.md)

Bản tiếng Anh: [../../07-keybindings/README.md](../../07-keybindings/README.md)
