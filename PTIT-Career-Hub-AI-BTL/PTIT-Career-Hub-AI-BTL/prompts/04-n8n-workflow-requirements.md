# Prompt Engineering — Workflow n8n đăng bài Facebook

> Tài liệu này **khôi phục yêu cầu theo cấu hình xuất ra**, không phải prompt lịch sử 100% giống câu đã nhập. Mục tiêu là giải thích quy trình hiện tại và tạo prompt đủ rõ để dựng lại mà **không thêm tính năng ngoài JSON**.

## 1. Vai trò và mục tiêu

Bạn là n8n Automation Engineer. Hãy xây dựng hoặc giải thích workflow `Facebook Auto Posting AI Agent` giúp lấy nội dung theo lịch từ Google Sheets, dùng Gemini viết post, lấy ảnh sẵn có trên Google Drive và đăng lên Facebook Fanpage.

**Không tạo ảnh AI. Không tự bổ sung Form nhập thủ công, bước duyệt, hoặc lịch cộng 30 phút** nếu cấu hình cung cấp không có. Chức năng chống đăng trùng là cải tiến cần quyết định riêng, không mô tả thành hiện trạng.

## 2. Input và trigger

- Trigger thực tế: `When Executed by Another Workflow` với một trường input `request`.
- Lịch bài: Google Sheets tab `Scheduled Posts`; cột `Ngày đăng`, `Giờ đăng`, `Nội dung`.
- Node `Is Post Time` parse `d/M/yyyy H:mm`, timezone `Asia/Ho_Chi_Minh`, điều kiện thời điểm hiện tại đã đến lịch và chưa quá 60 phút.
- Input `request` có khai báo nhưng **chưa thấy được sử dụng để viết bài** trong JSON; đoạn prompt AI Agent đọc trường `Nội dung` từ Google Sheets.

## 3. Gemini viết bài

Dùng `AI Agent` nối với `Google Gemini Chat Model`. Prompt node hiện tại định hướng:

- Độc giả: sinh viên PTIT.
- Hook một câu, thân 3–4 ý ngắn, CTA một câu.
- Độ dài khoảng 120–180 từ.
- 3–5 hashtag cuối bài.
- Giọng thực tế, dễ đọc, không thêm mở đầu kiểu “Dưới đây là...” và không Markdown.
- Không bịa JD, sự kiện, số liệu tuyển dụng hoặc chức năng sản phẩm.

**Node model trong workflow export:** `models/gemini-3.1-flash-lite` (khác model Agent bên ngoài). Đừng gộp hai cấu hình này thành một model duy nhất.

## 4. Lưu và làm sạch nội dung

- `Save to Final Content`: append dòng lên tab `Final Content` chứa ngày, nội dung gốc và bài đã viết.
- `Format Content`: chuẩn hóa văn bản, bỏ dấu Markdown gây lỗi trên Facebook, giữ hashtags, xuống dòng; tạo tên ảnh `imageName` theo ngày/giờ.
- Kiểm tra trường dữ liệu khi triển khai thật vì output của các node Google Sheets có thể thay đổi theo map cột.

## 5. Lấy ảnh từ Drive

- `Search files and folders`: tìm file trong folder Drive định sẵn theo `imageName`.
- `Download file`: tải file dạng binary `data`.
- Trường hợp không tìm được ảnh hoặc trùng tên phải được báo lỗi rõ; không chuyển sang công cụ AI image generation.

## 6. Đăng Fanpage qua Graph API

1. `/photos` upload ảnh với `published=false`; nhận media ID nếu thành công.
2. `/feed` tạo bài với `message` và `attached_media` dùng media ID trên.
3. Caption được nối thêm lời mời xem website PTIT Career Hub.
4. `Append row in sheet`: ghi nội dung, ngày/giờ, link bài xuống tab `Bài đã đăng`.

Credential và Page ID phải là biến riêng trong n8n; không hard-code trong tài liệu công khai. Chỉ ghi thành công khi API có kết quả hoặc post thực tế đã xác minh.

## 7. Sơ đồ node quan sát được

```text
When Executed by Another Workflow
  → Read Scheduled Posts
  → Is Post Time
  → AI Agent ↔ Google Gemini Chat Model
  → Save to Final Content
  → Format Content
  → Search files and folders (Google Drive)
  → Download file
  → Facebook Graph API (POST /photos, published=false)
  → HTTP Request (POST /feed)
  → Append row in sheet (Bài đã đăng)
```

Tổng: **12 nodes** (11 node tuyến chính + 1 Gemini model node nối bằng nhánh AI), đúng file JSON nhóm cung cấp.

## 8. Chấp nhận và kiểm tra

| Test case | Yêu cầu kiểm tra | Trạng thái từ đầu vào |
|---|---|---|
| Đúng giờ | Sheet đọc đúng dòng hợp lệ | Chưa có execution log |
| Content | Gemini viết theo chủ đề | JSON có prompt, chưa có output thật |
| Ảnh Drive | Khớp `imageName` và tải binary | JSON có hai node Drive |
| Facebook | POST `/photos` rồi `/feed` | Nhóm xác nhận đăng được |
| Không trùng | Một lịch chỉ đăng một lần | Chưa thấy cơ chế idempotency rõ |
| Không duyệt | Bài không qua bước approve | Đúng workflow JSON |

## 9. Cải tiến đề xuất, KHÔNG tính là chức năng đã có

- Check `published_at`, `post_id`/idempotency trước khi đăng, để hạn chế đăng trùng.
- Bổ sung nhánh lỗi khi thiếu ảnh hoặc API timeout.
- Rà soát `hasOutputParser: true` ở node Agent vì JSON không hiển thị parser connection.
- Nếu nhóm muốn duyệt content sau này mới thêm approval step, không sửa lại lịch sử triển khai.
