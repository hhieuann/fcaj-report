---
title: "Worklog Tuần 5"
date: 2026-09-14
weight: 5
chapter: false
pre: " <b> 1.5. </b> "
---

### Mục tiêu tuần 5:

* Phát hành bản đầu tiên `v0.1.0` của dự án: tách `release/v0.1.0` (16/10), cả nhóm nghiệm thu trên staging, gắn tag và deploy prod (19/10).
* Điều phối nhóm: review PR, chia câu hỏi thiết kế cho đúng người phụ trách, họp nhóm đi qua module mẫu.

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
|-----|-----------|--------------|-----------------|----------------|
| 2 | - Rà soát việc cả nhóm sau cuối tuần: Nhân merge tài liệu nghiệp vụ gợi ý, sự kiện và script sinh dữ liệu v1 (uv, pytest, CI Python chạy được) (#107, #109); Hoàng làm lại giao diện bản chàm (#117–#125)<br>- Đọc đề xuất thiết kế bảng `events`, `recs` của Nhân; chia 3 câu hỏi mở cho đúng người phụ trách (luồng PURCHASE cho Hoàng; xuất dữ liệu và con trỏ ACTIVE cho Nhân và An); lưu ý Firehose không có trên Free plan<br>- Phân tích lockfile hỏng sau khi merge 2 PR Dependabot tạo từ cùng một gốc (#111, #115), Hoàng sửa ở #119<br>- Tìm hiểu cách Dependabot hoạt động (lịch, nhóm cập nhật, nhánh riêng cho mỗi PR, bản lớn và bản nhỏ); cấu hình không mở PR bản lớn cho npm và uv, gộp bản nhỏ của uv thành 1 PR (PR #127) | 12/10/2026 | 12/10/2026 | https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/bp-general-nosql-design.html<br>https://pnpm.io/git#merge-conflicts<br>https://docs.github.com/en/code-security/dependabot/working-with-dependabot/dependabot-options-reference |

### Kết quả đạt được tuần 5:

* Đang cập nhật.
