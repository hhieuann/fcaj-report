---
title: "Worklog Tuần 9"
date: 2026-09-14
weight: 9
chapter: false
pre: " <b> 1.9. </b> "
---

### Mục tiêu tuần 9:

* Chốt các việc trước ngày tách `release/v1.0.0` (10/11): load test k6, diễn tập khôi phục PITR, runbook còn thiếu, cập nhật checklist.
* Nghiệm thu `v1.0.0-rc.1` trên staging 10–11/11, gắn tag lên prod 12/11.
* Đo số hiệu năng và phục hồi (P1–P3) cho báo cáo.

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
|-----|-----------|--------------|-----------------|----------------|
| 2 | - Merge #393 (tình trạng của Hoàng trong checklist rc.1) và #395: kịch bản k6 `smoke.js` chạy mỗi đêm vào staging qua CloudFront, `tiers.js` đo 50 và 200 người ảo bắn thẳng API Gateway, có phần đặt rồi huỷ đơn khi có tài khoản E2E. Chạy thử: smoke 105 request, 0 lỗi, p95 162 ms; tiers 5 người ảo p95 220 ms<br>- Viết runbook `pitr-restore.md` và `cost-spike.md` (#401). Sửa câu trong kế hoạch "khôi phục sang bảng mới rồi trỏ lại": Lambda không có quyền IAM trên bảng mới, nên phải chép item cần về bảng cũ<br>- **Diễn tập PITR trên staging (P3):** khôi phục bảng `orders` (23 item) về thời điểm 5 phút trước, bảng mới ACTIVE sau 3–10 phút, 23 = 23 item, GSI `byStatus` ACTIVE; bảng mới không có TTL, PITR, deletion protection như dự đoán. Xoá bảng tạm. Biết thêm: bảng chỉ 9 KB vẫn mất vài phút, waiter `table-exists` chỉ chờ khoảng 8 phút<br>- Kiểm kết quả Hoàng kiểm trước admin AD1–AD3 trên dev: deploy `9f9adf0` xanh, 2 test E2E 401/403 đạt, nhóm `admin` dev chỉ có tài khoản của Hoàng. Ghi là kiểm trước vì người viết tự test; nghiệm thu chính thức vẫn trên staging<br>- Cập nhật checklist rc.1 (#403): deploy dev xanh (E2E 46/46), nightly ZAP và Schemathesis xanh, Dependabot và CodeQL không có cảnh báo High, ruleset release cấm xoá nhánh, chi phí 01–09/11 dev $0,022, staging $0,020, prod $0,001; MFA #183 dời sau `v1.0.0`. Báo cả nhóm lịch nghiệm thu 10–11/11<br>- Nhận số Lighthouse P1 của Hoàng trên dev: điện thoại 99/98/99, LCP trung vị lớn nhất 2,11 s, CLS lớn nhất 0,043, JS 131–147 KB, đạt cả 4 mốc<br>- Chốt C1 (gọi thẳng `execute-api` bỏ qua WAF) với Nhân: ghi là giới hạn đã biết cho rc.1. Kiểm lý do của Nhân trong code: throttle stage 50/100, mọi route ghi gắn JWT authorizer, spam sự kiện giữ ở 0,3–2% theo thí nghiệm đầu độc. Ghi C1 và P1 vào checklist (#407)<br>- Merge 3 PR Dependabot không đụng code chạy (#397–#399); giữ #400 (17 thư viện web và Lambda) tới sau khi tách để bản rc.1 đúng mã đã đo P1<br>- Review và duyệt #405 của Hoàng (spec trang đơn hàng): đối chiếu với code; nhắc 5 PR code đi kèm mở sau ngày tách | 09/11/2026 | 09/11/2026 | https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/PointInTimeRecovery.html<br>https://grafana.com/docs/k6/latest/using-k6/scenarios/<br>https://docs.aws.amazon.com/cost-management/latest/userguide/budgets-managing-costs.html |
| 3 | - Kiểm nightly đêm 09/11: ZAP, Schemathesis xanh; k6 smoke chạy thật lần đầu, 73 request, 0 lỗi, p95 823 ms (gần ngưỡng 1000 ms do Lambda staging chạy lạnh lúc nửa đêm)<br>- **Tách `release/v1.0.0`** (`ae8fcfa`), nối `main` ngay lúc tách để PR vào `main` không bị `BEHIND`. Kiểm mã chạy thật trên nhánh giống hệt `9f9adf0` (bản đã đo P1, kiểm trước admin). Staging deploy xanh: smoke test, E2E<br>- Review #409 (API trả `firstItemName`, API admin giữ 5 trường bằng kiểu riêng) và #411 (`Button`, `Badge`, `EmptyState`, `ConfirmDialog` có `tone`) của Hoàng; duyệt rồi merge vào `develop` sau khi tách, cùng #400. Dev deploy xanh<br>- Chạy tay job gợi ý trên staging: 104 sản phẩm, 3 loại gợi ý đều bật phiên bản `20261110` (ALSO_BOUGHT hit rate 0,54 so với baseline 0,28), log không lỗi<br>- **Load test P2 trên staging:** đọc 50 người ảo p95 127 ms, 200 người ảo (36 request/giây) p95 108 ms, 0 lỗi, không bị throttle; đặt rồi huỷ đơn 80 lần p95 269 ms, 0 lỗi. Sau đó không còn đơn đang mở, hàng đợi và DLQ về 0<br>- Không nhớ mật khẩu người dùng E2E staging: đặt mật khẩu ngẫu nhiên mới lên Cognito và secret GitHub mà không hiện ra màn hình; chạy lại deploy staging, E2E đặt hàng 9/9 đạt với secret mới<br>- Sai sót: lần đầu đưa lệnh k6 còn chỗ trống `<user E2E>` nên lệnh lỗi; lần sau lấy giá trị thật từ biến environment GitHub | 10/11/2026 | 10/11/2026 | https://grafana.com/docs/k6/latest/using-k6/thresholds/<br>https://docs.aws.amazon.com/cognito/latest/developerguide/user-pool-settings-passwords.html<br>https://docs.github.com/en/actions/security-for-github-actions/security-guides/using-secrets-in-github-actions |

### Kết quả đạt được tuần 9:

* Đang cập nhật.
