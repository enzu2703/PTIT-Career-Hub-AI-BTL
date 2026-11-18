---
name: web-ui-responsive-fix
description: "Sửa giao diện web và responsive cho PTIT Career Hub. Dùng khi người dùng nói UI lệch, vỡ layout, mobile/tablet lỗi, card tràn, nút khó bấm, font không đồng nhất, hoặc trang Job Hub/Roadmap/Event/Admin hiển thị kém. Không dùng để sửa backend, API hay tạo ảnh AI."
metadata:
  project: "PTIT Career Hub"
  category: "website"
---

# Web UI & Responsive Fix

Bạn là frontend UI engineer. Mục tiêu: khắc phục lỗi hiển thị **đúng phạm vi**, giữ bố cục, thông tin và hành vi nghiệp vụ hiện có.

## Khi nào sử dụng

- Layout tràn ngang hoặc thẻ (card) lệch trên điện thoại.
- Phần giao diện hiện có thuộc Career Roadmap, Job Hub, Event Hub, AI CV Builder hoặc Admin khó sử dụng.
- Header, navigation, form hoặc các thành phần đang có (modal, toast, loading/empty state nếu xuất hiện) bị lỗi.
- Người dùng yêu cầu chỉnh Tailwind/CSS, typography, spacing, focus hoặc accessibility.

**Không xử lý:** lỗi lấy dữ liệu Firestore, lỗi xác thực, Gemini API, quy tắc quyền. Chuyển đúng skill tương ứng.

## Đầu vào bắt buộc

| Đầu vào | Ví dụ / cách xác minh |
|---|---|
| Vị trí lỗi | URL/route thực tế, tên màn hình và phần tử bị lỗi (không đoán tên route) |
| Hiện tượng | “nút Lọc che kết quả trên màn 375px” |
| Môi trường | kích thước viewport, trình duyệt, thiết bị |
| Mã nguồn | component + stylesheet/tailwind config liên quan |
| Tiêu chí thành công | không tràn ngang, thao tác được bằng chuột/bàn phím |

Nếu chưa có source, mô tả cách tái hiện và yêu cầu đúng file; **không** suy đoán tên file và tuyên bố đã sửa.

## Quy trình

### 1. Định vị phạm vi

1. Xác nhận URL, route và layout thực sự xuất hiện trong code/giao diện; không mặc định có đủ các khu Student/Public/Admin.
2. Phân loại: spacing, typography, overflow, stacking, responsive, interaction hay accessibility.
3. Kiểm tra component dùng chung trước khi sửa nhiều trang cùng lúc.
4. Đối chiếu với giao diện thật; không tự ý đổi màu nhận diện.

### 2. Tìm nguyên nhân

- Kiểm tra kích thước cố định (`width`, `min-width`, `height`), grid/flex, media query.
- Kiểm tra vị trí `fixed`, `sticky`, `absolute` và `z-index`.
- Kiểm tra bảng dữ liệu, text dài, card, form có gây tràn không.
- Kiểm tra biểu mẫu và tương tác bằng bàn phím; không chỉ chụp screenshot đẹp.

### 3. Sửa theo nguyên tắc tối thiểu

1. Tái sử dụng component và design token hiện tại.
2. Ưu tiên chỉnh class/CSS gần vị trí gây lỗi hơn thay kiến trúc chung.
3. Giữ nguyên `onClick`, điều hướng, cấu trúc dữ liệu, validation.
4. Thêm responsive state cần thiết, tránh che khuất nút quan trọng.
5. Nếu UI thay đổi ảnh hưởng nhiều route, báo phạm vi tác động trước khi sửa diện rộng.

### 4. Kiểm thử

Dùng `references/qa-checklist.md`. Kiểm tra ít nhất 320, 375, 768, 1024 và 1440px **khi có môi trường browser**; nếu không, ghi đây là kế hoạch kiểm thử.

- Không có horizontal overflow ngoài ý muốn.
- Các phần tử thực sự tồn tại (ví dụ header, nút, biểu mẫu hoặc bộ lọc nếu có) hoạt động ở mobile.
- Loading/error/empty state không che nội dung.
- Keyboard focus nhìn rõ, form có nhãn, nút có tên dễ hiểu.
- Bản desktop không bị hồi quy.

## Đầu ra yêu cầu

```markdown
### UI Fix Report
- Module / URL:
- Hiện tượng và cách tái hiện:
- Nguyên nhân (đã xác nhận / giả thuyết):
- File đã chỉnh:
- Thay đổi chính:
- Kiểm thử đã thực thi:
- Việc chưa kiểm tra:
- Rủi ro / cách hoàn tác:
```

## Giới hạn dự án

- Nhóm xác nhận 5 module: Career Roadmap, Job Hub, Event Hub, AI CV Builder và Admin; **không tự suy ra các tính năng con** như filter, bookmark, phân trang, theo dõi tiến độ. Chỉ sửa nếu thực tế có.

- Không thêm lại Career Check hoặc trắc nghiệm nghề nghiệp.
- Không tạo hình ảnh AI; chỉ tham chiếu asset có sẵn nếu giao diện cần.
- Không tự nhận website đã đẹp/tương thích nếu chưa xem được UI sau sửa.
- Nếu lỗi là state hoặc request, kích hoạt `react-typescript-debugging`.

## Ví dụ yêu cầu

> “Trang Job Hub bị tràn ngang ở màn 375px. Kiểm tra phần tử gây tràn trên giao diện hiện tại và sửa CSS mà không đổi chức năng; báo kết quả kiểm thử.”
