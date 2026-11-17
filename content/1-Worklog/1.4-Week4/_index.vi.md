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
| 2 | - Cập nhật checklist tuần 1 của trưởng nhóm (PR #33) <br> - Review và merge 5 PR Dependabot, gỡ xung đột lockfile <br> - Nâng `@vitejs/plugin-react` cho Vite 8 (PR #40) <br> - Kiểm thử thủ công web trên API giả lập (Prism) | 05/10/2026 | 05/10/2026 | [Dependabot](https://docs.github.com/en/code-security/dependabot) <br> [React Router upgrading](https://reactrouter.com/upgrading/component-routes) <br> [Vite migration](https://vite.dev/guide/migration) <br> [PR #40](https://github.com/hhieuann/shop-ai/pull/40) |
| 3 | - Tự học Cloud Practitioner Essentials, Module 10 <br> - CloudWatch, CloudTrail, Trusted Advisor, quản trị và tuân thủ | 06/10/2026 | 06/10/2026 | [Cloud Practitioner Essentials](https://skillbuilder.aws/learn/94T2BEN85A/aws-cloud-practitioner-essentials/) |
| 4 | - Tự học Cloud Practitioner Essentials, Module 11 <br> - Mô hình tính giá, Pricing Calculator, Budgets, Cost Explorer, gói hỗ trợ | 07/10/2026 | 07/10/2026 | [Cloud Practitioner Essentials](https://skillbuilder.aws/learn/94T2BEN85A/aws-cloud-practitioner-essentials/) |
| 5 | - Sửa lỗi deploy sandbox trên Windows (PR #64) <br> - **Stack web bằng CDK:** <br>&emsp; + S3 + CloudFront qua OAC, deploy dev (PR #66) <br>&emsp; + Health và cache danh sách sản phẩm 60 giây (PR #68) <br> - **Đăng nhập bằng Cognito:** <br>&emsp; + User pool, JWT authorizer, `GET /api/v1/me` (PR #70) <br>&emsp; + Pre sign-up, `/config.json`, web Amplify v6 (PR #72, #74) <br>&emsp; + App client riêng cho E2E (PR #83) <br> - Smoke test, cost allocation tag, nghiệp vụ demo (PR #76, #81) <br> - Review PR #79, viết ADR-0017 cho giỏ hàng (PR #85) <br> - Idempotency, X-Ray, lỗi 401 của authorizer (PR #87, #91, #95) <br> - Đọc nội quy workshop, dựng trang báo cáo | 08/10/2026 | 08/10/2026 | [S3 qua OAC](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/private-content-restricting-access-to-s3.html) <br> [CloudFront Functions](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/cloudfront-functions.html) <br> [Cache key](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/controlling-the-cache-key.html) <br> [Nội quy dự án FCAJ](https://hcm-rules.awsfcaj.com/3-project/) <br> [Cognito user pools](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-pools.html) <br> [JWT authorizer](https://docs.aws.amazon.com/apigateway/latest/developerguide/http-api-jwt-authorizer.html) <br> [Pre sign-up trigger](https://docs.aws.amazon.com/cognito/latest/developerguide/user-pool-lambda-pre-sign-up.html) <br> [Amplify sign-in](https://docs.amplify.aws/react/build-a-backend/auth/connect-your-frontend/sign-in/) <br> [Vitest env](https://vitest.dev/config/#env) <br> [Cost allocation tags](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/cost-alloc-tags.html) <br> [Powertools Idempotency](https://docs.powertools.aws.dev/lambda/typescript/latest/features/idempotency/) <br> [Powertools Tracer](https://docs.powertools.aws.dev/lambda/typescript/latest/core/tracer/) |
| 6 | - Review 2 PR giỏ hàng của Hoàng (#94, #97) <br> - Rà soát chuẩn bị tách `release/v0.1.0` <br> - Smoke test tự lấy URL từ output (PR #99) <br> - Kiểm trust policy của 3 role deploy OIDC <br> - Viết checklist phát hành v0.1.0 (PR #101) <br> - Sửa test idempotency flaky trên CI (PR #103) <br> - Nạp 104 sản phẩm demo ở prod (PR #105) | 09/10/2026 | 09/10/2026 | [GitHub environments](https://docs.github.com/en/actions/managing-workflow-runs-and-deployments/managing-deployments/managing-environments-for-deployment) <br> [cdk deploy](https://docs.aws.amazon.com/cdk/v2/guide/ref-cli-cmd-deploy.html) |

### Chi tiết công việc:

#### Thứ 2 (05/10/2026)

* **Checklist tuần 1:** cập nhật checklist tuần 1 của trưởng nhóm (PR #33).
* **5 PR cập nhật thư viện của Dependabot:** review và merge các bản TypeScript 6, Vite 8, React Router 7, jsdom 30; xử lý xung đột `pnpm-lock.yaml` giữa các PR.
* **`@vitejs/plugin-react`:** nâng lên bản hỗ trợ Vite 8 (PR #40).
* **Kiểm thử thủ công sau khi nâng React Router 7:** chạy web trên API giả lập (Prism), đi qua trang chủ, danh sách, chi tiết, giỏ hàng, đặt hàng, đăng nhập, 404.

#### Thứ 3 (06/10/2026)

* **Tự học trên AWS Skill Builder:** khoá AWS Cloud Practitioner Essentials, Module 10 - Monitoring, Compliance and Governance in the AWS Cloud: Amazon CloudWatch, AWS CloudTrail, AWS Trusted Advisor, quản trị và tuân thủ.

#### Thứ 4 (07/10/2026)

* **Tự học trên AWS Skill Builder:** khoá AWS Cloud Practitioner Essentials, Module 11 - Pricing and Support: mô hình tính giá, AWS Pricing Calculator, AWS Budgets, Cost Explorer, các gói hỗ trợ.

#### Thứ 5 (08/10/2026)

* **Sửa lỗi deploy sandbox trên Windows (PR #64):** lệnh `cp` khi đóng gói Lambda nạp dữ liệu; sandbox tự nạp 104 sản phẩm demo.
* **Thực hành: stack web bằng AWS CDK (PR #66):**
  * Bucket S3 riêng tư, CloudFront đọc qua Origin Access Control.
  * CloudFront Function đưa route của React về `index.html`.
  * Behavior `/api/*` chuyển sang HTTP API, cùng tên miền nên không cần CORS.
  * Deploy sandbox rồi dev qua GitHub Actions (OIDC).
* **Health và cache (PR #68):** endpoint `/api/v1/health` và cache CloudFront 60 giây cho danh sách sản phẩm; kiểm `X-Cache` Miss rồi Hit.
* **Báo cáo FCAJ:** đọc nội quy workshop và tiêu chí chấm; dựng trang báo cáo từ template FCAJ.
* **Thực hành: đăng nhập bằng Amazon Cognito (PR #70):** user pool đăng nhập bằng email, MFA tuỳ chọn TOTP, app client SRP không secret; JWT authorizer cho HTTP API; endpoint `GET /api/v1/me` đọc thông tin từ token.
* **Pre sign-up và cấu hình lúc chạy (PR #72):** Lambda trigger Pre sign-up chặn trùng username; CloudFront phát `/config.json` để web đọc cấu hình Cognito lúc chạy.
* **Web đăng nhập (PR #74):** đăng nhập, tạo tài khoản, xác nhận email, xác thực hai lớp bằng Amplify v6 (SRP), token lưu cookie Secure; 29 test giao diện.
* **Smoke test (PR #76):** chạy sau mỗi deploy qua CloudFront (health, sản phẩm, 404, 401, web, config.json); xử lý trùng tên biến BASE_URL với Vite.
* **Chi phí:** bật cost allocation tag `project`, `env`, `module` để đo chi phí theo dự án, môi trường, module.
* **Nghiệp vụ tài khoản và quy mô demo (PR #81):** số liệu giả định, mức tải k6, ngân sách.
* **E2E (PR #83):** app client Cognito riêng cho E2E (luồng `USER_PASSWORD_AUTH`, chỉ dev và staging); tạo user E2E trên dev, đặt biến và secret cho môi trường dev trên GitHub.
* **Review PR #79 của Hoàng:** web không gọi lại API khi nhận lỗi 4xx; merge.
* **ADR-0017 cho module giỏ hàng (PR #85):** chốt đề xuất kiến trúc của Hoàng:
  * cart chỉ đọc bảng products bằng BatchGetItem;
  * ordering chỉ UpdateItem trong giao dịch đặt hàng;
  * bảng idempotency riêng từng module.
* **Helper idempotency dùng chung (PR #87):** dùng Powertools for AWS Lambda.
  * Gọi lại cùng `Idempotency-Key` trả kết quả cũ, khác nội dung trả 422, đang xử lý trả 409.
  * 5 integration test với DynamoDB Local; xử lý cảnh báo nhầm của gitleaks.
* **Thực hành: AWS X-Ray bằng Powertools Tracer (PR #91):**
  * Subsegment cho handler và cho từng lần gọi DynamoDB, Cognito.
  * Annotation cold start, route, mã trạng thái để lọc trace lỗi.
  * Không ghi response vào trace để tránh lộ dữ liệu cá nhân.
* **Lỗi 401 của JWT authorizer không theo RFC 9457 (PR #95):**
  * Thử sửa bằng custom error response của CloudFront thì `cdk synth` báo CloudFront không nhận mã 401.
  * So sánh Lambda@Edge và tự kiểm JWT trong Lambda; chốt ghi rõ vào hợp đồng OpenAPI và ADR-0007, thêm smoke test.

#### Thứ 6 (09/10/2026)

* **Review 2 PR giỏ hàng trên server của Hoàng (#94, #97) theo ADR-0017:** Lambda cart chỉ có `BatchGetItem` trên bảng products, bảng `cart-idempotency` riêng, mọi route gắn JWT authorizer.
* **Rà soát chuẩn bị tách `release/v0.1.0`:** environment `staging` (chỉ nhánh `release/*`), `production` (chỉ tag `v*`, 1 người khác duyệt), vai trò OIDC; phát hiện staging và production chưa có URL cho smoke test.
* **Smoke test tự lấy URL (PR #99):** lấy URL CloudFront từ output của stack sau khi deploy, bỏ bước đặt tay biến khi phát hành; kiểm lại trên dev: smoke test tự chạy với URL đọc từ output, 6/6 qua.
* **Trust policy của 3 role deploy OIDC (dev, staging, production):** mỗi role chỉ nhận đúng repo và đúng environment.
* **Checklist phát hành v0.1.0 (PR #101):**
  * Phạm vi, lệnh tách nhánh, người dùng E2E cho staging.
  * 18 bước nghiệm thu chia theo người (người test khác người viết).
  * Tag và duyệt deploy prod, xử lý khi deploy lỗi.
  * Phát hiện prod chưa có dữ liệu sản phẩm.
* **Test idempotency bị flaky trên CI (PR #103):** hai request đua nhau ghi bản ghi IN_PROGRESS; sửa bằng cách chờ use case lần đầu bắt đầu rồi mới gọi lần hai.
* **Nạp 104 sản phẩm demo ở prod để demo v0.1.0 (PR #105):** prod chỉ thêm sản phẩm còn thiếu bằng `PutItem` có điều kiện `attribute_not_exists`, không ghi đè tồn kho và giá; role chỉ có `PutItem`.

### Kết quả đạt được tuần 4:

* Web của nhóm chạy trên dev qua CloudFront với 104 sản phẩm demo; mỗi lần merge vào `develop` thì cả API và web tự cập nhật.
* Hiểu cách bảo vệ bucket S3 bằng Origin Access Control: bucket không public, chỉ CloudFront đọc được.
* Hiểu cache key của CloudFront: cache danh sách sản phẩm 60 giây theo từng query string, các API khác không cache; endpoint health không bao giờ cache.
* Biết xử lý chuỗi PR cập nhật thư viện: merge lần lượt, để Dependabot tự cập nhật lại PR khi lockfile xung đột, kiểm lại cả tổ hợp sau cùng.
* Rút kinh nghiệm: lệnh shell trong bước đóng gói CDK phải chạy được trên cả Windows lẫn Linux; synth trên CI không có account nên tên ghi nhận cdk-nag không được phụ thuộc account.
* Tự học khoá AWS Cloud Practitioner Essentials trên Skill Builder, module 10–11: nắm kiến thức nền về dịch vụ AWS để áp dụng vào lab và dự án.
