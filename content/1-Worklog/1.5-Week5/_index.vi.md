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
| 2 | - Rà soát việc của nhóm (#107, #109, #117–#125) <br> - Đọc thiết kế bảng `events`, `recs`, chia 3 câu hỏi mở <br> - Phân tích lockfile hỏng (#111, #115, #119) <br> - Cấu hình Dependabot (PR #127, #128, #129) <br> - **Giám sát bằng CloudWatch:** <br>&emsp; + SNS topic cảnh báo (PR #132, #134, #138) <br>&emsp; + 4 alarm, diễn tập trên dev (PR #136) <br>&emsp; + Runbook và dashboard (PR #140, #142) <br> - Tăng giới hạn Lambda chạy cùng lúc 10 → 1.000 | 12/10/2026 | 12/10/2026 | [Thiết kế NoSQL](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/bp-general-nosql-design.html) <br> [pnpm merge conflicts](https://pnpm.io/git#merge-conflicts) <br> [Dependabot options](https://docs.github.com/en/code-security/dependabot/working-with-dependabot/dependabot-options-reference) <br> [CloudWatch](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/WhatIsCloudWatch.html) <br> [SNS email](https://docs.aws.amazon.com/sns/latest/dg/sns-email-notifications.html) <br> [Alarm gửi email](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/AlarmThatSendsEmail.html) <br> [CloudWatch pricing](https://aws.amazon.com/cloudwatch/pricing/) <br> [SNS access policy](https://docs.aws.amazon.com/sns/latest/dg/sns-access-policy-use-cases.html) <br> [Logs Insights](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/AnalyzingLogData.html) <br> [Tăng quota](https://docs.aws.amazon.com/servicequotas/latest/userguide/request-quota-increase.html) <br> [CloudWatch dashboards](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/CloudWatch_Dashboards.html) |
| 3 | - Đọc báo cáo tiến độ của Hoàng <br> - Kiểm cấu hình E2E của môi trường dev trên GitHub <br> - Cập nhật checklist phát hành v0.1.0 (PR #160) <br> - Chốt công cụ E2E: Vitest + fetch thay cho Newman | 13/10/2026 | 13/10/2026 | [GitHub releases](https://docs.github.com/en/repositories/releasing-projects-on-github/managing-releases-in-a-repository) <br> [npm audit](https://docs.npmjs.com/cli/commands/npm-audit) <br> [InitiateAuth](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_InitiateAuth.html) |
| 4 | - Nhân nắm đủ bước nghiệm thu C1–C7 <br> - Chốt job đêm Scan bảng `events` <br> - Đề xuất một con trỏ phiên bản chung cho bảng `recs` | 14/10/2026 | 14/10/2026 | [DynamoDB TTL](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/TTL.html) <br> [DynamoDB Scan](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Scan.html) |
| 5 | - Review 2 PR E2E của Hoàng (#162, #164) <br> - Kiểm log deploy dev: E2E 4 file qua <br> - Rà điều kiện tách `release/v0.1.0`: đủ | 15/10/2026 | 15/10/2026 | [Vitest fileParallelism](https://vitest.dev/config/#fileparallelism) |
| 6 | - Tự học Cloud Practitioner Essentials, Module 12 <br> - Cloud Adoption Framework, chiến lược 7R, Snow Family | 16/10/2026 | 16/10/2026 | [Cloud Practitioner Essentials](https://skillbuilder.aws/learn/94T2BEN85A/aws-cloud-practitioner-essentials/) |
| 7 | - Tự học Cloud Practitioner Essentials, Module 13 <br> - 6 trụ cột Well-Architected Framework <br> - Chuẩn bị buổi họp nhóm online | 17/10/2026 | 17/10/2026 | [Cloud Practitioner Essentials](https://skillbuilder.aws/learn/94T2BEN85A/aws-cloud-practitioner-essentials/) <br> [Well-Architected Framework](https://docs.aws.amazon.com/wellarchitected/latest/framework/welcome.html) |

### Chi tiết công việc:

#### Thứ 2 (12/10/2026)

* **Rà soát việc cả nhóm sau cuối tuần:**
  * Nhân merge tài liệu nghiệp vụ gợi ý, sự kiện và script sinh dữ liệu v1 (uv, pytest, CI Python chạy được) (#107, #109).
  * Hoàng làm lại giao diện bản chàm (#117–#125).
* **Thiết kế bảng `events`, `recs` của Nhân:** đọc đề xuất, chia 3 câu hỏi mở cho đúng người phụ trách (luồng PURCHASE cho Hoàng; xuất dữ liệu và con trỏ ACTIVE cho Nhân và An); lưu ý Firehose không có trên Free plan.
* **Lockfile hỏng:** phân tích lỗi sau khi merge 2 PR Dependabot tạo từ cùng một gốc (#111, #115); Hoàng sửa ở #119.
* **Dependabot (PR #127):**
  * Tìm hiểu cách hoạt động: lịch, nhóm cập nhật, nhánh riêng cho mỗi PR, bản lớn và bản nhỏ.
  * Cấu hình không mở PR bản lớn cho npm và uv, gộp bản nhỏ của uv thành 1 PR.
  * Bot tự đóng 3 PR bản lớn và mở 2 PR mới (#128, #129); merge lần lượt, theo dõi 2 lần deploy dev: Lambda cập nhật thành công, smoke test 6/6.
* **Tìm hiểu Amazon CloudWatch:** logs, metrics, alarm, dashboard; xem số liệu thật của Lambda, API Gateway trên dev.
* **Thực hành: bước 1 phần giám sát (PR #132):** SNS topic nhận cảnh báo, bắt buộc HTTPS, đăng ký email truyền từ GitHub secret; thử trên sandbox và nhận email.
* **Xử lý 2 lỗi thật (PR #134):**
  * Secret email sai định dạng làm deploy dev rollback.
  * Email lộ trong log CI qua tên tài nguyên do CDK sinh từ email.
  * Sửa bằng kiểm email lúc synth và ID cố định cho đăng ký.
* **Thực hành: bước 2 phần giám sát, alarm CloudWatch (PR #136):** 5xx và độ trễ p95 của API, lỗi Lambda pre sign-up, throttle của tài khoản ở prod; báo vào SNS khi có sự cố và khi đã ổn. Thiết kế theo giá: vừa đúng 10 metric alarm miễn phí cho 3 môi trường.
* **Giới hạn Lambda:** phát hiện tài khoản chỉ được chạy 10 Lambda cùng lúc (dùng chung mọi môi trường), rủi ro cho load test tuần 6. Gửi yêu cầu Service Quotas tăng từ 10 lên 1.000, được duyệt trong ngày.
* **Diễn tập cảnh báo trên dev bằng `set-alarm-state`:**
  * Lần 1 phát hiện CloudWatch không được gửi vào SNS: `enforceSSL` tạo topic policy chỉ có Deny, thay mất policy mặc định.
  * Thêm quyền cho `cloudwatch.amazonaws.com` giới hạn theo `aws:SourceAccount`, `aws:SourceArn` (PR #138).
  * Lần 2 nhận đủ email ALARM và OK. Xoá log CI có email bị lộ.
* **Bước 4 phần giám sát: runbook cho 4 alarm (PR #140):** chạy thử runbook `api-5xx` trên dev bằng CloudWatch Logs Insights trên access log của API Gateway, sửa runbook cho khớp giao diện Console mới.
* **Bước 3 phần giám sát: dashboard CloudWatch tổng quan cho mỗi môi trường (PR #142):** trạng thái alarm, request và lỗi API, độ trễ p50/p95 so với mục tiêu 300 ms, lỗi và thời gian chạy từng Lambda, số Lambda chạy cùng lúc của tài khoản.

#### Thứ 3 (13/10/2026)

* **Báo cáo của Hoàng:** collection E2E Newman v1 kịp trước 15/10; trang danh sách sản phẩm mới (#154, #156, #158) vào v0.1.0; 2 tiêu đề merge commit bị dán nhầm.
* **Cấu hình E2E của môi trường dev trên GitHub:** kiểm biến, secret; chỉ rõ cho Hoàng tên biến mà bước E2E trong workflow truyền vào.
* **Cập nhật checklist phát hành v0.1.0 (PR #160):**
  * Phạm vi thêm giao diện mới và giám sát.
  * Bước nghiệm thu C1, C2, C7 theo giao diện mới; sửa release note.
  * Xác nhận email cảnh báo và diễn tập alarm ở staging, prod.
* **Quyết định công cụ E2E:**
  * Hoàng báo Newman 6.2.3 có lỗ hổng High/Critical trong thư viện phụ thuộc.
  * Tự cài thử và chạy `npm audit` để xác nhận: 11 High, 1 Critical, có lỗ hổng chưa có bản vá.
  * So sánh 3 hướng, chốt viết E2E bằng Vitest + fetch, không thêm ngoại lệ bảo mật cho cả repo.

#### Thứ 4 (14/10/2026)

* **Nghiệm thu và dữ liệu sự kiện:** Nhân xác nhận nắm đủ 7 bước nghiệm thu C1–C7 trên staging; chốt cách job đêm lấy dữ liệu sự kiện: Scan bảng `events` mỗi đêm (Firehose không có trên Free plan).
* **Con trỏ phiên bản của bảng `recs`:**
  * Giải thích cho Nhân khác biệt khi rollback giữa con trỏ riêng từng anchor và một con trỏ chung cho cả bảng.
  * Đề xuất con trỏ chung, giữ dữ liệu phiên bản cũ bằng TTL để rollback tức thì; anchor mới trả baseline hàng bán chạy.

#### Thứ 5 (15/10/2026)

* **Review 2 PR E2E của Hoàng (#162 ADR-0018 và tài liệu, #164 code):**
  * E2E API bằng Vitest + fetch, đăng nhập Cognito qua `InitiateAuth`, né bẫy `BASE_URL` của Vite.
  * Thiếu biến thì bỏ qua cả bộ, chạy tuần tự không thử lại, dọn giỏ sau khi chạy.
* **Log deploy dev:** bước E2E chạy thật, 4 file test qua (đăng nhập, xem hàng, giỏ hàng, ca lỗi) ngay sau smoke test.
* **Điều kiện trước khi tách `release/v0.1.0`:** rà lại, đủ điều kiện.

#### Thứ 6 (16/10/2026)

* **Tự học trên AWS Skill Builder:** khoá AWS Cloud Practitioner Essentials, Module 12 - Migrating to the AWS Cloud: AWS Cloud Adoption Framework (6 góc nhìn), các chiến lược di chuyển lên cloud (7R), AWS Snow Family, các dịch vụ hỗ trợ di chuyển.

#### Thứ 7 (17/10/2026)

* **Tự học trên AWS Skill Builder:** khoá AWS Cloud Practitioner Essentials, Module 13 - Well-Architected Solutions: 6 trụ cột của AWS Well-Architected Framework (vận hành, bảo mật, độ tin cậy, hiệu năng, chi phí, bền vững), liên hệ với phần giám sát và chi phí đã làm trong dự án.
* **Chuẩn bị buổi họp nhóm online:**
  * Tình hình chung và kế hoạch Sprint 2.
  * 5 quyết định cần chốt: lịch phát hành v0.1.0 mới, luồng sự kiện mua hàng, câu hỏi mở trong threat model, sandbox cho thành viên, ngưỡng mô hình gợi ý.

### Kết quả đạt được tuần 5:

* Dự án có phần giám sát trên CloudWatch: SNS topic cảnh báo, alarm vừa đúng 10 metric miễn phí, runbook cho 4 alarm, dashboard cho mỗi môi trường; diễn tập trên dev nhận đủ email ALARM và OK.
* Giới hạn Lambda chạy cùng lúc của tài khoản được tăng từ 10 lên 1.000 trước load test tuần 6.
* Dependabot được cấu hình không mở PR bản lớn cho npm và uv, gộp bản nhỏ của uv thành 1 PR.
* Nhóm chốt viết E2E bằng Vitest + fetch thay cho Newman; E2E đã chạy thật sau smoke test trên dev, 4 file qua.
* Đủ điều kiện tách `release/v0.1.0`; checklist phát hành đã cập nhật theo giao diện mới và phần giám sát.
* Tự học khoá AWS Cloud Practitioner Essentials trên Skill Builder, module 12–13.
