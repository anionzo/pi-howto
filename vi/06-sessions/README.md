# Phiên làm việc

Pi lưu session dưới dạng file JSONL có cấu trúc cây. Mỗi entry thường có `id` và `parentId`, nhờ đó pi có thể phân nhánh lịch sử ngay trong cùng một file.

## Quản lý phiên làm việc

### CLI hay dùng

```bash
pi -c
pi -r
pi --no-session
pi --session <path>
pi --fork <path>
pi --session-dir <dir>
```

### Lệnh trong ứng dụng

- `/new` — tạo session mới
- `/resume` — mở lại session cũ
- `/session` — xem thông tin session hiện tại
- `/tree` — điều hướng cây session
- `/fork` — tạo file session mới từ nhánh hiện tại
- `/compact [prompt]` — nén ngữ cảnh cũ
- `/export [file]` — xuất HTML
- `/share` — chia sẻ qua private GitHub gist

## Vị trí tệp

Session mặc định nằm dưới:

```text
~/.pi/agent/sessions/--<path>--/<timestamp>_<uuid>.jsonl
```

Bạn có thể đổi thư mục lưu bằng:
- `--session-dir <dir>`
- `sessionDir` trong `settings.json`

## Phiên bản phiên làm việc

Pi tự hiểu và tự nâng cấp session cũ:

- `v1` — chuỗi entry tuyến tính
- `v2` — mô hình cây qua `id` và `parentId`
- `v3` — đổi role `hookMessage` thành `custom`

## Các loại entry quan trọng

- `session` — header của file session
- `message` — message thường
- `model_change` — đổi model
- `thinking_level_change` — đổi mức thinking
- `compaction` — bản tóm tắt ngữ cảnh cũ
- `branch_summary` — tóm tắt nhánh khi rời `/tree`
- `custom` — dữ liệu extension không gửi cho model
- `custom_message` — message extension có thể gửi cho model
- `label` — đánh dấu entry
- `session_info` — tên hiển thị của session

## `/tree`

`/tree` cho phép bạn di chuyển trong cây lịch sử mà không cần tạo file mới.

Các khả năng chính:
- tìm kiếm bằng cách gõ trực tiếp
- gập/mở nhánh bằng `Ctrl+←` / `Ctrl+→` hoặc `Alt+←` / `Alt+→`
- đổi chế độ lọc bằng `Ctrl+O`
- gắn nhãn bằng `Shift+L`
- bật/tắt timestamp của nhãn bằng `Shift+T`

## `/fork`

`/fork` tạo **file session mới** từ nhánh hiện tại hoặc từ session/path được chỉ định.

Luồng cơ bản:
1. chọn điểm muốn tách
2. pi sao chép lịch sử liên quan sang session mới
3. message được chọn được đưa vào editor để bạn chỉnh và tiếp tục

## Nén ngữ cảnh (`/compact`)

Compaction dùng để tóm tắt phần ngữ cảnh cũ khi session dài dần.

- có thể chạy thủ công bằng `/compact`
- tự động chạy khi gần chạm giới hạn context hoặc bị overflow
- lịch sử đầy đủ vẫn còn trong file JSONL

Ví dụ:

```bash
/compact
/compact Focus on recent code changes and unresolved bugs
```

## Điều quan trọng cần nhớ

- `/tree` giúp thử nhiều hướng khác nhau mà không phá hỏng nhánh cũ
- `/fork` tạo file session mới, còn `/tree` chỉ di chuyển trong cùng file
- compaction là cơ chế tóm tắt có cấu trúc, không phải xóa mù quáng
- nếu cần xem lại lịch sử cũ, hãy quay lại qua `/tree`

## CI và môi trường team

### Session tạm trong CI

```bash
# Trong CI, tắt lưu session
pi --no-session -p "query"
```

### Thư mục session chung cho team

Lưu session trong thư mục project và chia sẻ qua settings:

```json
{
  "sessionDir": ".pi/sessions"
}
```

Thêm `.pi/sessions/` vào `.gitignore` để không commit session lên repo.

### Compaction và điều hướng `/tree`

Khi dùng `/tree` để quay về điểm trước khi compaction xảy ra, bạn nhìn thấy lịch sử JSONL đầy đủ. Nhưng nếu tiếp tục hội thoại từ điểm đó, ngữ cảnh LLM nhận sẽ được **xây lại từ bản tóm tắt compaction**, không phải từ raw messages. Bản tóm tắt là những gì model "nhớ".

## Đọc tiếp

- [07-keybindings](../07-keybindings/README.md)
- [09-settings](../09-settings/README.md)
- Bản đầy đủ tiếng Anh: [../../06-sessions/README.md](../../06-sessions/README.md)
