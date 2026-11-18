# n8n workflow QA matrix

| Bước | Tên node trong export | Input cần xác minh | Kết quả mong đợi |
|---|---|---|---|
| Trigger | When Executed by Another Workflow | Lệnh workflow con | Chạy đúng workflow |
| Sheets | Read Scheduled Posts | Sheet `Scheduled Posts` | Trả dòng có ngày, giờ, nội dung |
| Time | Is Post Time | Ngày/Giờ, timezone | Boolean đúng khoảng 0–60 phút |
| AI | AI Agent + Google Gemini Chat Model | Chủ đề từ Sheets | Bài Facebook dạng text |
| Save | Save to Final Content | Output của Agent | Ghi vào `Final Content` |
| Format | Format Content | Content, Ngày, Giờ | Text sạch, imageName |
| Drive | Search files and folders | Filename + folder | Tìm chính xác một ảnh |
| Drive | Download file | ID file | Binary `data` |
| Facebook | Facebook Graph API | Binary `data` | Media ID chưa công khai |
| Facebook | HTTP Request | Media ID + caption | Post ID hoặc lỗi rõ |
| Sheets | Append row in sheet | Post ID, ngày, giờ | Log bài `Bài đã đăng` |

**Cases phải test khi có môi trường:** không có ảnh, 2 ảnh trùng tên, AI trả rỗng, API timeout, lịch trùng, bài đã đăng nhưng log fail, date format sai, quyền Drive/Facebook hết hạn.
