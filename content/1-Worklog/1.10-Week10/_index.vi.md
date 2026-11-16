---
title: "Worklog Tuần 10"
date: 2026-09-14
weight: 10
chapter: false
pre: " <b> 1.10. </b> "
---

### Mục tiêu tuần 10:

* Chuẩn bị `v1.0.0` (23/11): chuyển core rule set của WAF sang chặn sau khi đọc log prod, đưa nhánh release về `develop`, tập demo trên prod.
* Viết các phần báo cáo FCAJ còn trống trước hạn khoảng 29/11: Proposal, Workshop (kiến trúc, từng bước, kiểm thử, chi phí, dọn dẹp), Events; đưa số P1–P3 vào báo cáo.
* Review các PR của nhóm cho bản sau v1.0.0 (admin huỷ đơn, quên mật khẩu) và viết Blog 3.

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
|-----|-----------|--------------|-----------------|----------------|
| 2 | - Đọc mail cảnh báo Free Tier của AWS (15/11): CloudWatch Alarms đã dùng hơn 85% hạn mức miễn phí tháng 11 (8,5/10). Đếm alarm trên mọi vùng: 13 alarm, đều ở `ap-southeast-1` và đều OK (prod 7, staging 3, dev 3). AWS tính theo số metric nên các alarm dùng metric math (latency p95, presignup errors, recs pipeline) tính 2, tổng 20 alarm-metric, vượt khoảng 10, tức khoảng $1/tháng, trừ vào credit. Quyết định giữ nguyên vì alarm dev và staging bắt lỗi trước prod và là bằng chứng giám sát cho workshop; nếu muốn bỏ thì phải sửa `monitoring.ts`, không xoá tay | 16/11/2026 | 16/11/2026 | https://aws.amazon.com/cloudwatch/pricing/<br>https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/tracking-free-tier-usage.html |

### Kết quả đạt được tuần 10:

* Đang cập nhật.
