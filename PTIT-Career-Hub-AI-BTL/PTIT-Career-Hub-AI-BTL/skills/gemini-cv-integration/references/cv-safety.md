# CV integrity & model response checks

## Những gì AI có thể đề xuất

- Viết câu mô tả rõ hơn **từ dữ kiện đã tồn tại**.
- Phân nhóm kỹ năng, chỉ ra thiếu mục, phát hiện trùng lặp.
- Nếu website có phần đối chiếu JD, gợi ý từ khóa dựa trên JD đã cung cấp.
- Giải thích điểm phù hợp/chưa phù hợp một cách định tính.

## Những gì AI không được tạo

- Công ty, kinh nghiệm, bằng cấp, chứng chỉ chưa có.
- Doanh thu, phần trăm tăng trưởng, thành tích, năm làm việc không được cung cấp.
- “ATS 98%” nếu không có thuật toán chấm điểm minh bạch.

## Kiểm thử

1. Dữ liệu đầu ra có chứa sự kiện mới không có trong input?
2. Website có cơ chế áp dụng thay đổi nào? Nếu có, quyền quyết định thuộc người dùng không?
3. Key/API và CV nhạy cảm có lộ trong client/log không?
4. Format lỗi có fallback dễ hiểu không?
