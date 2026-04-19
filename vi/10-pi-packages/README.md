# Gói pi

Gói pi (pi packages) cho phép bạn đóng gói extensions, skills, prompt templates và themes để chia sẻ qua npm, git hoặc đường dẫn cục bộ.

## Vì sao nên dùng gói pi?

- chia sẻ workflow đã đóng gói sẵn
- cài cùng một bộ công cụ chỉ bằng một lệnh
- version hóa cách dùng agent cùng với mã nguồn
- phân phối công cụ riêng của team qua npm hoặc git

## Cài đặt và quản lý

```bash
pi install npm:@foo/bar@1.0.0
pi install git:github.com/user/repo@v1
pi install https://github.com/user/repo
pi install ./relative/path/to/package

pi remove npm:@foo/bar
pi uninstall npm:@foo/bar
pi list
pi update
pi config
```

Mặc định, `pi install` và `pi remove` cập nhật cấu hình toàn cục. Dùng `-l` để cài theo project:

```bash
pi install -l npm:@foo/bar
```

## Các loại nguồn

### npm
- `npm:@scope/pkg@1.2.3`
- `npm:pkg`

### git
- `git:github.com/user/repo@v1`
- `git:git@github.com:user/repo@v1`
- `https://github.com/user/repo@v1`
- `ssh://git@github.com/user/repo@v1`

### Đường dẫn cục bộ
- `/absolute/path/to/package`
- `./relative/path/to/package`

## Tạo package

### Cách 1: khai báo tường minh bằng khóa `pi`

```json
{
  "name": "my-package",
  "keywords": ["pi-package"],
  "pi": {
    "extensions": ["./extensions"],
    "skills": ["./skills"],
    "prompts": ["./prompts"],
    "themes": ["./themes"]
  }
}
```

### Cách 2: theo convention directory

Nếu không có `pi` manifest, pi sẽ tự phát hiện từ:
- `extensions/`
- `skills/`
- `prompts/`
- `themes/`

## Cấu trúc ví dụ

```text
my-package/
├── package.json
├── extensions/
│   └── my-extension.ts
├── skills/
│   └── code-review/
│       └── SKILL.md
├── prompts/
│   └── review.md
└── themes/
    └── my-theme.json
```

## Bật hoặc tắt tài nguyên

Dùng:

```bash
pi config
```

Bạn có thể bật/tắt extension, skill, prompt và theme từ package đã cài.

## Thư viện package cộng đồng

Duyệt package tại [shittycodingagent.ai/packages](https://shittycodingagent.ai/packages) — package gắn tag `pi-package` sẽ hiển thị ở đây.

### Cài package phổ biến

```bash
pi install git:github.com/badlogic/pi-skills  # web search, browser, Google APIs
```

## Bảo mật

> **Cảnh báo:** Gói pi có thể chạy mã với toàn quyền trên hệ thống. Extensions có thể thực thi mã tùy ý và skills có thể hướng dẫn model chạy lệnh. Chỉ cài từ nguồn bạn tin tưởng.

## Đọc tiếp

- [03-skills](../03-skills/README.md)
- [04-extensions](../04-extensions/README.md)
- [05-themes](../05-themes/README.md)
- [09-settings](../09-settings/README.md)
- Bản đầy đủ tiếng Anh: [../../10-pi-packages/README.md](../../10-pi-packages/README.md)
