# Kỹ năng

Kỹ năng (skills) là các gói khả năng tự chứa mà agent nạp khi cần. Một kỹ năng thường gói sẵn hướng dẫn, bước chuẩn bị, script hỗ trợ và tài liệu tham chiếu cho một loại công việc cụ thể.

Pi triển khai theo [chuẩn Agent Skills](https://agentskills.io/specification) và kiểm tra hợp lệ theo hướng mềm dẻo.

## Kỹ năng hoạt động như thế nào?

Pi dùng cơ chế nạp theo nhu cầu:

1. Khi khởi động, pi quét nơi chứa kỹ năng và chỉ đọc metadata như `name` và `description`
2. Danh sách kỹ năng khả dụng được đưa vào system prompt
3. Khi công việc phù hợp, agent mới đọc toàn bộ `SKILL.md`
4. Nếu cần, agent tiếp tục đọc script hoặc tài nguyên đi kèm

Điều này giúp chỉ phần mô tả ngắn luôn nằm trong ngữ cảnh, còn nội dung chi tiết chỉ được nạp khi thật sự cần.

## Vị trí nạp kỹ năng

> **Bảo mật:** Kỹ năng có thể hướng dẫn model chạy lệnh hoặc thực hiện hành động trên hệ thống. Hãy xem nội dung trước khi dùng.

Pi nạp kỹ năng từ:

- Toàn cục:
  - `~/.pi/agent/skills/`
  - `~/.agents/skills/`
- Theo project:
  - `.pi/skills/`
  - `.agents/skills/` trong thư mục hiện tại và các thư mục cha
- Gói cài đặt:
  - thư mục `skills/`
  - mục `pi.skills` trong `package.json`
- Cài đặt:
  - mảng `skills` trong `settings.json`
- CLI:
  - `--skill <path>`

### Quy tắc phát hiện

- Trong `~/.pi/agent/skills/` và `.pi/skills/`, các file `.md` ở thư mục gốc được xem là kỹ năng riêng lẻ
- Trong mọi nơi chứa kỹ năng, các thư mục có `SKILL.md` được phát hiện đệ quy
- Trong `~/.agents/skills/` và `.agents/skills/`, file `.md` ở thư mục gốc bị bỏ qua
- Dùng `--no-skills` để tắt tự động phát hiện (các đường dẫn chỉ định bằng `--skill` vẫn hoạt động)

### Dùng kỹ năng từ hệ khác

Bạn có thể trỏ pi tới thư mục kỹ năng của Claude Code hoặc Codex qua `settings.json`:

```json
{
  "skills": [
    "~/.claude/skills",
    "~/.codex/skills"
  ]
}
```

## Lệnh gọi kỹ năng

Kỹ năng đăng ký dưới dạng `/skill:name`:

```bash
/skill:brave-search
/skill:pdf-tools extract
```

Phần tham số sau tên lệnh sẽ được gắn thêm vào nội dung kỹ năng dưới dạng `User: <args>`.

Bật hoặc tắt kiểu lệnh này qua `/settings` hoặc `settings.json`:

```json
{
  "enableSkillCommands": true
}
```

## Cấu trúc kỹ năng

Một kỹ năng thường là một thư mục có `SKILL.md`. Những phần còn lại là tự do tổ chức.

```text
my-skill/
├── SKILL.md
├── scripts/
│   └── process.sh
├── references/
│   └── api-reference.md
└── assets/
    └── template.json
```

Khi tham chiếu tới file đi kèm, nên dùng đường dẫn tương đối tính từ thư mục kỹ năng.

## Frontmatter

Theo [chuẩn Agent Skills](https://agentskills.io/specification#frontmatter-required):

| Trường | Bắt buộc | Ý nghĩa |
|--------|----------|--------|
| `name` | Có | Tối đa 64 ký tự. Chỉ gồm chữ thường a-z, số và dấu gạch ngang. Phải trùng tên thư mục cha. |
| `description` | Có | Tối đa 1024 ký tự. Nêu rõ kỹ năng làm gì và khi nào nên dùng. |
| `license` | Không | Tên giấy phép hoặc tham chiếu tới file kèm theo. |
| `compatibility` | Không | Tối đa 500 ký tự. Yêu cầu môi trường. |
| `metadata` | Không | Dữ liệu tùy ý dạng key-value. |
| `allowed-tools` | Không | Danh sách công cụ được duyệt trước, phân tách bằng khoảng trắng. |
| `disable-model-invocation` | Không | Nếu là `true`, model không tự gọi kỹ năng này; người dùng phải gọi bằng `/skill:name`. |

### Quy tắc đặt tên

- 1-64 ký tự
- chỉ gồm chữ thường, số và `-`
- không bắt đầu hoặc kết thúc bằng `-`
- không có `--`
- phải trùng tên thư mục cha

Hợp lệ: `pdf-processing`, `data-analysis`, `code-review`

Không hợp lệ: `PDF-Processing`, `-pdf`, `pdf--processing`

## Kiểm tra hợp lệ

Pi sẽ kiểm tra kỹ năng theo chuẩn Agent Skills. Phần lớn lỗi chỉ tạo cảnh báo nhưng kỹ năng vẫn được nạp.

Các cảnh báo thường gặp:
- tên không khớp thư mục cha
- tên quá dài hoặc có ký tự không hợp lệ
- tên bắt đầu hoặc kết thúc bằng `-`
- tên có `--`
- mô tả dài quá 1024 ký tự

Nếu thiếu `description`, kỹ năng sẽ không được nạp.

## Ví dụ

```text
brave-search/
├── SKILL.md
├── search.js
└── content.js
```

**SKILL.md**

```markdown
---
name: brave-search
description: Web search and content extraction via Brave Search API. Use for searching documentation, facts, or any web content.
---

# Brave Search

## Setup

\`\`\`bash
cd /path/to/brave-search && npm install
\`\`\`
```

## Kho kỹ năng tham khảo

- [Anthropic Skills](https://github.com/anthropics/skills) - Kỹ năng xử lý tài liệu và web development
- [Pi Skills](https://github.com/badlogic/pi-skills) - Tìm kiếm web, tự động hóa trình duyệt, Google APIs, transcription

## Đọc tiếp

- [04-extensions](../04-extensions/README.md)
- [09-settings](../09-settings/README.md)
- Bản đầy đủ tiếng Anh: [../../03-skills/README.md](../../03-skills/README.md)
