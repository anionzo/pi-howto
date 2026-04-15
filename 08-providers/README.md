# Providers & Models

Pi supports both subscription-based providers via OAuth and API-key-based providers via environment variables or `auth.json`. For built-in providers, pi ships with curated lists of tool-capable models that are updated with each release.

## Authentication Modes

### 1. Subscription login via `/login`

Use this for providers that support OAuth or subscription-backed access.

Supported subscriptions:
- Anthropic Claude Pro / Max
- OpenAI ChatGPT Plus / Pro (Codex)
- GitHub Copilot
- Google Gemini CLI
- Google Antigravity

```bash
pi
/login
```

Use `/logout` to clear stored credentials.

### 2. API keys

Set a provider key in the environment or `~/.pi/agent/auth.json`.

Example using OpenAI:

**macOS/Linux**

```bash
export OPENAI_API_KEY="sk-..."
pi
```

**Windows PowerShell**

```powershell
$env:OPENAI_API_KEY="sk-..."
pi
```

## Selecting Models

### Interactive

- `/model` - open model selector
- `/scoped-models` - choose which models participate in cycling
- `Ctrl+P` / `Shift+Ctrl+P` - cycle scoped models
- `Ctrl+L` - open model selector directly

### CLI

```bash
pi --provider openai --model gpt-4o
pi --model openai/gpt-4o
pi --model sonnet:high
pi --models "claude-*,gpt-4o"
pi --list-models
```

## Subscriptions

| Provider | Notes |
|---------|-------|
| Claude Pro/Max | Anthropic subscription flow |
| ChatGPT Plus/Pro (Codex) | Great for OpenAI-hosted models |
| GitHub Copilot | Can require model enablement in VS Code |
| Gemini CLI | Standard Gemini access via Google |
| Antigravity | Google sandbox with Gemini / Claude / GPT-OSS models |

### GitHub Copilot notes

- Press Enter for `github.com`, or supply a GitHub Enterprise Server domain
- If pi says a model is unsupported, enable it in VS Code Copilot Chat first

### Google notes

- Gemini CLI and Antigravity are available with a Google account, subject to rate limits
- For paid Cloud Code Assist, set `GOOGLE_CLOUD_PROJECT`

### OpenAI Codex note

ChatGPT Plus/Pro subscription access is intended for personal use. For production workflows, use the OpenAI Platform API instead.

## API Key Providers

### Common environment variables

| Provider | Environment variable | `auth.json` key |
|---------|----------------------|-----------------|
| Anthropic | `ANTHROPIC_API_KEY` | `anthropic` |
| OpenAI | `OPENAI_API_KEY` | `openai` |
| Azure OpenAI Responses | `AZURE_OPENAI_API_KEY` | `azure-openai-responses` |
| Google Gemini | `GEMINI_API_KEY` | `google` |
| Mistral | `MISTRAL_API_KEY` | `mistral` |
| Groq | `GROQ_API_KEY` | `groq` |
| Cerebras | `CEREBRAS_API_KEY` | `cerebras` |
| xAI | `XAI_API_KEY` | `xai` |
| OpenRouter | `OPENROUTER_API_KEY` | `openrouter` |
| Vercel AI Gateway | `AI_GATEWAY_API_KEY` | `vercel-ai-gateway` |
| ZAI | `ZAI_API_KEY` | `zai` |
| OpenCode Zen | `OPENCODE_API_KEY` | `opencode` |
| OpenCode Go | `OPENCODE_API_KEY` | `opencode-go` |
| Hugging Face | `HF_TOKEN` | `huggingface` |
| Kimi For Coding | `KIMI_API_KEY` | `kimi-coding` |
| MiniMax | `MINIMAX_API_KEY` | `minimax` |
| MiniMax China | `MINIMAX_CN_API_KEY` | `minimax-cn` |

## `auth.json`

Credentials are stored in:

```text
~/.pi/agent/auth.json
```

Example:

```json
{
  "anthropic": { "type": "api_key", "key": "sk-ant-..." },
  "openai": { "type": "api_key", "key": "sk-..." },
  "google": { "type": "api_key", "key": "..." }
}
```

Pi creates the file with restrictive permissions (`0600`).

### Key formats supported in `auth.json`

The `key` field can be:

| Format | Example | Meaning |
|--------|---------|---------|
| Shell command | `"!op read 'op://vault/item/credential'"` | Execute and use stdout |
| Environment variable name | `"MY_OPENAI_KEY"` | Resolve from env |
| Literal value | `"sk-ant-..."` | Use directly |

OAuth tokens created through `/login` are also stored here and refreshed automatically.

## Credential Resolution Order

When pi resolves credentials for a provider, it uses this order:

1. CLI `--api-key`
2. `auth.json`
3. Environment variable
4. Custom provider keys from `models.json`

## Cloud Providers

## Azure OpenAI

```bash
export AZURE_OPENAI_API_KEY=...
export AZURE_OPENAI_BASE_URL=https://your-resource.openai.azure.com
# or:
export AZURE_OPENAI_RESOURCE_NAME=your-resource

# optional
export AZURE_OPENAI_API_VERSION=2024-02-01
export AZURE_OPENAI_DEPLOYMENT_NAME_MAP=gpt-4=my-gpt4,gpt-4o=my-gpt4o
```

### Amazon Bedrock

Pi supports several authentication paths.

#### Option 1: AWS profile

```bash
export AWS_PROFILE=your-profile
```

#### Option 2: IAM keys

```bash
export AWS_ACCESS_KEY_ID=AKIA...
export AWS_SECRET_ACCESS_KEY=...
```

#### Option 3: Bearer token

```bash
export AWS_BEARER_TOKEN_BEDROCK=...
```

Optional region:

```bash
export AWS_REGION=us-west-2
```

Run with:

```bash
pi --provider amazon-bedrock --model us.anthropic.claude-sonnet-4-20250514-v1:0
```

Additional Bedrock notes:
- supports ECS task roles and IRSA
- prompt caching is auto-enabled for recognizable Claude model IDs
- for application inference profiles, set `AWS_BEDROCK_FORCE_CACHE=1`
- for proxy setups, use `AWS_ENDPOINT_URL_BEDROCK_RUNTIME`
- use `AWS_BEDROCK_SKIP_AUTH=1` if your proxy does not require auth
- use `AWS_BEDROCK_FORCE_HTTP1=1` if your proxy only supports HTTP/1.1

### Google Vertex AI

Use Application Default Credentials:

```bash
gcloud auth application-default login
export GOOGLE_CLOUD_PROJECT=your-project
export GOOGLE_CLOUD_LOCATION=us-central1
```

Or provide a service account key file through `GOOGLE_APPLICATION_CREDENTIALS`.

## Custom Providers

### Via `models.json`

File: `~/.pi/agent/models.json`

Use this when a provider speaks a supported API. The file reloads each time you open `/model` — no restart needed.

#### Supported APIs

| API | Description |
|-----|-------------|
| `openai-completions` | OpenAI Chat Completions (most compatible) |
| `openai-responses` | OpenAI Responses API |
| `anthropic-messages` | Anthropic Messages API |
| `google-generative-ai` | Google Generative AI |

#### Minimal Example: Ollama

```json
{
  "providers": {
    "ollama": {
      "baseUrl": "http://localhost:11434/v1",
      "api": "openai-completions",
      "apiKey": "ollama",
      "models": [
        { "id": "llama3.1:8b" },
        { "id": "qwen2.5-coder:7b" }
      ]
    }
  }
}
```

`apiKey` is required but Ollama ignores it — any value works.

#### Full Example: Custom Model

```json
{
  "providers": {
    "ollama": {
      "baseUrl": "http://localhost:11434/v1",
      "api": "openai-completions",
      "apiKey": "ollama",
      "models": [
        {
          "id": "llama3.1:8b",
          "name": "Llama 3.1 8B (Local)",
          "reasoning": false,
          "input": ["text"],
          "contextWindow": 128000,
          "maxTokens": 32000,
          "cost": { "input": 0, "output": 0, "cacheRead": 0, "cacheWrite": 0 }
        }
      ]
    }
  }
}
```

#### Model Fields

| Field | Required | Default | Description |
|-------|----------|---------|-------------|
| `id` | Yes | — | Model ID sent to API |
| `name` | No | `id` | Display name in `/model` |
| `api` | No | provider's | Override API per model |
| `reasoning` | No | `false` | Supports extended thinking |
| `input` | No | `["text"]` | `["text"]` or `["text", "image"]` |
| `contextWindow` | No | `128000` | Context window (tokens) |
| `maxTokens` | No | `16384` | Max output tokens |
| `cost` | No | all zeros | Per million tokens |

#### OpenRouter Example

```json
{
  "providers": {
    "openrouter": {
      "baseUrl": "https://openrouter.ai/api/v1",
      "apiKey": "OPENROUTER_API_KEY",
      "api": "openai-completions",
      "models": [
        {
          "id": "anthropic/claude-3.5-sonnet",
          "name": "Claude 3.5 Sonnet (OpenRouter)"
        }
      ]
    }
  }
}
```

#### Google AI Studio Example (Gemma)

```json
{
  "providers": {
    "my-google": {
      "baseUrl": "https://generativelanguage.googleapis.com/v1beta",
      "api": "google-generative-ai",
      "apiKey": "GEMINI_API_KEY",
      "models": [
        {
          "id": "gemma-4-31b-it",
          "name": "Gemma 4 31B",
          "input": ["text", "image"],
          "contextWindow": 262144,
          "reasoning": true
        }
      ]
    }
  }
}
```

#### Proxy an Existing Provider

Route a built-in provider through a proxy without redefining models:

```json
{
  "providers": {
    "anthropic": {
      "baseUrl": "https://my-proxy.example.com/v1"
    }
  }
}
```

All built-in models remain available.

#### API Key Resolution

`apiKey` and `headers` support three formats:

| Format | Example |
|--------|---------|
| Shell command | `"!security find-generic-password -ws 'anthropic'"` |
| Env variable | `"MY_API_KEY"` |
| Literal | `"sk-..."` |

#### OpenAI Compatibility (`compat`)

For servers with partial OpenAI compatibility (Ollama, vLLM, SGLang):

```json
{
  "providers": {
    "ollama": {
      "baseUrl": "http://localhost:11434/v1",
      "api": "openai-completions",
      "apiKey": "ollama",
      "compat": {
        "supportsDeveloperRole": false,
        "supportsReasoningEffort": false
      },
      "models": [{ "id": "llama3.1:8b" }]
    }
  }
}
```

Key compat fields:

| Field | What it does |
|-------|--------------|
| `supportsDeveloperRole` | `false` → send `system` instead of `developer` role |
| `supportsReasoningEffort` | `false` → don't send `reasoning_effort` |
| `maxTokensField` | `"max_tokens"` or `"max_completion_tokens"` |
| `thinkingFormat` | `"reasoning_effort"`, `"qwen"`, `"zai"` |
| `openRouterRouting` | Provider routing preferences for OpenRouter |

### Via extensions

Use an extension when you need:
- custom OAuth flow
- custom streaming behavior
- provider-specific protocol handling
- team-specific proxies and auth logic

Extensions can register a provider dynamically:

```typescript
pi.registerProvider("my-proxy", {
  baseUrl: "https://proxy.example.com",
  apiKey: "PROXY_API_KEY",
  api: "anthropic-messages",
  models: [/* ... */]
});
```

## Useful CLI Patterns

```bash
# start pi with a specific model
pi --model openai/gpt-4o

# pick a reasoning level explicitly
pi --model sonnet:high

# list models matching a query
pi --list-models claude

# restrict model cycling
pi --models "claude-*,gpt-4o"
```

## Troubleshooting

### "No API key" or missing model access

Check:
- environment variable exists
- `auth.json` contains the expected key
- selected provider/model actually supports tool use
- subscription account has access to the chosen model

### OAuth login issues

Try:
- `/logout` then `/login` again
- verifying browser login completed successfully
- checking corporate proxies or SSO restrictions

## Related

- [09-settings](../09-settings/README.md)
- [10-pi-packages](../10-pi-packages/README.md)
- Official docs: `docs/providers.md`, `docs/models.md`, `docs/custom-provider.md`
