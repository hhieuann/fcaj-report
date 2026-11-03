---
title: "Worklog Tuần 8"
date: 2026-09-14
weight: 8
chapter: false
pre: " <b> 1.8. </b> "
---

### Mục tiêu tuần 8:

* Phát hành `v0.2.0` (trễ từ tuần 7): tách nhánh release 03/11, nghiệm thu trên staging 03–04/11, gắn tag lên prod 05/11.
* Sửa các lỗi tìm thấy khi nghiệm thu, hoặc ghi rõ việc dời sang `v1.0.0-rc.1`.
* Chuẩn bị `v1.0.0-rc.1`: WAF (Nhân), C1 và header bảo mật (An), trang admin, web gửi sự kiện (Hoàng).

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
|-----|-----------|--------------|-----------------|----------------|
| 3 | - Rà tiến độ sau 4 ngày không có thay đổi: v0.2.0 chưa tách nhánh, chưa gắn tag; đề xuất lịch mới cho nhóm (tag 05/11, `v1.0.0-rc.1` dời sang 12/11)<br>- Merge checklist #308, tách `release/v0.2.0`; staging deploy xanh (smoke test, E2E). Kiểm staging: worker gửi thư đủ biến môi trường, lịch job gợi ý bật, hàng đợi và DLQ trống, 3 alarm OK<br>- Nghiệm thu đặt hàng O1–O8 trên staging: O1, O2, O3, O4, O6, O8 đạt; O5 đạt một phần (web chưa hiện câu hướng dẫn và link khi quá 3 đơn, #315); O7 đã đạt trên dev; phát hiện thêm `/orders` chưa chuyển sang trang đăng nhập khi chưa đăng nhập (#316). Đối chiếu log worker; huỷ hết đơn test để trả tồn kho. Ghi kết quả vào checklist (PR #318 vào nhánh release)<br>- Review báo cáo bảo mật bản nháp của Nhân (#310); merge #310 và 4 PR Dependabot vào `develop`, deploy dev xanh<br>- Chạy tay job gợi ý trên staging để có dữ liệu cho nghiệm thu gợi ý; lấy log Lambda ghi `PURCHASE` cho Nhân kiểm S6 (log không có dữ liệu cá nhân) | 03/11/2026 | 03/11/2026 | https://docs.aws.amazon.com/lambda/latest/dg/testing-functions.html<br>https://docs.github.com/en/actions/writing-workflows/choosing-what-your-workflow-does/control-the-concurrency-of-workflows-and-jobs |

### Kết quả đạt được tuần 8:

* Đang cập nhật.
