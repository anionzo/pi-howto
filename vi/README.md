# pi-howto 🇻🇳

<p align="center">
  <img src="../assets/logo/pi-logo.svg" alt="pi logo" width="128">
</p>

> Tài liệu thực hành cho pi coding agent bằng tiếng Việt.

## Repo này có gì?

Bộ tài liệu này giải thích các phần quan trọng của pi:
- commands tương tác
- memory và context files
- skills
- extensions
- themes
- sessions
- keybindings
- providers và models
- settings
- pi packages

## Mục lục

| Phần | Mô tả |
|------|-------|
| [01-commands](01-commands/README.md) | Slash commands, CLI options, editor features |
| [02-memory](02-memory/README.md) | `AGENTS.md`, `KNOWNS.md`, `SYSTEM.md`, `APPEND_SYSTEM.md` |
| [03-skills](03-skills/README.md) | Skill locations, format, validation, workflow skills |
| [04-extensions](04-extensions/README.md) | Extensions TypeScript, events, tools, commands, UI |
| [05-themes](05-themes/README.md) | Theme format, 51 color tokens, hot reload |
| [06-sessions](06-sessions/README.md) | JSONL sessions, branching, compaction |
| [07-keybindings](07-keybindings/README.md) | Tùy biến keyboard shortcuts |
| [08-providers](08-providers/README.md) | OAuth subscriptions, API keys, cloud providers |
| [09-settings](09-settings/README.md) | Settings toàn cục và theo project |
| [10-pi-packages](10-pi-packages/README.md) | Đóng gói và chia sẻ resources |
| [11-prompt-templates](11-prompt-templates/README.md) | Prompt macro tái sử dụng, tham số, ví dụ |

Tham khảo thêm:
- [CATALOG.md](CATALOG.md)
- [QUICK_REFERENCE.md](QUICK_REFERENCE.md)
- [Tiếng Anh](../README.md)

## Quick Start

```bash
npm install -g @mariozechner/pi-coding-agent

# Dùng API key
export ANTHROPIC_API_KEY=sk-ant-...
pi

# Hoặc dùng subscription
pi
/login
```

## Các thao tác đầu tiên nên biết

```bash
/model
/settings
/resume
/tree
/hotkeys
```

## Trạng thái tài liệu tiếng Việt

- [x] README
- [x] 01-commands
- [x] 02-memory
- [x] 03-skills
- [x] 04-extensions
- [x] 05-themes
- [x] 06-sessions
- [x] 07-keybindings
- [x] 08-providers
- [x] 09-settings
- [x] 10-pi-packages
- [x] 11-prompt-templates
- [x] CATALOG.md
- [x] QUICK_REFERENCE.md
