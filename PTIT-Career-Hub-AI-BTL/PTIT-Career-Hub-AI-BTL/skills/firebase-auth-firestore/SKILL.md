---
name: firebase-auth-firestore
description: "Chỉ dùng khi mã nguồn PTIT Career Hub thực sự có Firebase Authentication/Firestore: kiểm tra lỗi xác thực, truy vấn dữ liệu hoặc quyền Admin liên quan 5 module hiện hành. Không mặc định dự án có hồ sơ sinh viên, chức năng lưu Job, hay các collection trong tài liệu thiết kế cũ."
metadata:
  project: "PTIT Career Hub"
  category: "website"
---

# Firebase Auth & Firestore Reliability

Bạn là kỹ sư backend tích hợp Firebase. **Trước tiên xác minh dự án thực sự sử dụng Firebase và đường đi dữ liệu có liên quan**, sau đó bảo vệ quyền truy cập, dữ liệu cá nhân và tính nhất quán khi sửa.

## Khi nào sử dụng

**Chỉ chọn skill này khi phát hiện cấu hình Firebase/Firestore trong code thực tế.** Firebase có trong bản thiết kế ban đầu nhưng chưa được xác minh độc lập trên website công khai.

- Đăng nhập/đăng xuất hoặc route được bảo vệ hoạt động sai, **nếu project có các luồng đó**.
- Dữ liệu của chức năng hiện có trong Career Roadmap/Job Hub/Event Hub không được lưu hoặc không hiển thị đúng, nếu đang sử dụng Firestore.
- Thao tác quản trị đối với Job/Event bị lỗi, **chỉ khi giao diện quản trị thực tế có thao tác đó**.
- Firestore truy vấn sai dữ liệu, thiếu index hoặc lỗi quyền.

## Thông tin cần đọc

- Schema/collection thực tế, sample **đã ẩn thông tin riêng tư**.
- Auth provider, cách cấp role, Security Rules và route guard.
- Log lỗi Firestore, query, thao tác tái hiện, user role thử nghiệm.
- Các transaction/batch write liên quan.

**Không yêu cầu token, private key, mật khẩu hoặc CV cá nhân nguyên bản.** Nếu chưa có đủ dữ liệu, tạo checklist chẩn đoán thay vì mở quyền rộng.

## Quy trình

### 1. Vẽ đường dữ liệu

`UI event → auth state → service → Firestore query/write → snapshot/listener → UI state`.

Xác minh từng bước, phân biệt lỗi không ghi thành công với lỗi UI không cập nhật.

### 2. Kiểm tra xác thực và phân quyền

- User chỉ đọc/sửa dữ liệu của chính mình theo quy tắc dự án.
- Admin role phải dựa trên cơ chế tin cậy phía server/rules, không dựa vào nút ẩn/hiện UI.
- Không để client tự gán `admin: true` rồi được quyền ghi nội dung quản trị.
- Tránh Security Rules kiểu `allow read, write: if true;` trong sản phẩm thật.

### 3. Kiểm tra dữ liệu và đồng bộ

- Review ID, field types, timestamp và status **nếu tồn tại trong schema thực tế**, không tự tạo trường `draft`/`published`.
- Đọc/ghi đúng documents; tránh ghi đè cả object khi chỉ đổi một trường.
- Nếu có cơ chế xuất bản, query chỉ trả về dữ liệu được phép công khai; không mặc định hệ thống có trạng thái Published.
- Chỉ kiểm tra tiến độ Roadmap hoặc lưu việc làm khi mã nguồn/giao diện xác nhận những thao tác đó có tồn tại.
- Xem lại xử lý loading, offline, retry, lỗi permission-denied, missing-index.

### 4. Sửa và xác minh

1. Đề xuất migration khi đổi schema; đánh giá tác động dữ liệu cũ.
2. Test các role thực tế được cấu hình trong dự án (ví dụ student/admin nếu có) trong môi trường an toàn.
3. Test quyền truy cập theo Rules thực tế; không mặc định cấu trúc role/collection của bản thiết kế ban đầu còn được sử dụng.
4. Test reload và nhiều phiên làm việc khi khả thi.
5. Không ghi “đã test” nếu chưa thực thi.

## Đầu ra

```markdown
### Firebase Review
- Module, collection, thao tác:
- Nguyên nhân & bằng chứng:
- Security implications:
- Firestore Rules/queries cần sửa:
- Migrations (nếu có):
- Test theo vai trò (PASS/FAIL/NOT RUN):
- Cảnh báo bảo mật còn lại:
```

Tham chiếu `references/access-review.md` trước khi chốt.

## Không được làm

- Mở full database cho mọi tài khoản để chữa lỗi quyền.
- Log raw access tokens, hồ sơ/CV nhạy cảm hay credential.
- Cố tình vượt route guard hoặc bỏ quy tắc Admin.
- Khẳng định đã triển khai an toàn khi không có rules và test.

## Ví dụ

> “Một thao tác lưu dữ liệu trong trang Admin gặp lỗi permission-denied. Nếu dự án sử dụng Firestore, đối chiếu Rules và query thực tế, không mở quyền rộng chỉ để hết lỗi.”
