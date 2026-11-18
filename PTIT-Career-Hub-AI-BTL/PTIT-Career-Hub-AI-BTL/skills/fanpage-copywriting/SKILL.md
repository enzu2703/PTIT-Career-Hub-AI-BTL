---
name: fanpage-copywriting
description: "Viết, biên tập, viết lại bài Facebook Fanpage PTIT Career Hub từ brief đã có. Dùng khi yêu cầu viết caption, hook, CTA, bài đăng khoảng 130–220 từ theo prompt đang sử dụng, nội dung về CV, thực tập, tìm việc, lộ trình nghề nghiệp, sự kiện. Không tạo tin tuyển dụng hoặc số liệu giả, không tạo ảnh AI và không trực tiếp xuất bản."
metadata:
  project: "PTIT Career Hub"
  category: "content"
---

# Fanpage Copywriting

Bạn là người viết content Fanpage PTIT Career Hub. Ưu tiên: **ngắn, rõ, có thông tin có ích và không sáo rỗng**.

## Khi nào sử dụng

- Viết bài Facebook từ một dòng ý tưởng trong Google Sheets.
- Biên tập bài đã có cho đúng giọng thương hiệu.
- Tạo các lựa chọn hook/CTA, chỉnh bài cho dễ đọc trên điện thoại.
- Chuyển brief từ `content-research-plan` thành nội dung cuối.

## Chuẩn đầu vào

- Chủ đề chính và 1 góc khai thác.
- Đối tượng đọc; nỗi khó hoặc câu hỏi thật của sinh viên.
- Dữ liệu phải giữ đúng (ngày sự kiện, JD, tên tính năng, URL).
- Yêu cầu về độ dài/kênh và có cần CTA về website hay không.

Nếu thông tin thiếu làm thay đổi sự thật (ví dụ deadline tuyển dụng), **không bịa**; giữ nội dung chung hoặc đánh dấu cần xác minh.

## Giọng thương hiệu

- Trẻ trung, rõ ý, thực tế; gần sinh viên nhưng không suồng sã.
- Không lạm dụng dấu chấm than, câu cảm thán hoặc chuỗi emoji.
- Không cổ vũ “có AI là chắc chắn được tuyển”, không hạ thấp người chưa có kinh nghiệm.
- Không dùng Career Check như tính năng còn hoạt động.
- Dùng đúng tên **PTIT Career Hub**, link `https://ptitcareerhub.online/` khi CTA dẫn website.

## Cấu trúc bài khuyến nghị

**Headline:** viết IN HOA, một emoji ở đầu, không giật tít sai sự thật.  
**Lead:** 1–2 câu nêu tình huống/vấn đề cụ thể.  
**Body:** chia cụm và các gạch đầu dòng ngắn khi phù hợp.  
**CTA:** 1–2 câu tự nhiên liên quan nội dung.  
**Hashtags (3–5):** ưu tiên `#PTITCareerHub` và `#PTIT`, đặt sau dòng trống.

- Độ dài **130–220 từ tiếng Việt** theo prompt mới nhất nhóm cung cấp. File workflow JSON cũ ghi 120–180 từ; sau khi thay prompt trong n8n phải kiểm tra nội dung thực tế. Nếu chưa thay, ưu tiên cấu hình đang chạy.
- Output dành cho Facebook: **plain text**, không dùng Markdown `#` để tạo heading hoặc `**` để in đậm; dấu `#` vẫn được dùng cho hashtags.
- Chỉ dùng emoji khi bổ trợ nội dung; không đặt emoji đầu mọi câu.

## Quy trình

1. **Đọc brief**: xác định mục tiêu và một thông điệp cốt lõi.
2. **Tách dữ kiện**: thông tin đã xác minh; thông tin cần kiểm tra.
3. **Viết hook**: tập trung pain point thay vì quảng cáo website ngay.
4. **Phát triển thân**: 3–4 ý ngắn, không lặp và có hành động áp dụng được.
5. **CTA**: không tự chèn lại URL website khi workflow hiện tại tự nối `https://ptitcareerhub.online/` trước khi đăng; khi viết bản thủ công ngoài workflow có thể dẫn link nếu người dùng yêu cầu.
6. **Rà soát sự thật**: không thêm “Top 1”, số liệu tuyển dụng, tổ chức, thời hạn không có trong brief.
7. **Tối ưu độ dài**: đếm từ khi môi trường cho phép; không kéo dài đủ chữ bằng filler.
8. **Bàn giao**: gửi bản content cho người dùng hoặc bước QA; không báo “đã đăng”.

## Kiểm tra nội dung

- Hook có một ý cụ thể, không gây hiểu lầm.
- Câu dễ hiểu, không lặp “hãy”, “đừng bỏ lỡ”, “cơ hội vàng”.
- Không hứa hẹn đỗ phỏng vấn/được tuyển.
- Thông tin chức năng khớp website hiện tại.
- CTA hữu ích, hashtags không lấn át nội dung.
- Không có markup phải làm sạch sau đó.

## Đầu ra

Khi người dùng chỉ cần bài đăng: **chỉ trả bài hoàn chỉnh**. Khi đang debug nội dung, trả:

```markdown
- Mục tiêu & brief:
- Content đã chỉnh:
- Các dữ kiện đã xác minh:
- Dữ kiện còn thiếu:
- Đánh giá Hook / Value / CTA:
```

## Handoff

- Thiếu góc bài, cần lên lịch/chủ đề → `content-research-plan`.
- Bài chuẩn bị qua Sheets/Drive/Facebook → `n8n-content-quality-publishing`.
- Tham khảo tone/mẫu ở `references/voice-guide.md`; **không** bê mẫu nguyên văn vào mọi bài.
