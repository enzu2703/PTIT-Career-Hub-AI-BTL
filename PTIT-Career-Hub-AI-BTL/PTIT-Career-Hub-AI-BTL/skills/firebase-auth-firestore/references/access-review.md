# Firebase & Access Review — danh sách kiểm tra có điều kiện

Tài liệu này **chỉ áp dụng khi mã nguồn thực tế sử dụng Firebase/Firestore**. Không coi các thao tác sau là chức năng đã xác nhận có trên website.

| Trường hợp cần kiểm tra (nếu có) | Câu hỏi kiểm chứng |
|---|---|
| Đăng nhập và phân quyền | Source đang xác thực bằng công cụ gì? Vai trò nào được cấp? |
| Dữ liệu riêng của người dùng | Có document nào cần kiểm tra owner UID và Security Rules? |
| Career Roadmap | Có lưu trạng thái người dùng không? Lưu tại collection nào? |
| Job Hub / Event Hub | Dữ liệu được đọc và cập nhật qua API/collection nào? Có quy tắc xuất bản không? |
| Admin | Các thao tác Admin thực sự được code hỗ trợ là gì? Quyền được kiểm chứng tại đâu? |

**Không làm:** suy đoán có bookmark, hồ sơ cá nhân, trạng thái Published hay hệ thống vai trò nhiều cấp chỉ từ prompt ban đầu. Không mở rộng `allow read, write` để chữa lỗi.
