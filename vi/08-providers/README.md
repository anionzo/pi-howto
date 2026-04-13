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

Xem bản đầy đủ: [../../08-providers/README.md](../../08-providers/README.md)
