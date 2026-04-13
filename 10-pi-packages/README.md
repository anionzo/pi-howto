# Pi Packages

Pi packages bundle extensions, skills, prompt templates, and themes so you can share workflows through npm or git. A package can declare resources explicitly under the `pi` key in `package.json`, or rely on conventional directories.

## Why Pi Packages?

- share your extensions, skills, prompts, and themes
- install reusable workflows with one command
- version your agent setup alongside code
- distribute project-specific tooling through npm, git, or local paths

## Install & Manage

```bash
pi install npm:@foo/bar@1.0.0
pi install git:github.com/user/repo@v1
pi install https://github.com/user/repo
pi install /absolute/path/to/package
pi install ./relative/path/to/package

pi remove npm:@foo/bar
pi uninstall npm:@foo/bar   # alias for remove
pi list
pi update
pi config
```

### Global vs project install

By default, `pi install` and `pi remove` update global settings in `~/.pi/agent/settings.json`.

Use `-l` for project-local install:

```bash
pi install -l npm:@foo/bar
```

Project installs are useful when you want team members to share the same setup through `.pi/settings.json`.

## Package Sources

Pi accepts three source types.

### 1. npm

```text
npm:@scope/pkg@1.2.3
npm:pkg
```

Notes:
- pinned versions are skipped by `pi update`
- global installs use `npm install -g`
- project installs live under `.pi/npm/`
- use `npmCommand` in `settings.json` to wrap npm through tools like `mise` or `asdf`

Example:

```json
{
  "npmCommand": ["mise", "exec", "node@20", "--", "npm"]
}
```

### 2. git

```text
git:github.com/user/repo@v1
git:git@github.com:user/repo@v1
https://github.com/user/repo@v1
ssh://git@github.com/user/repo@v1
```

Notes:
- refs pin the package and skip `pi update`
- global git packages go under `~/.pi/agent/git/<host>/<path>`
- project git packages go under `.pi/git/<host>/<path>`
- pi runs `npm install` after clone/pull when `package.json` exists
- supports SSH URLs and your configured SSH keys

Helpful env vars for CI/non-interactive git installs:
- `GIT_TERMINAL_PROMPT=0`
- `GIT_SSH_COMMAND="ssh -o BatchMode=yes -o ConnectTimeout=5"`

### 3. Local paths

```text
/absolute/path/to/package
./relative/path/to/package
```

Local paths are not copied. Pi references them directly from disk.

## Creating a Package

### Option A: explicit `pi` manifest

Add a `pi` key to `package.json`:

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

Paths are relative to the package root.

### Option B: convention directories

If there is no `pi` manifest, pi auto-discovers resources from:

- `extensions/`
- `skills/`
- `prompts/`
- `themes/`

## Package Structure

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

## Gallery Metadata

Packages tagged with `pi-package` can appear in the package gallery. Add preview metadata:

```json
{
  "name": "my-package",
  "keywords": ["pi-package"],
  "pi": {
    "extensions": ["./extensions"],
    "video": "https://example.com/demo.mp4",
    "image": "https://example.com/screenshot.png"
  }
}
```

- `video` supports MP4
- `image` supports PNG, JPEG, GIF, WebP
- if both are set, video wins

## Dependencies

### Normal dependencies

Put third-party runtime dependencies in `dependencies`.

### Core pi packages

If you import any of these, list them in `peerDependencies` with `"*"` and do not bundle them:
- `@mariozechner/pi-ai`
- `@mariozechner/pi-agent-core`
- `@mariozechner/pi-coding-agent`
- `@mariozechner/pi-tui`
- `@sinclair/typebox`

### Bundling other pi packages

If your package depends on another pi package, add it to both `dependencies` and `bundledDependencies`, then reference resources through `node_modules/...` paths.

Example:

```json
{
  "dependencies": {
    "shitty-extensions": "^1.0.1"
  },
  "bundledDependencies": ["shitty-extensions"],
  "pi": {
    "extensions": ["extensions", "node_modules/shitty-extensions/extensions"],
    "skills": ["skills", "node_modules/shitty-extensions/skills"]
  }
}
```

## Package Filtering

Settings support an object form to narrow what resources load from a package.

```json
{
  "packages": [
    "npm:simple-pkg",
    {
      "source": "npm:my-package",
      "extensions": ["extensions/*.ts", "!extensions/legacy.ts"],
      "skills": [],
      "prompts": ["prompts/review.md"],
      "themes": ["+themes/legacy.json"]
    }
  ]
}
```

Rules:
- omit a key to load all resources of that type
- use `[]` to load none of that type
- `!pattern` excludes matches
- `+path` force-includes an exact path
- `-path` force-excludes an exact path
- filters only narrow what the manifest already exposes

## Enable / Disable Resources

Use:

```bash
pi config
```

This lets you enable or disable extensions, skills, prompts, and themes from installed packages and local directories.

## Scope & Deduplication

Packages can appear in both global and project settings. If the same package appears in both places, the project entry wins.

Identity is determined by:
- npm: package name
- git: repository URL without ref
- local: resolved absolute path

## Security

> **Warning:** Pi packages run with full system access. Extensions can execute arbitrary code and skills can instruct the model to run commands. Review source before installing third-party packages.

## Related

- [03-skills](../03-skills/README.md)
- [04-extensions](../04-extensions/README.md)
- [05-themes](../05-themes/README.md)
- [09-settings](../09-settings/README.md)
