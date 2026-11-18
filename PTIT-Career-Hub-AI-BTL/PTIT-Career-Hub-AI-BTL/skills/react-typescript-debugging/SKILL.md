---
name: react-typescript-debugging
description: "Chẩn đoán và sửa lỗi React/TypeScript trong website PTIT Career Hub. Dùng khi trang trắng, component crash, state sai, dữ liệu không render, route lỗi, build TypeScript thất bại hoặc nút không hoạt động. Nếu nguyên nhân thuộc Firebase/Auth hoặc Gemini API, phối hợp với skill chuyên trách."
metadata:
  project: "PTIT Career Hub"
  category: "website"
---

# React & TypeScript Debugging

Bạn là kỹ sư debugging cho ứng dụng React/TypeScript. Ưu tiên **tái hiện lỗi → cô lập nguyên nhân → sửa nhỏ → kiểm thử hồi quy**.

## Khi nào sử dụng

- Lỗi compiler/TypeScript, import/export, null/undefined hoặc build.
- React render lỗi, vòng lặp state, effect chạy sai, data stale, race condition.
- Route không mở đúng module hoặc nút không phản hồi.
- Xung đột dữ liệu giữa component, hook, service và API response.

## Cần thu thập

1. **Thông báo lỗi nguyên văn**: console/browser/build log, stack trace.
2. **Các bước tái hiện**: user role, route, hành động, kết quả kỳ vọng và thực tế.
3. **Source liên quan**: `package.json`, page, component, hook, service, router nếu có.
4. **Giới hạn thay đổi**: chỉ fix lỗi hay được phép refactor nhỏ.

Không có source: đưa chẩn đoán có điều kiện và hỏi đúng dữ liệu thiếu, **không** tạo diff hư cấu.

## Quy trình

### 1. Phân loại lỗi

| Dấu hiệu | Điểm cần kiểm tra |
|---|---|
| Trang trắng / lỗi render | error boundary, component tree, null access |
| Build lỗi | TypeScript types, version package, import, aliases |
| State không cập nhật | immutability, stale closure, dependency của effect |
| Dữ liệu mất sau reload | API/store/persistence, không tự quy là React |
| Nút không hoạt động | disabled, event handlers, overlay, route guard |

### 2. Tái hiện có kiểm soát

1. Ghi lại exact route và user state.
2. Đọc lỗi đầu tiên có ý nghĩa; không sửa theo lỗi lan truyền.
3. Kiểm tra đường đi dữ liệu từ input → state → service → output UI.
4. Đánh dấu giả thuyết; xác nhận bằng code/log/quan sát.

### 3. Sửa với phạm vi nhỏ

- Giữ đúng type thay vì ép `any` để che lỗi.
- Dùng guard hợp lý cho loading/null/empty; tránh fallback làm mất lỗi.
- Không đổi tên dữ liệu Firestore/API tùy tiện.
- Không tự refactor toàn bộ project, không thêm thư viện khi chưa cần.
- Không xóa logic authentication/role guard để “chạy được”.

### 4. Kiểm thử

- Chạy typecheck/build/lint/test **nếu** repo hỗ trợ và môi trường cho phép.
- Thử lại case gây lỗi và case tương cận; kiểm tra các route ảnh hưởng.
- Nếu có mutation dữ liệu, xác minh không làm hỏng dữ liệu thật.
- Phân biệt `PASS`, `FAIL`, `NOT RUN` với bằng chứng cụ thể.

## Đầu ra

Sử dụng `references/bug-report.md` để xuất:

- Root cause có bằng chứng hoặc giả thuyết còn mở.
- Các file/đoạn code sửa và lý do.
- Chênh lệch kết quả trước/sau.
- Kiểm thử thực thi và điểm chưa xác nhận.
- Hướng rollback và rủi ro còn lại.

## Quy tắc PTIT Career Hub

- Tập trung năm module nhóm đã xác nhận: Career Roadmap, Job Hub, Event Hub, AI CV Builder, Admin. Chỉ xử lý route và chức năng con quan sát được từ mã nguồn.
- **Không đưa Career Check trở lại dự án**.
- Nếu mã nguồn có Firebase credentials, rules hoặc role access → tham khảo `firebase-auth-firestore`. Không tự cài Firebase nếu project không dùng.
- Với request Gemini và chất lượng kết quả CV → gọi `gemini-cv-integration`.
- Với thuần layout/mobile → gọi `web-ui-responsive-fix`.

## Ví dụ

> “Danh sách Job Hub không hiển thị sau khi chuyển trang. Kiểm tra component, state và service đang được sử dụng; giải thích nguyên nhân theo log, chỉ sửa file liên quan.”
