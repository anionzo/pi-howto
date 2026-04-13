# Memory & Context Files

Pi dùng nhiều lớp để quản lý context:

1. **Context files**: `AGENTS.md`, `SYSTEM.md`, `APPEND_SYSTEM.md`
2. **Knowns / memory layer**: tasks, docs, templates, workflow state
3. **Runtime config**: `settings.json`

## Thứ tự load context

Pi load `AGENTS.md` (hoặc `CLAUDE.md`) từ:
1. `~/.pi/agent/AGENTS.md`
2. parent directories
3. current directory

## Vai trò của `KNOWNS.md`

Trong repo kiểu Knowns:
- `KNOWNS.md` là source of truth cho repo guidance
- `AGENTS.md` thường chỉ là compatibility shim
- nếu `AGENTS.md` và `KNOWNS.md` khác nhau thì theo `KNOWNS.md`

## Knowns CLI tools

```bash
knowns doc list --plain
knowns task list --plain
knowns task view <id> --plain
knowns search "query" --plain
knowns retrieve "query" --json
knowns validate <id-or-path>
```

## MCP tools thường gặp

- `mcp__knowns__get_task`
- `mcp__knowns__update_task`
- `mcp__knowns__create_task`
- `mcp__knowns__get_doc`
- `mcp__knowns__search`
- `mcp__knowns__retrieve`
- `mcp__knowns__validate`

## System prompt files

| File | Vai trò |
|------|---------|
| `SYSTEM.md` | Thay hẳn default system prompt |
| `APPEND_SYSTEM.md` | Nối thêm vào system prompt |
| `AGENTS.md` | Hướng dẫn dự án / workflow |
| `settings.json` | Cấu hình runtime |

## Best practices

- search trước rồi mới read
- chỉ đọc phần liên quan
- follow các ref như `@task-...`, `@doc/...`
- validate trước khi mark done
- không sửa trực tiếp markdown do Knowns quản lý

Xem bản đầy đủ: [../../02-memory/README.md](../../02-memory/README.md)
