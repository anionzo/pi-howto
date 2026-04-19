# Nhà cung cấp và mô hình

Pi hỗ trợ hai nhóm chính:
- nhà cung cấp dùng subscription qua OAuth (`/login`)
- nhà cung cấp dùng API key qua biến môi trường hoặc `auth.json`

## 1) Đăng nhập bằng `/login`

Dùng cách này khi provider hỗ trợ OAuth hoặc subscription-backed access.

Các subscription được hỗ trợ gồm:
- Anthropic Claude Pro / Max
- OpenAI ChatGPT Plus / Pro (Codex)
- GitHub Copilot
- Google Gemini CLI
- Google Antigravity

```bash
pi
/login
```

Dùng `/logout` để xóa thông tin đăng nhập đã lưu.

## 2) Dùng API key

Bạn có thể đặt API key bằng biến môi trường hoặc lưu trong `~/.pi/agent/auth.json`.

### Ví dụ biến môi trường

Ví dụ với OpenAI:

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

### Một số biến môi trường phổ biến

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

## `auth.json`

Thông tin xác thực được lưu tại:

```text
~/.pi/agent/auth.json
```

Ví dụ:

```json
{
  "anthropic": { "type": "api_key", "key": "sk-ant-..." },
  "openai": { "type": "api_key", "key": "sk-..." },
  "google": { "type": "api_key", "key": "..." }
}
```

Trường `key` có thể là:
- giá trị literal
- tên biến môi trường
- shell command bắt đầu bằng `!`

OAuth token sinh ra từ `/login` cũng được lưu ở đây và làm mới tự động khi cần.

## Chọn model

### Trong giao diện tương tác
- `/model`
- `/scoped-models`
- `Ctrl+P` / `Shift+Ctrl+P`
- `Ctrl+L`

### Trên CLI

```bash
pi --provider openai --model gpt-4o
pi --model openai/gpt-4o
pi --model sonnet:high
pi --models "claude-*,gpt-4o"
pi --list-models
```

## Nhà cung cấp đám mây

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

Ví dụ chạy:

```bash
pi --provider amazon-bedrock --model us.anthropic.claude-sonnet-4-20250514-v1:0
```

### Google Vertex AI
- `gcloud auth application-default login`
- `GOOGLE_CLOUD_PROJECT`
- `GOOGLE_CLOUD_LOCATION`
- hoặc `GOOGLE_APPLICATION_CREDENTIALS`

## Thứ tự phân giải thông tin xác thực

Pi ưu tiên theo thứ tự:
1. `--api-key`
2. `auth.json`
3. biến môi trường
4. khóa từ `models.json`

## Nhà cung cấp tùy chỉnh qua `models.json`

File: `~/.pi/agent/models.json`

Dùng cách này khi provider nói một API tương thích mà pi đã hỗ trợ sẵn. File sẽ được nạp lại khi bạn mở `/model`, không cần restart.

### API hỗ trợ

- `openai-completions`
- `openai-responses`
- `anthropic-messages`
- `google-generative-ai`

### Ví dụ tối thiểu với Ollama

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

### Proxy cho provider có sẵn

```json
{
  "providers": {
    "anthropic": {
      "baseUrl": "https://my-proxy.example.com/v1"
    }
  }
}
```

## Khi nào nên dùng tiện ích mở rộng?

Nếu bạn cần:
- OAuth flow riêng
- giao thức streaming không chuẩn
- logic xác thực đặc thù cho doanh nghiệp
- proxy nội bộ phức tạp

thì nên dùng extension thay vì chỉ `models.json`.

## Gói chia sẻ bổ sung

- `pi-share-hf` — chia sẻ session lên Hugging Face thay vì GitHub Gist. Cài bằng `pi install npm:pi-share-hf`.

## Đọc tiếp

- [09-settings](../09-settings/README.md)
- [10-pi-packages](../10-pi-packages/README.md)
- Bản đầy đủ tiếng Anh: [../../08-providers/README.md](../../08-providers/README.md)
