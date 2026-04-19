# Keybindings

All keyboard shortcuts in pi can be customized via `~/.pi/agent/keybindings.json`. Each action can be bound to one or more keys.

After editing `keybindings.json`, run `/reload` to apply changes.

## Key Format

Bindings use the form `modifier+key`.

### Supported modifiers

- `ctrl`
- `shift`
- `alt`

These can be combined:
- `ctrl+shift+x`
- `alt+ctrl+x`
- `ctrl+shift+alt+x`

### Supported keys

- letters: `a-z`
- digits: `0-9`
- specials: `escape`, `esc`, `enter`, `return`, `tab`, `space`, `backspace`, `delete`, `insert`, `clear`, `home`, `end`, `pageUp`, `pageDown`, `up`, `down`, `left`, `right`
- function keys: `f1`-`f12`
- symbols such as `` ` ``, `-`, `=`, `[`, `]`, `\`, `;`, `'`, `,`, `.`, `/`, `!`, `@`, `#`, `$`, `%`, `^`, `&`, `*`, `(`, `)`, `_`, `+`, `|`, `~`, `{`, `}`, `:`, `<`, `>`, `?`

Older non-namespaced config IDs are migrated automatically on startup.

## TUI Editor: Cursor Movement

| Keybinding id | Default | Description |
|---------------|---------|-------------|
| `tui.editor.cursorUp` | `up` | Move cursor up |
| `tui.editor.cursorDown` | `down` | Move cursor down |
| `tui.editor.cursorLeft` | `left`, `ctrl+b` | Move cursor left |
| `tui.editor.cursorRight` | `right`, `ctrl+f` | Move cursor right |
| `tui.editor.cursorWordLeft` | `alt+left`, `ctrl+left`, `alt+b` | Move left by word |
| `tui.editor.cursorWordRight` | `alt+right`, `ctrl+right`, `alt+f` | Move right by word |
| `tui.editor.cursorLineStart` | `home`, `ctrl+a` | Jump to start of line |
| `tui.editor.cursorLineEnd` | `end`, `ctrl+e` | Jump to end of line |
| `tui.editor.jumpForward` | `ctrl+]` | Jump forward to character |
| `tui.editor.jumpBackward` | `ctrl+alt+]` | Jump backward to character |
| `tui.editor.pageUp` | `pageUp` | Scroll editor up |
| `tui.editor.pageDown` | `pageDown` | Scroll editor down |

## TUI Editor: Deletion & Editing

| Keybinding id | Default | Description |
|---------------|---------|-------------|
| `tui.editor.deleteCharBackward` | `backspace` | Delete character backward |
| `tui.editor.deleteCharForward` | `delete`, `ctrl+d` | Delete character forward |
| `tui.editor.deleteWordBackward` | `ctrl+w`, `alt+backspace` | Delete word backward |
| `tui.editor.deleteWordForward` | `alt+d`, `alt+delete` | Delete word forward |
| `tui.editor.deleteToLineStart` | `ctrl+u` | Delete to line start |
| `tui.editor.deleteToLineEnd` | `ctrl+k` | Delete to line end |
| `tui.editor.yank` | `ctrl+y` | Paste/yank last deleted text |
| `tui.editor.yankPop` | `alt+y` | Rotate kill ring after yank |
| `tui.editor.undo` | `ctrl+-` | Undo |

## TUI Input

| Keybinding id | Default | Description |
|---------------|---------|-------------|
| `tui.input.newLine` | `shift+enter` | Insert a new line |
| `tui.input.submit` | `enter` | Submit prompt |
| `tui.input.tab` | `tab` | Autocomplete / tab |
| `tui.input.copy` | `ctrl+c` | Copy selection |

## Selection UI

| Keybinding id | Default | Description |
|---------------|---------|-------------|
| `tui.select.up` | `up` | Move selection up |
| `tui.select.down` | `down` | Move selection down |
| `tui.select.pageUp` | `pageUp` | Page up |
| `tui.select.pageDown` | `pageDown` | Page down |
| `tui.select.confirm` | `enter` | Confirm selection |
| `tui.select.cancel` | `escape`, `ctrl+c` | Cancel selection |

## Application Keys

| Keybinding id | Default | Description |
|---------------|---------|-------------|
| `app.interrupt` | `escape` | Cancel / abort |
| `app.clear` | `ctrl+c` | Clear editor |
| `app.exit` | `ctrl+d` | Exit when editor is empty |
| `app.suspend` | `ctrl+z` | Suspend to background |
| `app.editor.external` | `ctrl+g` | Open external editor (`$VISUAL` / `$EDITOR`) |
| `app.clipboard.pasteImage` | `ctrl+v` (`alt+v` on Windows) | Paste image from clipboard |

## Sessions

| Keybinding id | Default | Description |
|---------------|---------|-------------|
| `app.session.new` | *(none)* | Start a new session |
| `app.session.tree` | *(none)* | Open `/tree` |
| `app.session.fork` | *(none)* | Fork current session |
| `app.session.resume` | *(none)* | Open `/resume` |
| `app.session.togglePath` | `ctrl+p` | Toggle path display in resume selector |
| `app.session.toggleSort` | `ctrl+s` | Toggle sort mode |
| `app.session.toggleNamedFilter` | `ctrl+n` | Toggle named-only filter |
| `app.session.rename` | `ctrl+r` | Rename session |
| `app.session.delete` | `ctrl+d` | Delete session |
| `app.session.deleteNoninvasive` | `ctrl+backspace` | Delete session when query is empty |

## Models & Thinking

| Keybinding id | Default | Description |
|---------------|---------|-------------|
| `app.model.select` | `ctrl+l` | Open model selector |
| `app.model.cycleForward` | `ctrl+p` | Cycle scoped models forward |
| `app.model.cycleBackward` | `shift+ctrl+p` | Cycle scoped models backward |
| `app.thinking.cycle` | `shift+tab` | Cycle thinking level |
| `app.thinking.toggle` | `ctrl+t` | Collapse or expand thinking blocks |

## Display & Message Queue

| Keybinding id | Default | Description |
|---------------|---------|-------------|
| `app.tools.expand` | `ctrl+o` | Collapse or expand tool output |
| `app.message.followUp` | `alt+enter` | Queue follow-up message |
| `app.message.dequeue` | `alt+up` | Restore queued message to editor |

## Tree Navigation

| Keybinding id | Default | Description |
|---------------|---------|-------------|
| `app.tree.foldOrUp` | `ctrl+left`, `alt+left` | Fold branch or move to previous segment |
| `app.tree.unfoldOrDown` | `ctrl+right`, `alt+right` | Unfold branch or move to next segment |
| `app.tree.editLabel` | `shift+l` | Edit selected entry label |
| `app.tree.toggleLabelTimestamp` | `shift+t` | Toggle label timestamps |

## Commonly Used Shortcuts

| Key | Action |
|-----|--------|
| `Ctrl+C` | Clear editor |
| `Escape` | Abort |
| `Escape` twice | Open `/tree` (depending on app behavior/settings) |
| `Ctrl+L` | Model selector |
| `Ctrl+P` | Cycle model |
| `Shift+Tab` | Cycle thinking level |
| `Ctrl+O` | Expand/collapse tool output |
| `Ctrl+T` | Expand/collapse thinking |
| `Ctrl+G` | External editor |

## Known Default Conflicts

`Ctrl+P` is bound to both `app.model.cycleForward` and `app.session.togglePath`. The binding that fires depends on context — `togglePath` only applies inside the `/resume` selector, while `cycleForward` applies in the main editor.

Some terminals also capture `Ctrl+P` for command history. If you experience conflicts, rebind model cycling:

```json
{
  "app.model.cycleForward": ["ctrl+shift+m"],
  "app.model.cycleBackward": ["shift+ctrl+shift+m"]
}
```

## Custom Configuration

Create `~/.pi/agent/keybindings.json`:

```json
{
  "tui.editor.cursorUp": ["up", "ctrl+p"],
  "tui.editor.cursorDown": ["down", "ctrl+n"],
  "tui.editor.deleteWordBackward": ["ctrl+w", "alt+backspace"],
  "app.session.tree": ["ctrl+shift+t"]
}
```

A binding can be a single key or an array of keys. User config overrides defaults.

## Example: Emacs-style Setup

```json
{
  "tui.editor.cursorUp": ["up", "ctrl+p"],
  "tui.editor.cursorDown": ["down", "ctrl+n"],
  "tui.editor.cursorLeft": ["left", "ctrl+b"],
  "tui.editor.cursorRight": ["right", "ctrl+f"],
  "tui.editor.cursorWordLeft": ["alt+left", "alt+b"],
  "tui.editor.cursorWordRight": ["alt+right", "alt+f"],
  "tui.editor.deleteCharForward": ["delete", "ctrl+d"],
  "tui.editor.deleteCharBackward": ["backspace", "ctrl+h"],
  "tui.input.newLine": ["shift+enter", "ctrl+j"]
}
```

## Example: Vim-style Setup

```json
{
  "tui.editor.cursorUp": ["up", "alt+k"],
  "tui.editor.cursorDown": ["down", "alt+j"],
  "tui.editor.cursorLeft": ["left", "alt+h"],
  "tui.editor.cursorRight": ["right", "alt+l"],
  "tui.editor.cursorWordLeft": ["alt+left", "alt+b"],
  "tui.editor.cursorWordRight": ["alt+right", "alt+w"]
}
```

## Related

- [06-sessions](../06-sessions/README.md)
- [09-settings](../09-settings/README.md)
