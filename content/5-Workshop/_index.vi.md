---
title: "Workshop"
date: 2026-09-14
weight: 5
chapter: false
pre: " <b> 5. </b> "
---

# Xây web bán hàng serverless có gợi ý sản phẩm trên AWS

#### Tổng quan

Workshop này hướng dẫn dựng lại **shop-ai (Black Magic)**, web bán laptop và linh kiện máy tính của nhóm, chạy hoàn toàn serverless trên AWS. Bạn sẽ deploy cả hệ thống vào tài khoản của mình bằng AWS CDK, rồi đi qua từng phần quan trọng:

- **Hạ tầng:** CloudFront làm cửa vào duy nhất cho web và API, S3 cho web tĩnh, Cognito cho đăng nhập, API Gateway kiểm JWT, Lambda và DynamoDB cho từng module.
- **Đặt hàng không mất, không trùng:** một giao dịch DynamoDB, `Idempotency-Key`, hàng đợi SQS có DLQ, email qua SES.
- **Gợi ý tự xây:** job Python chạy mỗi đêm bằng EventBridge Scheduler, dữ liệu người dùng được băm trước khi lưu, có thí nghiệm chống đầu độc gợi ý.
- **Vận hành:** log, trace, alarm, runbook; deploy bằng GitHub Actions qua OIDC; và dọn sạch tài nguyên khi xong.

Workshop do cả nhóm viết: An (5.1, 5.2, 5.3, 5.6, 5.7), Hoàng (5.4), Nhân (5.5). Mã nguồn: [github.com/hhieuann/shop-ai](https://github.com/hhieuann/shop-ai).

#### Nội dung

1. [Giới thiệu](5.1-introduction/)
2. [Chuẩn bị](5.2-prerequisite/)
3. [Triển khai hạ tầng](5.3-infrastructure/)
4. [Luồng bán hàng](5.4-salesflow/)
5. [Gợi ý và bảo mật dữ liệu](5.5-recommendations/)
6. [Kiểm thử và vận hành](5.6-testing/)
7. [Dọn dẹp](5.7-cleanup/)

{{% notice info %}}
Lab chạy trên sandbox riêng của bạn và gần như không tốn tiền nếu làm xong chương 5.7. Chi phí khi chạy thật một tháng (khoảng $24,86) nằm ở cuối chương 5.2.
{{% /notice %}}
