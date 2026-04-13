# Pi Packages

Pi packages dùng để bundle extensions, skills, prompt templates, và themes để chia sẻ qua npm hoặc git.

## Install & Manage

```bash
pi install npm:@foo/bar@1.0.0
pi install git:github.com/user/repo@v1
pi install https://github.com/user/repo
pi install ./relative/path

pi remove npm:@foo/bar
pi uninstall npm:@foo/bar
pi list
pi update
pi config
```

## Các loại source

### npm
- `npm:@scope/pkg@1.2.3`
- `npm:pkg`

### git
- `git:github.com/user/repo@v1`
- `git:git@github.com:user/repo@v1`
- `https://github.com/user/repo@v1`
- `ssh://git@github.com/user/repo@v1`

### local path
- `/absolute/path/to/package`
- `./relative/path/to/package`

## Tạo package

Thêm `pi` key vào `package.json`:

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

## Convention directories

Nếu không có `pi` manifest, pi auto-discover từ:
- `extensions/`
- `skills/`
- `prompts/`
- `themes/`

## Security

Pi packages có full system access. Chỉ cài từ nguồn bạn tin tưởng.

Xem bản đầy đủ: [../../10-pi-packages/README.md](../../10-pi-packages/README.md)
