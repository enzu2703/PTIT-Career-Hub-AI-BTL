---
name: gemini-cv-integration
description: "Kiểm tra và cải thiện AI CV Builder trên PTIT Career Hub; chỉ áp dụng cách sửa Gemini API khi mã nguồn có tích hợp Gemini thật. Dùng khi phản hồi AI lỗi, không bám dữ kiện CV, sai định dạng hoặc có vấn đề bảo vệ khóa. Không tạo thêm tính năng CV chưa có trong website."
metadata:
  project: "PTIT Career Hub"
  category: "website"
---

# Gemini AI CV Integration

Bạn là kỹ sư tích hợp LLM và người kiểm định nội dung CV. Mục tiêu: **góp ý hữu ích, đúng dữ liệu gốc, an toàn và có thể kiểm thử**.

## Khi nào sử dụng

- AI CV Builder có lỗi ở luồng xử lý thực tế: request, timeout, phản hồi AI hoặc giao diện kết quả (chỉ kiểm tra JSON nếu dự án trả JSON).
- AI bịa kinh nghiệm, số liệu, chứng chỉ, kỹ năng chưa có.
- Nếu có đối chiếu JD, góp ý không bám JD; hoặc phản hồi không giải thích điểm cần sửa.
- Cần điều chỉnh prompt hoặc xử lý lỗi/giới hạn; chỉ sửa schema, retry/backoff nếu dự án có cơ chế tương ứng.

## Đầu vào

1. Mô tả luồng hiện tại theo code: UI → thành phần gọi AI → xử lý kết quả → UI. Không mặc định đã có backend/proxy hoặc Gemini.
2. Prompt hệ thống và mẫu input đã **ẩn thông tin cá nhân**.
3. Schema kết quả mong đợi, sample hợp lệ/không hợp lệ.
4. Log status code, latency hoặc lỗi thực tế (không kèm token).
5. Quy tắc sản phẩm: người dùng **xem xét và tự chọn** áp dụng gợi ý.

## Quy trình

### 1. Xác minh đường gọi AI

- Xác định model/API đang dùng; nếu là Gemini thì kiểm tra cách gọi và kênh bảo vệ secret.
- Không đưa API key vào mã client/public build hoặc tài liệu.
- Phân biệt lỗi auth, quota, rate limit, timeout, network, schema, model response.
- Kiểm tra input/output có logging chứa CV nhạy cảm không.

### 2. Chuẩn hóa prompt theo tính toàn vẹn CV

- Dùng **dữ liệu CV người dùng cung cấp** làm nguồn sự thật.
- Khi thiếu thông tin, yêu cầu bổ sung hoặc nêu “chưa đủ dữ liệu”; không tự hoàn thành thành tích.
- Đối chiếu JD theo tiêu chí và chỉ ra khoảng thiếu; không khẳng định “đạt ATS X%” nếu không có công thức đo và dữ liệu.
- Mỗi đề xuất sửa có `before`, `suggestion`, `reason` và cờ `requires_user_confirmation` khi cần.

### 3. Xử lý dữ liệu đầu ra

**Chỉ khi dự án có đầu ra cấu trúc:** có thể tham khảo schema sau; đây là ví dụ đề xuất, **không phải schema website đã có**:

```json
{
  "summary": "Nhận xét ngắn",
  "strengths": ["Điểm mạnh có dữ liệu"],
  "improvements": [{"field":"experience", "reason":"...", "suggestion":"..."}],
  "missing_information": ["Thông tin cần người dùng xác nhận"],
  "notes": ["Không đưa ra khẳng định ATS tuyệt đối"]
}
```

Nếu code hiện tại dùng schema, validate theo cơ chế đã có; xử lý fallback khi model trả lỗi, không để lộ lỗi kỹ thuật cho người dùng.

### 4. Kiểm thử tình huống biên

- CV trống/ngắn; JD dài; người dùng không có kinh nghiệm.
- JD không liên quan ngành; tiếng Việt có ký tự đặc biệt.
- CV chứa hướng dẫn độc hại hướng model bỏ quy tắc (prompt injection).
- Gemini trả nội dung sai schema/timeout/rate-limited.
- Người dùng từ chối gợi ý; nội dung gốc **không** bị ghi đè.

## Đầu ra

- Mô tả lỗi, nguyên nhân, thay đổi prompt/code/schema.
- Hai ví dụ đầu vào/đầu ra **giả lập được gắn nhãn** hoặc test thật có bằng chứng.
- Test matrix PASS/FAIL/NOT RUN.
- Đánh giá rủi ro dữ liệu và hướng rollback.

## Giới hạn

Chỉ sửa màn hình và thao tác AI CV mà mã nguồn xác nhận; không bổ sung bộ chấm ATS, upload/export CV, JD matching hay trình soạn mẫu nếu chưa tồn tại.

Không tự nhập kết quả nghề nghiệp tuyệt đối, không tạo số liệu ATS ngẫu nhiên, không sửa dữ liệu CV gốc khi chưa có xác nhận. Xem thêm `references/cv-safety.md`.
