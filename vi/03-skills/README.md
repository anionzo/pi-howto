# Skills

Skills là capability packages được load theo nhu cầu.

## Skills hoạt động thế nào?

Pi dùng progressive disclosure:
- metadata (`name`, `description`) luôn có trong context
- `SKILL.md` body chỉ load khi cần
- resources đi kèm được đọc khi workflow yêu cầu

## Vị trí load skill

- `~/.pi/agent/skills/`
- `~/.agents/skills/`
- `.pi/skills/`
- `.agents/skills/` trong project và parent dirs
- package `skills/`
- `settings.json` → `skills`
- CLI `--skill <path>`

## Frontmatter

Bắt buộc:
- `name`
- `description`

Tùy chọn:
- `license`
- `compatibility`
- `metadata`
- `allowed-tools`
- `disable-model-invocation`

## Name rules

- 1-64 ký tự
- chỉ chữ thường, số, dấu `-`
- không bắt đầu / kết thúc bằng `-`
- không có `--`
- phải match tên thư mục cha

## String substitutions

- `$ARGUMENTS`
- `${PI_SKILL_DIR}`
- ``!`command` ``

## Skill commands

```bash
/skill:brave-search
/skill:pdf-tools extract
```

## Built-in workflow skills

- `kn-init`
- `kn-plan`
- `kn-research`
- `kn-spec`
- `kn-implement`
- `kn-verify`
- `kn-doc`
- `kn-commit`

Xem bản đầy đủ: [../../03-skills/README.md](../../03-skills/README.md)
