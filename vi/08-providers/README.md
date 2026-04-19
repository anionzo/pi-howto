# Nhà cung cấp & Mô hình

Pi hỗ trợ cả nhà cung cấp dùng subscription qua OAuth lẫn nhà cung cấp dùng API key qua biến môi trường hoặc `auth.json`. Với các provider tích hợp sẵn, pi đi kèm danh sách model hỗ trợ tool được cập nhật theo từng phiên bản.

## Chế độ xác thực

### 1. Đăng nhập subscription qua `/login`

Dùng cách này khi provider hỗ trợ OAuth hoặc subscription-backed access.

Các subscription được hỗ trợ:
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

### 2. API key

Đặt API key trong biến môi trường hoặc `~/.pi/agent/auth.json`.

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

## Chọn model

### Trong giao diện tương tác

- `/model` - mở bộ chọn model
- `/scoped-models` - chọn model nào tham gia vòng xoay
- `Ctrl+P` / `Shift+Ctrl+P` - chuyển đổi model trong scope
- `Ctrl+L` - mở bộ chọn model trực tiếp

### Trên CLI

```bash
pi --provider openai --model gpt-4o
pi --model openai/gpt-4o
pi --model sonnet:high
pi --models "claude-*,gpt-4o"
pi --list-models
```

## Subscription

| Nhà cung cấp | Ghi chú |
|---------|-------|
| Claude Pro/Max | Luồng subscription của Anthropic |
| ChatGPT Plus/Pro (Codex) | Phù hợp cho model do OpenAI host |
| GitHub Copilot | Có thể cần bật model trong VS Code |
| Gemini CLI | Truy cập Gemini tiêu chuẩn qua Google |
| Antigravity | Sandbox của Google với các model Gemini / Claude / GPT-OSS |

### Lưu ý GitHub Copilot

- Nhấn Enter để chọn `github.com`, hoặc nhập domain GitHub Enterprise Server
- Nếu pi báo model không được hỗ trợ, hãy bật model đó trong VS Code Copilot Chat trước

### Lưu ý Google

- Gemini CLI và Antigravity khả dụng với tài khoản Google, có giới hạn rate limit
- Với Cloud Code Assist trả phí, đặt `GOOGLE_CLOUD_PROJECT`

### Lưu ý OpenAI Codex

Truy cập qua subscription ChatGPT Plus/Pro dành cho sử dụng cá nhân. Với workflow production, hãy dùng OpenAI Platform API.

## Nhà cung cấp dùng API Key

### Biến môi trường phổ biến

| Nhà cung cấp | Biến môi trường | Khóa `auth.json` |
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

Pi tạo file với quyền hạn chế (`0600`).

### Định dạng khóa hỗ trợ trong `auth.json`

Trường `key` có thể là:

| Định dạng | Ví dụ | Ý nghĩa |
|--------|---------|---------|
| Shell command | `"!op read 'op://vault/item/credential'"` | Thực thi và dùng stdout |
| Tên biến môi trường | `"MY_OPENAI_KEY"` | Lấy từ biến môi trường |
| Giá trị trực tiếp | `"sk-ant-..."` | Dùng trực tiếp |

OAuth token sinh ra từ `/login` cũng được lưu ở đây và tự động làm mới khi cần.

## Thứ tự phân giải thông tin xác thực

Khi pi phân giải thông tin xác thực cho một provider, thứ tự ưu tiên là:

1. CLI `--api-key`
2. `auth.json`
3. Biến môi trường
4. Khóa custom provider từ `models.json`

## Nhà cung cấp đám mây

## Azure OpenAI

```bash
export AZURE_OPENAI_API_KEY=...
export AZURE_OPENAI_BASE_URL=https://your-resource.openai.azure.com
# hoặc:
export AZURE_OPENAI_RESOURCE_NAME=your-resource

# tùy chọn
export AZURE_OPENAI_API_VERSION=2024-02-01
export AZURE_OPENAI_DEPLOYMENT_NAME_MAP=gpt-4=my-gpt4,gpt-4o=my-gpt4o
```

### Amazon Bedrock

Pi hỗ trợ nhiều phương thức xác thực.

#### Cách 1: AWS profile

```bash
export AWS_PROFILE=your-profile
```

#### Cách 2: IAM key

```bash
export AWS_ACCESS_KEY_ID=AKIA...
export AWS_SECRET_ACCESS_KEY=...
```

#### Cách 3: Bearer token

```bash
export AWS_BEARER_TOKEN_BEDROCK=...
```

Region tùy chọn:

```bash
export AWS_REGION=us-west-2
```

Chạy với:

```bash
pi --provider amazon-bedrock --model us.anthropic.claude-sonnet-4-20250514-v1:0
```

Lưu ý thêm về Bedrock:
- hỗ trợ ECS task role và IRSA
- prompt caching tự động bật cho các model ID Claude nhận diện được
- với application inference profile, đặt `AWS_BEDROCK_FORCE_CACHE=1`
- với proxy, dùng `AWS_ENDPOINT_URL_BEDROCK_RUNTIME`
- dùng `AWS_BEDROCK_SKIP_AUTH=1` nếu proxy không yêu cầu auth
- dùng `AWS_BEDROCK_FORCE_HTTP1=1` nếu proxy chỉ hỗ trợ HTTP/1.1

### Google Vertex AI

Dùng Application Default Credentials:

```bash
gcloud auth application-default login
export GOOGLE_CLOUD_PROJECT=your-project
export GOOGLE_CLOUD_LOCATION=us-central1
```

Hoặc cung cấp file service account key qua `GOOGLE_APPLICATION_CREDENTIALS`.

## Nhà cung cấp tùy chỉnh

### Qua `models.json`

File: `~/.pi/agent/models.json`

Dùng cách này khi provider nói một API mà pi đã hỗ trợ sẵn. File sẽ được nạp lại khi bạn mở `/model` — không cần restart.

#### API hỗ trợ

| API | Mô tả |
|-----|-------------|
| `openai-completions` | OpenAI Chat Completions (tương thích nhất) |
| `openai-responses` | OpenAI Responses API |
| `anthropic-messages` | Anthropic Messages API |
| `google-generative-ai` | Google Generative AI |

#### Ví dụ tối thiểu: Ollama

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

`apiKey` là bắt buộc nhưng Ollama bỏ qua nó — giá trị gì cũng được.

#### Ví dụ đầy đủ: Custom Model

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

#### Các trường model

| Trường | Bắt buộc | Mặc định | Mô tả |
|-------|----------|---------|-------------|
| `id` | Có | — | Model ID gửi đến API |
| `name` | Không | `id` | Tên hiển thị trong `/model` |
| `api` | Không | của provider | Ghi đè API cho từng model |
| `reasoning` | Không | `false` | Hỗ trợ extended thinking |
| `input` | Không | `["text"]` | `["text"]` hoặc `["text", "image"]` |
| `contextWindow` | Không | `128000` | Cửa sổ ngữ cảnh (token) |
| `maxTokens` | Không | `16384` | Số token đầu ra tối đa |
| `cost` | Không | tất cả bằng 0 | Giá mỗi triệu token |

#### Ví dụ OpenRouter

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

#### Ví dụ Google AI Studio (Gemma)

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

#### Proxy cho provider có sẵn

Route provider tích hợp sẵn qua proxy mà không cần định nghĩa lại model:

```json
{
  "providers": {
    "anthropic": {
      "baseUrl": "https://my-proxy.example.com/v1"
    }
  }
}
```

Tất cả model tích hợp sẵn vẫn khả dụng.

#### Phân giải API Key

`apiKey` và `headers` hỗ trợ ba định dạng:

| Định dạng | Ví dụ |
|--------|---------|
| Shell command | `"!security find-generic-password -ws 'anthropic'"` |
| Biến môi trường | `"MY_API_KEY"` |
| Giá trị trực tiếp | `"sk-..."` |

#### Tương thích OpenAI (`compat`)

Dùng cho các server tương thích OpenAI một phần (Ollama, vLLM, SGLang):

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

Các trường compat chính:

| Trường | Tác dụng |
|-------|--------------|
| `supportsDeveloperRole` | `false` → gửi `system` thay vì `developer` role |
| `supportsReasoningEffort` | `false` → không gửi `reasoning_effort` |
| `maxTokensField` | `"max_tokens"` hoặc `"max_completion_tokens"` |
| `thinkingFormat` | `"reasoning_effort"`, `"qwen"`, `"zai"` |
| `requiresToolResultName` | `true` → đính kèm trường `name` trong kết quả tool (một số provider yêu cầu) |
| `openRouterRouting` | Tùy chọn routing cho OpenRouter |

### Qua tiện ích mở rộng (extension)

Dùng extension khi bạn cần:
- OAuth flow riêng
- hành vi streaming tùy chỉnh
- xử lý giao thức đặc thù cho provider
- proxy và logic xác thực nội bộ doanh nghiệp

Extension có thể đăng ký provider động:

```typescript
pi.registerProvider("my-proxy", {
  baseUrl: "https://proxy.example.com",
  apiKey: "PROXY_API_KEY",
  api: "anthropic-messages",
  models: [/* ... */]
});
```

## Mẫu CLI hữu ích

```bash
# khởi động pi với model cụ thể
pi --model openai/gpt-4o

# chọn mức reasoning rõ ràng
pi --model sonnet:high

# liệt kê model theo từ khóa
pi --list-models claude

# giới hạn vòng xoay model
pi --models "claude-*,gpt-4o"
```

## Xử lý sự cố

### "No API key" hoặc không truy cập được model

Kiểm tra:
- biến môi trường tồn tại
- `auth.json` chứa khóa đúng
- provider/model đã chọn thực sự hỗ trợ tool use
- tài khoản subscription có quyền truy cập model đã chọn

### Vấn đề đăng nhập OAuth

Thử:
- `/logout` rồi `/login` lại
- kiểm tra trình duyệt đã hoàn tất đăng nhập
- kiểm tra proxy doanh nghiệp hoặc hạn chế SSO

### Gói chia sẻ bổ sung

- `pi-share-hf` — chia sẻ session lên Hugging Face thay vì GitHub Gist. Cài bằng `pi install npm:pi-share-hf`.
---

[← Phím tắt](../07-keybindings/README.md) | [Mục lục](../README.md) | [Cài đặt →](../09-settings/README.md)

### Xem thêm

- [Cài đặt](../09-settings/README.md)
- [Pi Packages](../10-pi-packages/README.md)

Bản tiếng Anh: [../../08-providers/README.md](../../08-providers/README.md)
