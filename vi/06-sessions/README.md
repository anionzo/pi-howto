# Sessions

Pi lưu session dưới dạng JSONL với tree structure.

## CLI options

```bash
pi -c
pi -r
pi --no-session
pi --session <path>
pi --fork <path>
pi --session-dir <dir>
```

## Commands trong app

- `/new`
- `/resume`
- `/session`
- `/tree`
- `/fork`
- `/compact`
- `/export`
- `/share`

## File location

```text
~/.pi/agent/sessions/--<path>--/<timestamp>_<uuid>.jsonl
```

## Session versions

- v1: linear
- v2: tree
- v3: `hookMessage` đổi thành `custom`

## Entry types quan trọng

- `session`
- `message`
- `model_change`
- `thinking_level_change`
- `compaction`
- `branch_summary`
- `custom`
- `custom_message`
- `label`
- `session_info`

## `/tree`

Cho phép di chuyển trong cây session:
- search by typing
- fold/unfold branch
- label bằng `Shift+L`
- toggle timestamp bằng `Shift+T`
- filter modes bằng `Ctrl+O`

## `/fork`

Tạo session file mới từ một branch hiện tại hoặc từ session/path được chỉ định.

## Compaction

- manual: `/compact`
- automatic: khi gần hết context hoặc overflow
- full history vẫn còn trong JSONL file

Xem bản đầy đủ: [../../06-sessions/README.md](../../06-sessions/README.md)
