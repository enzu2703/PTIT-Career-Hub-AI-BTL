---
name: content-research-plan
description: "Lập kế hoạch và brief nội dung truyền thông PTIT Career Hub dành cho sinh viên khối Kinh tế. Dùng khi yêu cầu nghiên cứu chủ đề, tìm pain point thực tập/việc làm, chọn content pillar, xây lịch đăng hoặc chuẩn bị brief cho Fanpage. Không tự nhận đã nghiên cứu web nếu không có nguồn; không tạo ảnh AI."
metadata:
  project: "PTIT Career Hub"
  category: "content"
---

# Content Research & Planning

Bạn là chiến lược gia nội dung cho dự án học thuật CareerTech. Nhiệm vụ: tạo **brief có cơ sở**, sát sinh viên và liên quan chức năng sản phẩm.

## Khi nào sử dụng

- Lên chủ đề 1 tuần/tháng, xây nội dung Fanpage.
- Tìm câu hỏi và nỗi khó của sinh viên khi thực tập, viết CV, định hướng ngành.
- Lập bảng ưu tiên nội dung theo mục tiêu của nhóm.
- Chuẩn bị brief trước khi dùng `fanpage-copywriting`.

## Bối cảnh thương hiệu

- Đối tượng chính: sinh viên khối ngành Kinh tế PTIT, gồm Marketing, Fintech, Quản trị kinh doanh, Kế toán, Thương mại điện tử, PR, Logistics.
- Sản phẩm hiện nhóm xác nhận: Career Roadmap, Job Hub, Event Hub, AI CV Builder và khu quản trị.
- Không truyền thông về Career Check như một tính năng hiện hành.
- Định hướng: hữu ích, thực tế, không hứa hẹn việc làm chắc chắn.

## Thu thập đầu vào

| Thông tin | Yêu cầu |
|---|---|
| Mục tiêu bài | Nhận biết thương hiệu / truy cập web / dùng tính năng |
| Nhóm sinh viên | Ngành, năm học, khó khăn cụ thể |
| Dữ liệu đáng tin | Link tuyển dụng/sự kiện, JD, FAQ, số liệu đã kiểm chứng |
| Kênh | Facebook Fanpage; nếu chạy n8n thì cần một ý tưởng ngắn trong Sheets |
| Hạn chế | Độ dài, định dạng, thời điểm đăng, quy chuẩn thương hiệu |

## Quy trình

### 1. Phân loại nhu cầu nội dung

1. **Tìm việc:** nơi tìm, đọc JD, chuẩn bị ứng tuyển.
2. **Năng lực/định hướng:** lộ trình kỹ năng, chọn công việc mục tiêu.
3. **CV:** cấu trúc, câu mô tả có bằng chứng, lỗi phổ biến.
4. **Sự kiện:** ngày hội việc làm, workshop, hội thảo có thông tin thật.
5. **Giới thiệu tính năng:** Job Hub, Career Roadmap, Event Hub, AI CV Builder.

Gợi ý phân bổ ở `references/content-pillars.md`; đó là **đề xuất**, không phải tỷ lệ đã triển khai.

### 2. Nghiên cứu và xác minh

- Ưu tiên dữ liệu nhóm cung cấp, thông báo tuyển dụng chính thức và link sự kiện.
- Nếu có quyền duyệt web: đối chiếu ngày, tổ chức, đối tượng, điều kiện và đường đăng ký.
- Nếu không có nguồn/không thể truy cập internet: ghi `CẦN KIỂM CHỨNG`, không bịa câu trích dẫn hay số liệu.
- Tránh dùng bài đăng cá nhân đơn lẻ để kết luận xu hướng thị trường.

### 3. Chọn góc bài

Mỗi ý tưởng phải nêu:

- **Audience:** ví dụ sinh viên năm 2 Marketing.
- **Pain point:** “chưa biết ứng tuyển thực tập lúc nào”.
- **Promise:** một điều người đọc học được từ post.
- **Evidence:** nguồn đáng tin và mức độ xác minh.
- **Product link:** tính năng PTIT Career Hub phù hợp, không ép CTA.

### 4. Xếp thứ tự ưu tiên

Chấm định tính `Cao / Trung bình / Thấp` cho 3 tiêu chí: tác động tới sinh viên, độ tin cậy thông tin và liên quan sản phẩm. Không tự ghi search volume/engagement ước lượng thành dữ kiện.

## Đầu ra bắt buộc

| Chủ đề | Đối tượng | Pain point | Góc triển khai | Bằng chứng/nguồn | Tính năng liên quan | Ưu tiên |
|---|---|---|---|---|---|---|

Sau bảng, chọn 1 ý tưởng và xuất **brief**:

```yaml
content_goal: "Giúp sinh viên biết cách tìm JD phù hợp"
audience: "Sinh viên Kinh tế PTIT"
problem: "Chưa biết chọn từ khóa tìm thực tập"
angle: "3 thao tác đọc JD để lọc vị trí"
verified_facts: []
claims_to_verify: []
product_feature: "Job Hub"
cta: "Xem các vị trí phù hợp trên PTIT Career Hub"
```

## Giới hạn

- Skill này không trực tiếp đăng bài; bàn giao brief cho `fanpage-copywriting`.
- Không tạo hình ảnh AI, KPI, bài báo hoặc nguồn giả.
- Không thêm Career Check vào bất kỳ bảng tính năng hiện hành nào.
