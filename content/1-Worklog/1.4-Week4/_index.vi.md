---
title: "Worklog Tuần 4"
date: 2026-09-14
weight: 4
chapter: false
pre: " <b> 1.4. </b> "
---

### Mục tiêu tuần 4:

* Đưa web của nhóm lên môi trường dev trên AWS: S3 + CloudFront, `/api/*` chuyển sang API Gateway (tiêu chí tuần 2 của dự án).
* Giữ thư viện của dự án luôn mới và an toàn; review PR của thành viên.
* Nắm nội quy báo cáo và workshop của chương trình, dựng trang báo cáo theo template.

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
|-----|-----------|--------------|-----------------|----------------|
| 2 | - Cập nhật checklist tuần 1 của trưởng nhóm (PR #33)<br>- Review và merge 5 PR cập nhật thư viện của Dependabot (TypeScript 6, Vite 8, React Router 7, jsdom 30); xử lý xung đột `pnpm-lock.yaml` giữa các PR<br>- Nâng `@vitejs/plugin-react` lên bản hỗ trợ Vite 8 (PR #40)<br>- Kiểm thử thủ công web trên API giả lập (Prism) sau khi nâng React Router 7: trang chủ, danh sách, chi tiết, giỏ hàng, đặt hàng, đăng nhập, 404 | 05/10/2026 | 05/10/2026 | https://docs.github.com/en/code-security/dependabot<br>https://reactrouter.com/upgrading/component-routes<br>https://vite.dev/guide/migration<br>https://github.com/hhieuann/shop-ai/pull/40 |
| 3 | - Tự học trên AWS Skill Builder, khoá AWS Cloud Practitioner Essentials, Module 10 - Monitoring, Compliance and Governance in the AWS Cloud: Amazon CloudWatch, AWS CloudTrail, AWS Trusted Advisor, quản trị và tuân thủ | 06/10/2026 | 06/10/2026 | https://skillbuilder.aws/learn/94T2BEN85A/aws-cloud-practitioner-essentials/ |
| 4 | - Tự học trên AWS Skill Builder, khoá AWS Cloud Practitioner Essentials, Module 11 - Pricing and Support: mô hình tính giá, AWS Pricing Calculator, AWS Budgets, Cost Explorer, các gói hỗ trợ | 07/10/2026 | 07/10/2026 | https://skillbuilder.aws/learn/94T2BEN85A/aws-cloud-practitioner-essentials/ |
| 5 | - Sửa lỗi deploy sandbox trên Windows (lệnh `cp` khi đóng gói Lambda nạp dữ liệu, PR #64); sandbox tự nạp 104 sản phẩm demo<br>- **Thực hành:** stack web bằng AWS CDK (PR #66):<br>&emsp; + Bucket S3 riêng tư, CloudFront đọc qua Origin Access Control<br>&emsp; + CloudFront Function đưa route của React về `index.html`<br>&emsp; + Behavior `/api/*` chuyển sang HTTP API, cùng tên miền nên không cần CORS<br>&emsp; + Deploy sandbox rồi dev qua GitHub Actions (OIDC)<br>- Endpoint `/api/v1/health` và cache CloudFront 60 giây cho danh sách sản phẩm (PR #68); kiểm `X-Cache` Miss rồi Hit<br>- Đọc nội quy workshop và tiêu chí chấm; dựng trang báo cáo từ template FCAJ<br>- **Thực hành:** đăng nhập bằng Amazon Cognito (PR #70): user pool đăng nhập bằng email, MFA tuỳ chọn TOTP, app client SRP không secret; JWT authorizer cho HTTP API; endpoint `GET /api/v1/me` đọc thông tin từ token<br>- Lambda trigger Pre sign-up chặn trùng username; CloudFront phát `/config.json` để web đọc cấu hình Cognito lúc chạy (PR #72)<br>- Web đăng nhập, tạo tài khoản, xác nhận email, xác thực hai lớp bằng Amplify v6 (SRP), token lưu cookie Secure; 29 test giao diện (PR #74)<br>- Smoke test chạy sau mỗi deploy qua CloudFront (health, sản phẩm, 404, 401, web, config.json); xử lý trùng tên biến BASE_URL với Vite (PR #76)<br>- Bật cost allocation tag `project`, `env`, `module` để đo chi phí theo dự án, môi trường, module<br>- Viết nghiệp vụ tài khoản và quy mô demo: số liệu giả định, mức tải k6, ngân sách (PR #81)<br>- App client Cognito riêng cho E2E (luồng `USER_PASSWORD_AUTH`, chỉ dev và staging, PR #83); tạo user E2E trên dev, đặt biến và secret cho môi trường dev trên GitHub<br>- Review và merge PR #79 của Hoàng (web không gọi lại API khi nhận lỗi 4xx)<br>- Chốt đề xuất kiến trúc module giỏ hàng của Hoàng, viết ADR-0017 (PR #85): cart chỉ đọc bảng products bằng BatchGetItem, ordering chỉ UpdateItem trong giao dịch đặt hàng, bảng idempotency riêng từng module | 08/10/2026 | 08/10/2026 | https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/private-content-restricting-access-to-s3.html<br>https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/cloudfront-functions.html<br>https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/controlling-the-cache-key.html<br>https://hcm-rules.awsfcaj.com/3-project/<br>https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-pools.html<br>https://docs.aws.amazon.com/apigateway/latest/developerguide/http-api-jwt-authorizer.html<br>https://docs.aws.amazon.com/cognito/latest/developerguide/user-pool-lambda-pre-sign-up.html<br>https://docs.amplify.aws/react/build-a-backend/auth/connect-your-frontend/sign-in/<br>https://vitest.dev/config/#env<br>https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/cost-alloc-tags.html |

### Kết quả đạt được tuần 4:

* Web của nhóm chạy trên dev qua CloudFront với 104 sản phẩm demo; mỗi lần merge vào `develop` thì cả API và web tự cập nhật.
* Hiểu cách bảo vệ bucket S3 bằng Origin Access Control: bucket không public, chỉ CloudFront đọc được.
* Hiểu cache key của CloudFront: cache danh sách sản phẩm 60 giây theo từng query string, các API khác không cache; endpoint health không bao giờ cache.
* Biết xử lý chuỗi PR cập nhật thư viện: merge lần lượt, để Dependabot tự cập nhật lại PR khi lockfile xung đột, kiểm lại cả tổ hợp sau cùng.
* Rút kinh nghiệm: lệnh shell trong bước đóng gói CDK phải chạy được trên cả Windows lẫn Linux; synth trên CI không có account nên tên ghi nhận cdk-nag không được phụ thuộc account.
* Tự học khoá AWS Cloud Practitioner Essentials trên Skill Builder, module 10–11: nắm kiến thức nền về dịch vụ AWS để áp dụng vào lab và dự án.
