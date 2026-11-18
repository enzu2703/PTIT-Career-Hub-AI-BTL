---
name: n8n-content-quality-publishing
description: "Rà soát nội dung và vận hành workflow n8n đăng Facebook cho PTIT Career Hub. Dùng khi người dùng cần kiểm tra lịch Google Sheets, kết quả Gemini, tên ảnh Google Drive, lỗi tải ảnh, Graph API, log bài đăng, lịch đăng trùng hoặc chất lượng đầu ra. Không tạo ảnh AI và không giả định có bước duyệt thủ công trong workflow hiện tại."
metadata:
  project: "PTIT Career Hub"
  category: "automation"
---

# n8n Content QA & Publishing

Bạn là người kiểm tra workflow đăng nội dung Fanpage. Mục tiêu: **luồng đúng dữ liệu, đăng đúng bài, có log và phát hiện rủi ro**.

## Phạm vi workflow hiện có

Dựa trên JSON nhóm cung cấp:

`When Executed by Another Workflow → Read Scheduled Posts → Is Post Time → AI Agent (Gemini) → Save to Final Content → Format Content → Search files and folders (Drive) → Download file → Facebook Graph API (upload photo unpublished) → HTTP Request (post feed) → Append row in sheet`.

- Ảnh là file **có sẵn trong Google Drive**, không tạo bằng AI.
- Không có node duyệt bài thủ công trong export.
- Agent ngoài gọi workflow nội bộ; trong export có `skills: []`.
- Trạng thái workflow có `active: true` **không tự chứng minh** một bài cụ thể đã đăng thành công; thông tin đăng thành công do nhóm xác nhận.

## Khi nào sử dụng

- Lỗi hẹn giờ, đọc sai cột `Ngày đăng`, `Giờ đăng`, `Nội dung`.
- Nội dung Gemini sinh bị trống hoặc Markdown lỗi hiển thị.
- Không tìm thấy ảnh theo `imageName` hoặc tải Drive thất bại.
- Graph API trả lỗi ảnh/feed, bài đăng sai Page, thiếu link.
- Có nguy cơ đăng trùng hoặc nhiều lần chạy song song.

## Đầu vào yêu cầu

1. Export JSON workflow/Agent (đã xóa credentials và token).
2. Execution log/node error cụ thể, thời điểm theo `Asia/Ho_Chi_Minh`.
3. Dòng Sheets thử nghiệm đã ẩn thông tin riêng tư.
4. Tên file Drive dự kiến, trạng thái và loại MIME.
5. Kết quả API hoặc link bài đăng **nếu có thật**.

## Quy trình

### 1. Kiểm tra đầu vào & lịch

- `Read Scheduled Posts` có đúng sheet và đúng cột không.
- `Is Post Time` parse ngày dạng `d/M/yyyy H:mm`, timezone `Asia/Ho_Chi_Minh`.
- Trong JSON, khoảng hợp lệ là đã tới giờ và **không trễ quá 60 phút**.
- Đánh dấu **rủi ro** nếu cùng dòng vẫn được xử lý trong nhiều lần chạy; JSON không cho thấy cơ chế đánh dấu trạng thái đã xử lý trước bước đăng.

### 2. Kiểm tra Gemini & định dạng

- Đầu vào AI dùng trường `Nội dung` của sheet.
- Mẫu content yêu cầu hook 1 câu, 3–4 ý ngắn, CTA, khoảng 120–180 từ, 3–5 hashtags.
- `Save to Final Content` lưu bài do Agent trả về; `Format Content` làm sạch Markdown và tạo `imageName`.
- **Lưu ý xác minh:** Agent đang bật `hasOutputParser: true` nhưng export không cho thấy kết nối node parser; kiểm tra thực thi thực tế trước khi đề xuất thay đổi.

### 3. Kiểm tra file ảnh Drive

- Tên file phải khớp cách tạo `imageName` từ ngày/giờ trong node Format Content.
- Search Drive theo đúng folder và tên, xử lý `0 files`, `>1 files`, file không phải ảnh.
- Download phải có dữ liệu binary `data` để node upload nhận.
- Không gọi công cụ tạo ảnh AI hoặc fallback sang AI image generator.

### 4. Kiểm tra publish

- Node upload ảnh dùng endpoint `/photos` với `published=false`.
- Node `/feed` đính kèm `media_fbid` và caption + link website.
- Chỉ ghi “đã đăng” khi nhận success API hoặc có link bài hiển thị thực tế.
- Không tạo post lại khi chưa chắc lời gọi cũ đã thất bại.

### 5. Đối chiếu nhật ký và chống trùng

- Sheet `Final Content` chứa nội dung AI tạo; sheet `Bài đã đăng` ghi nội dung, ngày, giờ, link.
- Với lịch chạy lặp lại, đề xuất `status = published`, `post_id`, `executed_at`, idempotency key trước khi đăng mới. **Đây là đề xuất nâng cấp**, không phải chức năng đã xác nhận có.
- Ghi rõ workflow không có bước duyệt người quản trị; nếu thêm thì cần yêu cầu riêng.

## Kết quả mong đợi

```markdown
### n8n Workflow Review
- Execution ID / thời gian:
- Node gây lỗi (nếu có):
- Input đã xác minh:
- Content đầu ra / link Drive / Facebook:
- API response (đã ẩn định danh/token):
- Đã đăng? VERIFIED / USER-REPORTED / NOT VERIFIED
- Nguy cơ trùng / ảnh hưởng:
- Sửa đổi đề xuất và test lại:
```

Dùng `references/workflow-review.md` để kiểm tra từng node.

## Không được làm

- Không tạo ảnh AI, không thay nhánh Drive bằng image generation.
- Không đăng thử vào Fanpage thật nếu chưa có quyền và xác nhận rõ.
- Không ghi Page token, URL tài khoản cá nhân hoặc email vào log công khai.
- Không ghi thêm bước kiểm duyệt, form nhập tay, schedule 30 phút nếu không có trong JSON hiện tại.
- Không đánh đồng skill này với node/skill **đã được cài** trong Agent.
