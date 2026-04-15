# Providers & Models

Pi hỗ trợ 2 nhóm chính:
- subscription providers qua OAuth (`/login`)
- API key providers qua env vars hoặc `auth.json`

## Subscription providers

- Claude Pro / Max
- ChatGPT Plus / Pro (Codex)
- GitHub Copilot
- Google Gemini CLI
- Google Antigravity

```bash
pi
/login
```

## API key providers

Một số env vars phổ biến:
- `ANTHROPIC_API_KEY`
- `OPENAI_API_KEY`
- `AZURE_OPENAI_API_KEY`
- `GEMINI_API_KEY`
- `MISTRAL_API_KEY`
- `GROQ_API_KEY`
- `CEREBRAS_API_KEY`
- `XAI_API_KEY`
- `OPENROUTER_API_KEY`
- `HF_TOKEN`

## auth.json

```text
~/.pi/agent/auth.json
```

Ví dụ:

```json
{
  "anthropic": { "type": "api_key", "key": "sk-ant-..." },
  "openai": { "type": "api_key", "key": "sk-..." }
}
```

## Chọn model

- `/model`
- `/scoped-models`
- `Ctrl+P` / `Shift+Ctrl+P`
- `Ctrl+L`

CLI:

```bash
pi --provider openai --model gpt-4o
pi --model openai/gpt-4o
pi --model sonnet:high
```

## Cloud providers

### Azure OpenAI
- `AZURE_OPENAI_API_KEY`
- `AZURE_OPENAI_BASE_URL`
- `AZURE_OPENAI_RESOURCE_NAME`
- `AZURE_OPENAI_DEPLOYMENT_NAME_MAP`

### Amazon Bedrock
- `AWS_PROFILE`
- `AWS_ACCESS_KEY_ID`
- `AWS_SECRET_ACCESS_KEY`
- `AWS_BEARER_TOKEN_BEDROCK`
- `AWS_REGION`

### Vertex AI
- `gcloud auth application-default login`
- `GOOGLE_CLOUD_PROJECT`
- `GOOGLE_CLOUD_LOCATION`

## Credential resolution order

1. `--api-key`
2. `auth.json`
3. environment variable
4. `models.json`

## Custom Providers qua `models.json`

File: `~/.pi/agent/models.json` — reload tự động khi mở `/model`, không cần restart.

### Ollama (ví dụ nhanh nhất)

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

### OpenRouter

```json
{
  "providers": {
    "openrouter": {
      "baseUrl": "https://openrouter.ai/api/v1",
      "apiKey": "OPENROUTER_API_KEY",
      "api": "openai-completions",
      "models": [
        { "id": "anthropic/claude-3.5-sonnet", "name": "Claude 3.5 (OpenRouter)" }
      ]
    }
  }
}
```

### Proxy provider có sẵn

```json
{
  "providers": {
    "anthropic": {
      "baseUrl": "https://my-proxy.example.com/v1"
    }
  }
}
```

Giữ nguyên tất cả model built-in, chỉ đổi endpoint.

Chi tiết đầy đủ (model fields, compat, API key resolution): xem [bản English](../../08-providers/README.md#custom-providers).

Xem bản đầy đủ: [../../08-providers/README.md](../../08-providers/README.md)
