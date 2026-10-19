---
title: "Worklog Tuần 6"
date: 2026-09-14
weight: 6
chapter: false
pre: " <b> 1.6. </b> "
---

### Mục tiêu tuần 6:

* Hoàn tất phát hành `v0.1.0`: nghiệm thu trên staging, gắn tag và deploy prod.
* Bắt đầu Sprint 2: hạ tầng cho luồng sự kiện mua hàng, khoá HMAC, API gợi ý.

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
|-----|-----------|--------------|-----------------|----------------|
| 2 | - Tách nhánh `release/v0.1.0` từ `develop` (trễ 3 ngày so với kế hoạch, báo lại lịch mới cho nhóm: nghiệm thu 19–21/10, tag 22/10)<br>- Staging deploy lần đầu thành công: 2 stack `shop-stg-api`, `shop-stg-web` gồm cả alarm, dashboard; 104 sản phẩm demo; smoke test tự lấy URL từ output<br>- Tạo người dùng E2E trên user pool staging, đặt biến và secret cho environment `staging` trên GitHub; xử lý lỗi dán nhiều lệnh làm mật khẩu bị ghi sai bằng một lệnh tự sinh mật khẩu ngẫu nhiên<br>- Chạy lại deploy staging: smoke 6/6, E2E API 4/4 file qua; gửi URL staging cho nhóm nghiệm thu<br>- Review và merge 3 PR Dependabot (17 bản nhỏ của npm, 2 bản của công cụ Python) và PR hợp đồng route sự kiện của Nhân vào `develop` sau khi đã tách release; kiểm lockfile Python vẫn khớp; 2 lần deploy dev xanh, smoke 6/6, E2E 4/4 | 19/10/2026 | 19/10/2026 | https://docs.aws.amazon.com/cognito/latest/developerguide/how-to-create-user-accounts.html<br>https://cli.github.com/manual/gh_secret_set |

### Kết quả đạt được tuần 6:

* Đang cập nhật.
