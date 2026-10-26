---
title: "Worklog Tuần 7"
date: 2026-09-14
weight: 7
chapter: false
pre: " <b> 1.7. </b> "
---

### Mục tiêu tuần 7:

* Chuẩn bị phát hành `v0.2.0`: tách nhánh release 30/10, gắn tag 02/11.
* Đưa gợi ý sản phẩm chạy thật trên dev: pipeline mỗi đêm của Nhân ghi bảng `recs`, API gợi ý trả dữ liệu, widget trên web.
* Hoàn thành và đăng Blog 1.

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
|-----|-----------|--------------|-----------------|----------------|
| 2 | - Rà các PR nhóm merge cuối tuần chưa có người duyệt: hạ tầng worker xác nhận đơn (#263), email SES của worker (#265), sửa E2E đặt hàng (#267), job gợi ý mỗi đêm của Nhân (#269). Worker có quyền IAM hẹp, chỉ worker được gửi tin lên topic `order-confirmed`, giới hạn 2 lần chạy cùng lúc cho hợp mức 1 thư/giây của SES sandbox; code email đúng các điểm đã chốt nhưng chưa bật vì còn chờ PR quyền SES, Cognito. Tìm nguyên nhân deploy đỏ của #265 (E2E chờ đơn `PENDING` trong khi worker đã xác nhận), đã được #267 sửa<br>- Duyệt và merge 4 PR Dependabot (17 gói npm gồm AWS SDK, CDK, vite; ruff; uv-build; action setup-uv); deploy dev cuối cùng xanh, kiểm lại `uv.lock` và `pnpm-lock.yaml` vẫn khớp<br>- Chuẩn bị Blog 1 về deploy bằng OIDC: đối chiếu từng chi tiết kỹ thuật với template `github-oidc.yaml`, workflow, PR #25, lượt chạy đo thời gian, quy tắc GitHub Environments và tài liệu GitHub về subject bất biến; cập nhật số liệu (118 lượt deploy thành công trên `develop`); sửa phần sự cố cho đúng những gì còn nhớ và đối chiếu được; sơ đồ minh họa làm với công cụ AI hỗ trợ và ghi rõ trong bài. Chưa đăng<br>- Chốt review OIDC (#180) theo hướng chấp nhận rủi ro: kiểm lại 5 lớp chặn trên GitHub (environment chỉ nhận đúng nhánh/tag, `production` cần 1 trong 3 người duyệt, chặn tự duyệt, admin không được bỏ qua, chỉ admin tạo tag `v*`); sửa ADR-0013 cho đúng việc role deploy vẫn dùng `cfn-exec-role` có AdministratorAccess, cập nhật threat model C6, E9 (PR #278, chờ duyệt)<br>- Sửa 2 alarm báo nhầm trên staging ngày 21/10 (PR #280, chờ duyệt): `api-latency-p95` dùng metric math chỉ tính chu kỳ có từ 20 request; `presignup-errors` trừ lần từ chối đăng ký đếm bằng metric filter trên log Lambda, vẫn báo lỗi thật như timeout | 26/10/2026 | 26/10/2026 | https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/using-metric-math.html<br>https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/FilterAndPatternSyntax.html<br>https://docs.github.com/en/actions/reference/security/oidc<br>https://docs.aws.amazon.com/STS/latest/APIReference/API_AssumeRoleWithWebIdentity.html<br>https://docs.aws.amazon.com/lambda/latest/dg/services-sqs-scaling.html |

### Kết quả đạt được tuần 7:

* Đang cập nhật.
