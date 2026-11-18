# PTIT Career Hub — Prompt Engineering & AI Skills

Bộ hồ sơ bài tập lớn về ứng dụng AI trong phát triển website PTIT Career Hub và tự động hóa nội dung Fanpage. Các tài liệu được tổ chức theo hướng dẫn `SKILL.md`: mỗi skill có metadata, điều kiện sử dụng, quy trình, đầu ra và tài liệu tham khảo riêng.

**Website dự án:** https://ptitcareerhub.online/  
**Phạm vi:** hỗ trợ sinh viên PTIT định hướng nghề nghiệp, tiếp cận thông tin công việc/sự kiện và cải thiện CV; đồng thời sử dụng n8n để đăng nội dung Fanpage.  
**Tình trạng AI Skills:** bảy bộ skill được biên soạn làm tài liệu bài tập lớn. Việc một skill có trong ZIP không chứng minh nó đã được nạp/chạy trong Agent; cần đối chiếu cấu hình và lịch sử chạy thực tế.

## 1. Cấu trúc hồ sơ

```text
PTIT-Career-Hub-AI-BTL/
├── README.md
├── skills/
│   ├── web-ui-responsive-fix/
│   │   ├── SKILL.md
│   │   └── references/qa-checklist.md
│   ├── react-typescript-debugging/
│   │   ├── SKILL.md
│   │   └── references/bug-report.md
│   ├── firebase-auth-firestore/
│   │   ├── SKILL.md
│   │   └── references/access-review.md
│   ├── gemini-cv-integration/
│   │   ├── SKILL.md
│   │   └── references/cv-safety.md
│   ├── content-research-plan/
│   │   ├── SKILL.md
│   │   └── references/content-pillars.md
│   ├── fanpage-copywriting/
│   │   ├── SKILL.md
│   │   └── references/voice-guide.md
│   └── n8n-content-quality-publishing/
│       ├── SKILL.md
│       └── references/workflow-review.md
├── prompts/
│   ├── 01-website-implementation.md
│   ├── 03-n8n-agent-instructions.md
│   └── 04-n8n-workflow-requirements.md
└── workflows/
    ├── auto-fb-posting.reference.json
    └── facebook-auto-posting-workflow.reference.json
```

## 2. Danh mục 7 skills

| Skill | Nhiệm vụ | Nhóm |
|---|---|---|
| [web-ui-responsive-fix](skills/web-ui-responsive-fix/SKILL.md) | Kiểm tra/sửa giao diện và responsive của trang hiện có | Website |
| [react-typescript-debugging](skills/react-typescript-debugging/SKILL.md) | Chẩn đoán và sửa lỗi mã nguồn frontend thực tế | Website |
| [firebase-auth-firestore](skills/firebase-auth-firestore/SKILL.md) | Kiểm tra tích hợp auth/dữ liệu **nếu dự án thực tế sử dụng Firebase** | Website |
| [gemini-cv-integration](skills/gemini-cv-integration/SKILL.md) | Kiểm tra luồng AI CV và Gemini **khi mã nguồn có tích hợp** | Website |
| [content-research-plan](skills/content-research-plan/SKILL.md) | Nghiên cứu ý tưởng và brief truyền thông | Content |
| [fanpage-copywriting](skills/fanpage-copywriting/SKILL.md) | Viết bài Facebook đúng yêu cầu Fanpage | Content |
| [n8n-content-quality-publishing](skills/n8n-content-quality-publishing/SKILL.md) | Kiểm tra quy trình đăng bài Google Sheets → Gemini → Drive → Facebook | Content / Automation |

**Không có skill tạo ảnh AI.** Workflow lấy ảnh có sẵn từ Google Drive.

## 3. Phạm vi website — chỉ ghi những gì có cơ sở

Nhóm phát triển xác nhận hiện có **5 nhóm chức năng**:

1. Career Roadmap
2. Job Hub
3. Event Hub
4. AI CV Builder
5. Admin

**Career Check đã bị loại bỏ** và không được đưa trở lại trong prompt/skills. Các chức năng nhỏ hơn như bộ lọc theo ngành, lưu việc làm, điểm tương thích, chỉnh sửa hồ sơ, trạng thái xuất bản, theo dõi tiến độ hoặc báo cáo phân tích **không được coi là đã có** chỉ vì xuất hiện trong tài liệu thiết kế cũ. Chỉ làm việc với chúng **nếu thấy thật trong giao diện hoặc mã nguồn**.

**Mức độ xác minh website (18/11/2026):** Đã truy cập URL công khai và đọc được tiêu đề “PTIT Career Hub - Nền tảng Định hướng Nghề nghiệp & Việc làm PTIT”, nhưng công cụ đọc trang không trích xuất được giao diện JavaScript bên trong. Không có quyền đăng nhập/admin hoặc repository mã nguồn để thử trực tiếp từng chức năng. Do đó, năm module trên là **thông tin nhóm xác nhận**, không phải kết quả kiểm thử độc lập toàn bộ website.

## 4. Quy trình triển khai và Agent thực tế

- Nhóm dùng **Google AI Studio** kết hợp ChatGPT/Gemini hỗ trợ viết và chỉnh mã website.
- Agent n8n `Auto FB posting` gọi workflow `Facebook Auto Posting AI Agent`.
- Workflow tham chiếu đọc `Scheduled Posts` (các cột `Ngày đăng`, `Giờ đăng`, `Nội dung`), Gemini viết bài, lấy ảnh Drive và đăng lên Fanpage bằng Facebook Graph API; ghi log vào Google Sheets.
- Nhóm xác nhận đã đăng bài thành công; không có bước duyệt thủ công hoặc tạo ảnh AI trong export đã cung cấp.
- Hai JSON trong `workflows/` là bản tham chiếu đã ẩn cấu hình nhạy cảm, **không phải bản import để chạy ngay**. JSON Agent gốc có `skills: []`; việc gắn thêm skill sau đó phải kiểm tra lại trên Agent, không tự coi là đã hoạt động.

## 5. Sử dụng hồ sơ

1. Xem danh mục skill ở mục 2 và chọn đúng công việc cần giải quyết.
2. Đọc `skills/<tên-skill>/SKILL.md`, kèm tệp trong `references/` khi được chỉ định.
3. Khi làm việc với website, **đọc giao diện và mã nguồn đang chạy trước**, không tạo thêm tính năng mới từ suy đoán.
4. Dùng `prompts/` như tài liệu prompt đã chuẩn hóa để tham khảo; **không coi đây là toàn bộ lịch sử lệnh được nhập nguyên văn**.
5. Dùng `workflows/` để đối chiếu kiến trúc n8n, không dùng bản JSON đã ẩn cấu hình để import trực tiếp.

Nếu chỉ dán nội dung `SKILL.md` vào prompt Google AI Studio, đó là dùng **hướng dẫn của skill**, không phải tự động cài đặt Agent Skills vào công cụ. Khi một Agent có hỗ trợ skills thực sự, phải tải skill lên theo giao diện của Agent và kiểm tra log khi thử nghiệm.

## 6. Nguyên tắc trung thực và an toàn

- Không trình bày tính năng dự kiến là tính năng đã có.
- Không trình bày code chưa test là đã chạy hoặc skill chưa gắn là đã sử dụng.
- Không công khai credential, token, khóa API, dữ liệu CV cá nhân hay thông tin sinh viên.
- Không bịa thông tin tuyển dụng, sự kiện, người dùng hoặc kết quả AI.
- Nội dung tài liệu được viết riêng cho PTIT Career Hub; hình thức tổ chức tham khảo kiểu thư mục Agent Skills phổ biến, không sao chép nội dung repository khác.
