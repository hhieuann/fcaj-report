---
title: "Worklog Tuần 3"
date: 2026-09-14
weight: 3
chapter: false
pre: " <b> 1.3. </b> "
---

### Mục tiêu tuần 3:

* Khởi động dự án nhóm shop-ai (shop linh kiện serverless có gợi ý mua kèm): chốt kế hoạch, phân vai, quy trình làm việc.
* Dựng nền tảng: repo GitHub có CI; tài khoản AWS sẵn sàng deploy bằng code (CDK) và deploy tự động không cần access key (OIDC).
* Làm module mẫu đầu tiên chạy thật trên AWS để cả nhóm làm theo.

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
|-----|-----------|--------------|-----------------|----------------|
| 2 | - Tự học trên AWS Skill Builder, khoá AWS Cloud Practitioner Essentials, Module 7 - Databases: Amazon RDS, Aurora, DynamoDB và các cơ sở dữ liệu chuyên dụng khác | 28/09/2026 | 28/09/2026 | https://skillbuilder.aws/learn/94T2BEN85A/aws-cloud-practitioner-essentials/ |
| 3 | - Tự học trên AWS Skill Builder, khoá AWS Cloud Practitioner Essentials, Module 8 - AI ML and Data Analytics: các dịch vụ AI/ML và phân tích dữ liệu của AWS (Amazon SageMaker, Amazon Bedrock, Amazon Athena…) | 29/09/2026 | 29/09/2026 | https://skillbuilder.aws/learn/94T2BEN85A/aws-cloud-practitioner-essentials/ |
| 4 | - Rà soát kế hoạch dự án của nhóm, đánh giá rủi ro chi phí và phạm vi: Amazon Personalize trên Free plan, chi phí Campaign chạy liên tục, quy mô load test<br>- Thống nhất kế hoạch với nhóm; lập kế hoạch kỹ thuật từ build, test tới deploy và vận hành<br>- Tạo repo public `hhieuann/shop-ai`: README, git flow, hướng dẫn kiểm thử, 12 ADR, mẫu issue/PR, workflow GitHub Actions<br>- Cấu hình GitHub: ruleset bảo vệ nhánh `main`, `develop`, `release/*`; environment dev/staging/production; Dependabot, secret scanning, CodeQL<br>- Tìm hiểu kiến trúc modular monolith và Hexagonal rút gọn (ADR-0009) | 30/09/2026 | 30/09/2026 | https://github.com/hhieuann/shop-ai<br>https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/managing-rulesets/about-rulesets<br>https://www.conventionalcommits.org/ |
| 5 | - Thêm Hoàng và Nhân vào repo; viết tài liệu vai trò, việc cần làm và lộ trình 8 tuần cho từng thành viên; cấu hình CODEOWNERS<br>- Đổi luật nhánh `develop`: không bắt buộc duyệt, CI xanh là merge được<br>- Tạo bảng GitHub Projects (Kanban, thêm cột Released) cho cả nhóm<br>- Hỏi mentor về Paid plan và cách chấm workshop → quyết định tự xây mô hình gợi ý trên Lambda + DynamoDB thay Amazon Personalize (ADR-0016); workshop chấm theo nhóm<br>- Cài pnpm, AWS CLI v2; đăng nhập CLI bằng `aws login` (phiên tạm, không tạo access key)<br>- Thêm cảnh báo AWS Budgets; CDK bootstrap ở ap-southeast-1 và us-east-1<br>- Tạo OIDC provider và 4 IAM role cho GitHub Actions (deploy dev, staging, prod và xem diff chỉ đọc)<br>- Bắt đầu module mẫu catalog: domain, use case, adapter DynamoDB; integration test với DynamoDB Local qua Testcontainers | 01/10/2026 | 01/10/2026 | https://docs.github.com/en/issues/planning-and-tracking-with-projects<br>https://docs.aws.amazon.com/cli/latest/userguide/<br>https://docs.aws.amazon.com/cost-management/latest/userguide/budgets-managing-costs.html<br>https://docs.aws.amazon.com/cdk/v2/guide/bootstrapping.html<br>https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_providers_create_oidc.html<br>https://node.testcontainers.org/ |
| 6 | - Tự học trên AWS Skill Builder, khoá AWS Cloud Practitioner Essentials, Module 9 - Security: IAM (user, group, role, policy, MFA), AWS Organizations, tuân thủ, các dịch vụ bảo mật như AWS Shield, AWS WAF, Amazon GuardDuty | 02/10/2026 | 02/10/2026 | https://skillbuilder.aws/learn/94T2BEN85A/aws-cloud-practitioner-essentials/ |
| 7 | - Module mẫu: lỗi API theo chuẩn RFC 9457, handler cho `GET /api/v1/products/{id}`, khai báo endpoint trong OpenAPI<br>- Tạo app AWS CDK: bảng DynamoDB, Lambda (Node.js 24, arm64), HTTP API, CloudWatch Logs; kiểm luật bảo mật bằng cdk-nag; chạy `cdk diff` lên sandbox | 03/10/2026 | 03/10/2026 | https://www.rfc-editor.org/rfc/rfc9457<br>https://docs.aws.amazon.com/cdk/v2/guide/home.html<br>https://github.com/cdklabs/cdk-nag |
| CN | - Deploy module mẫu lên sandbox `shop-sbx-an`, nạp 2 sản phẩm mẫu, gọi thử API: 200 với hàng đang bán, 404 với hàng ngừng bán<br>- Sửa module mẫu theo hợp đồng OpenAPI bản 0 và thiết kế bảng của Hoàng (khoá `productId`, luật BR-04)<br>- Xử lý lỗi `ConflictException` khi đổi tên biến trong route API Gateway; ghi lại vào tài liệu hạ tầng<br>- Merge module mẫu vào `develop` (PR #22); bật deploy tự động lên môi trường dev<br>- Sửa trust policy OIDC theo định dạng `sub` mới có mã số bất biến của GitHub (tìm nguyên nhân qua CloudTrail, PR #25) → deploy dev thành công<br>- Quy ước số trong tên nhánh là số issue, thêm bước CI tự kiểm (PR #27, chờ merge) | 04/10/2026 | 04/10/2026 | https://github.com/hhieuann/shop-ai/pull/22<br>https://github.com/hhieuann/shop-ai/pull/25<br>https://github.com/hhieuann/shop-ai/pull/27 |

### Kết quả đạt được tuần 3:

* Nhóm có kế hoạch đã chốt, phân vai rõ (An: nền tảng và DevOps; Hoàng: nghiệp vụ bán hàng; Nhân: dữ liệu, gợi ý, bảo mật) và lịch 8 tuần với các mốc v0.1.0 (19/10), v0.2.0 (02/11), v1.0.0 (23/11).
* Repo có quy trình như doanh nghiệp: nhánh theo git flow, commit theo Conventional Commits, mọi PR phải qua CI (lint, type, test, build, quét bí mật và thư viện), có bảng công việc.
* Tránh rủi ro hoá đơn: thay Amazon Personalize bằng mô hình tự xây theo góp ý của mentor; budget cảnh báo theo nhiều ngưỡng.
* Hiểu và dựng được CI/CD không cần access key: GitHub Actions đổi token OIDC lấy quyền IAM role ngắn hạn, chỉ đúng repo và đúng environment mới dùng được.
* Module mẫu chạy thật trên AWS theo kiến trúc phân lớp (domain, application, ports, infra), có 16 unit test, 3 integration test và 14 test hạ tầng; lỗi trả về theo chuẩn RFC 9457.
* Toàn bộ hạ tầng viết bằng CDK: dựng hoặc xoá cả môi trường bằng một lệnh; mỗi lần merge vào `develop` thì môi trường dev tự cập nhật.
* Rút kinh nghiệm từ lỗi thật: đổi tên biến trong route làm CloudFormation tạo route mới trước khi xoá route cũ; GitHub đổi định dạng `sub` của OIDC với repo mới; AWS CLI trên Windows không đọc được file có ký tự tiếng Việt.
* Tự học khoá AWS Cloud Practitioner Essentials trên Skill Builder, module 7–9: nắm kiến thức nền về dịch vụ AWS để áp dụng vào lab và dự án.
