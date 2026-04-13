# Keybindings

Pi cho phép tùy biến mọi phím tắt qua `~/.pi/agent/keybindings.json`.

Sau khi sửa file, chạy `/reload` để áp dụng.

## Format

Dạng chung: `modifier+key`

Modifiers:
- `ctrl`
- `shift`
- `alt`

Ví dụ:
- `ctrl+shift+x`
- `alt+ctrl+x`
- `ctrl+1`

## Nhóm keybindings chính

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

### Application
- `app.interrupt`
- `app.clear`
- `app.exit`
- `app.suspend`
- `app.editor.external`
- `app.clipboard.pasteImage`

### Sessions
- `app.session.new`
- `app.session.tree`
- `app.session.fork`
- `app.session.resume`
- `app.session.rename`
- `app.session.delete`

### Models & Thinking
- `app.model.select`
- `app.model.cycleForward`
- `app.model.cycleBackward`
- `app.thinking.cycle`
- `app.thinking.toggle`

### Tree Navigation
- `app.tree.foldOrUp`
- `app.tree.unfoldOrDown`
- `app.tree.editLabel`
- `app.tree.toggleLabelTimestamp`

## Ví dụ config

```json
{
  "tui.editor.cursorUp": ["up", "ctrl+p"],
  "tui.editor.cursorDown": ["down", "ctrl+n"],
  "app.session.tree": ["ctrl+shift+t"]
}
```

## Shortcut hay dùng

- `Ctrl+L` - model selector
- `Ctrl+P` - cycle model
- `Shift+Tab` - cycle thinking
- `Ctrl+O` - toggle tool output
- `Ctrl+T` - toggle thinking
- `Ctrl+G` - external editor

Xem bản đầy đủ: [../../07-keybindings/README.md](../../07-keybindings/README.md)
