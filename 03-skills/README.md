# Skills

Skills are self-contained capability packages that the agent loads on demand. A skill bundles instructions, setup steps, helper scripts, and reference material for a specific task.

Pi implements the [Agent Skills standard](https://agentskills.io/specification) and validates skills leniently.

## How Skills Work

Pi uses progressive disclosure for skills:

1. At startup, pi scans skill locations and reads only metadata such as `name` and `description`
2. Available skills are listed in the system prompt
3. When a task matches, the agent reads the full `SKILL.md`
4. The agent follows the skill instructions and can load bundled files as needed

That means only the short description is always in context. The full skill body is loaded when needed.

## Locations

> **Security:** Skills can instruct the model to run commands and perform arbitrary actions. Review skill content before using it.

Pi loads skills from:

- Global:
  - `~/.pi/agent/skills/`
  - `~/.agents/skills/`
- Project:
  - `.pi/skills/`
  - `.agents/skills/` in the current directory and ancestor directories
- Packages:
  - `skills/` directories
  - `pi.skills` entries in `package.json`
- Settings:
  - `skills` array in `settings.json`
- CLI:
  - `--skill <path>`

### Discovery rules

- In `~/.pi/agent/skills/` and `.pi/skills/`, root `.md` files are discovered as individual skills
- In all skill locations, directories containing `SKILL.md` are discovered recursively
- In `~/.agents/skills/` and project `.agents/skills/`, root `.md` files are ignored
- Use `--no-skills` to disable discovery (explicit `--skill` paths still load)

### Using skills from other harnesses

You can point pi at Claude Code or Codex skill directories through settings:

```json
{
  "skills": [
    "~/.claude/skills",
    "~/.codex/skills"
  ]
}
```

## Skill Commands

Skills register as `/skill:name` commands:

```bash
/skill:brave-search
/skill:pdf-tools extract
```

Arguments after the command are appended to the loaded skill as `User: <args>`.

Enable or disable `/skill:name` commands via `/settings` or `settings.json`:

```json
{
  "enableSkillCommands": true
}
```

## Skill Structure

A skill is a directory containing `SKILL.md`. Everything else is freeform.

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

Use relative paths from the skill directory when referencing bundled files.

## Frontmatter

Per the [Agent Skills specification](https://agentskills.io/specification#frontmatter-required):

| Field | Required | Description |
|-------|----------|-------------|
| `name` | Yes | Max 64 chars. Lowercase a-z, 0-9, hyphens. Must match parent directory. |
| `description` | Yes | Max 1024 chars. Explain what the skill does and when to use it. |
| `license` | No | License name or bundled file reference. |
| `compatibility` | No | Max 500 chars. Environment requirements. |
| `metadata` | No | Arbitrary key-value mapping. |
| `allowed-tools` | No | Space-delimited list of pre-approved tools. |
| `disable-model-invocation` | No | When `true`, the skill is hidden from automatic model invocation and must be called explicitly with `/skill:name`. Use this when a skill has dangerous side effects (API calls, file deletion) and should only be triggered manually. |

### Name rules

- 1-64 characters
- lowercase letters, numbers, and hyphens only
- no leading or trailing hyphen
- no consecutive hyphens
- must match the parent directory name

Valid: `pdf-processing`, `data-analysis`, `code-review`

Invalid: `PDF-Processing`, `-pdf`, `pdf--processing`

### Security example with `allowed-tools` and `disable-model-invocation`

```yaml
---
name: deploy-prod
description: Deploy to production. Only triggered manually via /skill:deploy-prod.
allowed-tools: bash read
disable-model-invocation: true
---
```

- `allowed-tools` restricts which tools the model can use while this skill is active
- `disable-model-invocation` ensures the skill is never auto-loaded by the model

## Validation

Pi validates skills against the Agent Skills standard. Most issues produce warnings but the skill still loads.

Warnings include:
- name does not match parent directory
- name is too long or contains invalid characters
- name starts or ends with `-`
- name contains consecutive hyphens
- description exceeds 1024 characters

Skills with a missing `description` are not loaded.

## Example

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

## Search

\`\`\`bash
./search.js "query"
./search.js "query" --content
\`\`\`
```

## Skill Repositories

- [Anthropic Skills](https://github.com/anthropics/skills) - Document processing and web development skills
- [Pi Skills](https://github.com/badlogic/pi-skills) - Web search, browser automation, Google APIs, transcription
- [Agent Skills Specification](https://agentskills.io/specification) - Full specification of frontmatter fields
- [Community Packages Gallery](https://shittycodingagent.ai/packages) - Browse community-contributed skills and packages
---

[← Memory & Context](../02-memory/README.md) | [Table of Contents](../README.md) | [Extensions →](../04-extensions/README.md)

### See Also

- [Extensions](../04-extensions/README.md)
- [Settings](../09-settings/README.md)
