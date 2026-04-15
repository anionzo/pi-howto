# Mẫu prompt

Mẫu prompt (prompt templates) là các file Markdown có thể tái sử dụng và mở rộng thành prompt đầy đủ. Bạn gõ `/tên` trong editor để gọi mẫu, trong đó `tên` là tên file không có `.md`.

Có thể hiểu đơn giản đây là **macro cho những yêu cầu bạn dùng lặp đi lặp lại**.

## Cách hoạt động

1. Bạn tạo file `.md` chứa mô tả và hướng dẫn
2. Pi tự phát hiện khi khởi động
3. Bạn gõ `/tên-file` trong editor
4. Pi mở rộng nội dung đó thành prompt hoàn chỉnh và gửi cho agent

## Vị trí lưu trữ

| Vị trí | Phạm vi |
|--------|---------|
| `~/.pi/agent/prompts/*.md` | Toàn cục |
| `.pi/prompts/*.md` | Theo project |
| package pi | Qua `prompts/` hoặc `pi.prompts` trong `package.json` |
| `settings.json` | Mảng `prompts` |
| CLI | `--prompt-template <path>` |

Pi chỉ quét các file `.md` ở thư mục gốc của thư mục prompts, không quét đệ quy. Dùng `--no-prompt-templates` để tắt tự động phát hiện.

## Định dạng

Một template là file Markdown có thể có YAML frontmatter:

```markdown
---
description: Review staged git changes
---
Review the staged changes (`git diff --cached`). Focus on:
- Bugs and logic errors
- Security issues
- Error handling gaps
```

Quy ước chính:
- tên file trở thành tên lệnh: `review.md` → `/review`
- `description` là tùy chọn; nếu không có, pi dùng dòng không rỗng đầu tiên
- phần body là prompt gửi cho agent

## Tham số

Template hỗ trợ tham số vị trí:

| Cú pháp | Ý nghĩa |
|---------|---------|
| `$1`, `$2`, ... | Tham số theo vị trí |
| `$@` hoặc `$ARGUMENTS` | Toàn bộ tham số nối lại |
| `${@:N}` | Tham số từ vị trí N trở đi |
| `${@:N:L}` | L tham số bắt đầu từ vị trí N |

Ví dụ (`component.md`):

```markdown
---
description: Tạo React component
---
Create a React component named $1 with these features: ${@:2}
```

Cách gọi:

```text
/component Button "onClick handler" "disabled support"
```

## Cách dùng

Gõ `/` rồi tên template. Autocomplete sẽ hiện các mẫu khả dụng cùng mô tả.

```text
/review
/component Button
/component Button "click handler"
```

## Ví dụ thường gặp

### Review code (`review.md`)

```markdown
---
description: Review staged git changes for bugs and issues
---
Review the staged changes (`git diff --cached`). Focus on:
- Bugs and logic errors
- Security issues
- Error handling gaps
- Performance concerns

Be concise. Only flag real problems.
```

### Commit message (`commit.md`)

```markdown
---
description: Generate a commit message from staged changes
---
Look at the staged changes (`git diff --cached`) and write a conventional commit message.

Format: `type(scope): description`
```

### Dịch file (`translate.md`)

```markdown
---
description: Translate a file to another language
---
Translate the file $1 to $2.

Rules:
- Keep all code blocks, links, and formatting unchanged
- Keep technical terms in English where appropriate
- Natural, fluent translation
```

## Mẹo dùng tốt

- giữ template ngắn, rõ và tập trung
- dùng `.pi/prompts/` cho workflow riêng của project
- dùng `~/.pi/agent/prompts/` cho shortcut cá nhân
- có thể kết hợp template với skill khi workflow lặp lại có nhiều bước
- không cần restart; template được nạp lại khi mở autocomplete

## Đọc tiếp

- [03-skills](../03-skills/README.md)
- [10-pi-packages](../10-pi-packages/README.md)
- Bản đầy đủ tiếng Anh: [../../11-prompt-templates/README.md](../../11-prompt-templates/README.md)
