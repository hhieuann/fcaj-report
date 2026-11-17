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
| 2 | - Tách nhánh `release/v0.1.0`, báo lịch mới cho nhóm <br> - Staging deploy lần đầu thành công <br> - Tạo người dùng E2E trên staging, đặt biến và secret <br> - Deploy lại staging: smoke 6/6, E2E 4/4 <br> - Review và merge 4 PR vào `develop` | 19/10/2026 | 19/10/2026 | [Tạo user Cognito](https://docs.aws.amazon.com/cognito/latest/developerguide/how-to-create-user-accounts.html) <br> [gh secret set](https://cli.github.com/manual/gh_secret_set) |
| 3 | - Tự học: bắt đầu AWS Cloud Quest: Cloud Practitioner | 20/10/2026 | 20/10/2026 | [AWS Cloud Quest](https://aws.amazon.com/training/digital/aws-cloud-quest/) |
| 4 | - Rà tiến độ nhóm ngày hạn nghiệm thu <br> - Kiểm 5 lớp chặn trên GitHub cho issue OIDC (#180) <br> - Soát bộ dữ liệu giả lập của Nhân <br> - Review và duyệt PR bucket dữ liệu huấn luyện (#179) <br> - Xử lý cảnh báo thật đầu tiên từ staging (PR #182) <br> - Nghiệm thu giỏ hàng G1–G6 trên staging <br> - Tổng hợp nghiệm thu, cập nhật checklist (PR #196) | 21/10/2026 | 21/10/2026 | [S3 Bucket Key](https://docs.aws.amazon.com/AmazonS3/latest/userguide/bucket-key.html) <br> [GitHub environments](https://docs.github.com/en/actions/managing-workflow-runs-and-deployments/managing-deployments/managing-environments-for-deployment) <br> [Lambda memory](https://docs.aws.amazon.com/lambda/latest/dg/configuration-memory.html) <br> [Cognito Lambda triggers](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-pools-working-with-lambda-triggers.html) |
| 5 | - Tạo khoá HMAC ẩn danh userId cho 3 môi trường <br> - **Phát hành `v0.1.0`:** <br>&emsp; + Sửa commitlint, merge vào `main` (#205, #207) <br>&emsp; + Gắn tag, deploy prod, sửa release note <br>&emsp; + Diễn tập alarm prod, đưa release về `develop` (#209) <br> - Sprint 2: SNS `order-confirmed` và hàng đợi SQS (PR #211) <br> - **API gợi ý:** <br>&emsp; + Hợp đồng OpenAPI 3 route (PR #213) <br>&emsp; + Chốt cách ghép với pipeline của Nhân <br>&emsp; + Bảng `recs` và lõi API (PR #221, #224) <br> - Viết lại README hai ngôn ngữ (PR #215, #217) | 22/10/2026 | 22/10/2026 | [Parameter Store](https://docs.aws.amazon.com/systems-manager/latest/userguide/systems-manager-parameter-store.html) <br> [commitlint config](https://commitlint.js.org/reference/configuration.html) <br> [set-alarm-state](https://docs.aws.amazon.com/cli/latest/reference/cloudwatch/set-alarm-state.html) <br> [git merge conflicts](https://git-scm.com/docs/git-merge#_how_to_resolve_conflicts) <br> [SNS tới SQS](https://docs.aws.amazon.com/sns/latest/dg/sns-sqs-as-subscriber.html) <br> [Raw message delivery](https://docs.aws.amazon.com/sns/latest/dg/sns-large-payload-raw-message-delivery.html) |
| 6 | - Tự học: làm tiếp AWS Cloud Quest: Cloud Practitioner | 23/10/2026 | 23/10/2026 | [AWS Cloud Quest](https://aws.amazon.com/training/digital/aws-cloud-quest/) |
| 7 | - Trả lời câu hỏi thiết kế đặt hàng của Hoàng <br> - Xem lại PR ordering đã merge (#233, #234, #236, #237) <br> - Kiểm PR hàm băm `userId` dùng chung (#225) | 24/10/2026 | 24/10/2026 | [Điều kiện DynamoDB](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/specifying-conditions.html) |
| CN | - Hoàn thành API gợi ý (PR #241) <br> - Kiểm trên máy, CI xanh, Hoàng duyệt <br> - Sửa tên cột mã sản phẩm (PR #246) <br> - Deploy dev, gọi thử 3 route <br> - Chốt worker xác nhận đơn với Hoàng (PR #261) <br> - Xem lại PR đặt hàng (#248, #254) và `recs` (#250) | 25/10/2026 | 25/10/2026 | [DynamoDB Query](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Query.html) <br> [BatchGetItem](https://docs.aws.amazon.com/amazondynamodb/latest/APIReference/API_BatchGetItem.html) <br> [Testcontainers](https://node.testcontainers.org/) <br> [SES production access](https://docs.aws.amazon.com/ses/latest/dg/request-production-access.html) |

### Chi tiết công việc:

#### Thứ 2 (19/10/2026)

* **Tách nhánh `release/v0.1.0` từ `develop`:** trễ 3 ngày so với kế hoạch, báo lại lịch mới cho nhóm: nghiệm thu 19–21/10, tag 22/10.
* **Staging deploy lần đầu thành công:** 2 stack `shop-stg-api`, `shop-stg-web` gồm cả alarm, dashboard; 104 sản phẩm demo; smoke test tự lấy URL từ output.
* **Người dùng E2E trên staging:** tạo trên user pool staging, đặt biến và secret cho environment `staging` trên GitHub; xử lý lỗi dán nhiều lệnh làm mật khẩu bị ghi sai bằng một lệnh tự sinh mật khẩu ngẫu nhiên.
* **Chạy lại deploy staging:** smoke 6/6, E2E API 4/4 file qua; gửi URL staging cho nhóm nghiệm thu.
* **Review và merge vào `develop` sau khi đã tách release:**
  * 3 PR Dependabot (17 bản nhỏ của npm, 2 bản của công cụ Python).
  * PR hợp đồng route sự kiện của Nhân.
  * Kiểm lockfile Python vẫn khớp; 2 lần deploy dev xanh, smoke 6/6, E2E 4/4.

#### Thứ 3 (20/10/2026)

* **Tự học trên AWS Skill Builder:** bắt đầu **AWS Cloud Quest: Cloud Practitioner**, khoá học dạng trò chơi nhập vai, mỗi nhiệm vụ giải một bài toán của thành phố ảo bằng dịch vụ AWS và làm lab trên môi trường AWS thật của Skill Builder; nối tiếp khoá Cloud Practitioner Essentials đã học xong.

#### Thứ 4 (21/10/2026)

* **Tiến độ nhóm ngày hạn nghiệm thu:** rà soát, nhắc nhóm báo kết quả và hỏi tiến độ Sprint 2.
* **Issue review bảo mật OIDC của Nhân (#180):** xác nhận 5 lớp chặn trên GitHub, 4 lớp đạt; phát hiện environment production còn cho admin bỏ qua bước duyệt.
* **Bộ dữ liệu giả lập của Nhân:**
  * Chạy script sinh dữ liệu và đánh giá offline.
  * Phát hiện danh mục mặc định có mã sản phẩm không khớp shop thật (0/104).
  * Chạy lại với danh mục thật (102/102): luật mua kèm vẫn hơn baseline nhưng cách biệt nhỏ hơn số đã công bố.
* **PR bucket dữ liệu huấn luyện của Nhân (#179):** chạy test, synth dev và prod qua cdk-nag; deploy dev tạo bucket thành công, smoke và E2E xanh; duyệt.
* **Cảnh báo thật đầu tiên từ staging:** alarm lỗi Lambda pre sign-up và độ trễ p95.
  * Đọc log theo runbook: Lambda pre sign-up bị cắt ở 3 giây (128 MB quá ít CPU, khởi động lạnh từ 55 ms tới hơn 3 s).
  * Alarm độ trễ là báo giả do khởi động lạnh khi ít người dùng.
  * Sửa trên nhánh release: pre sign-up 512 MB, timeout 5 giây (PR #182); Hoàng và Nhân duyệt, staging deploy lại.
  * Kiểm log khi Hoàng làm lại bước A3: khởi động lạnh còn 817 ms (trước bị cắt ở 3.000 ms), từ chối username trùng đúng.
* **Nghiệm thu phần giỏ hàng G1–G6 trên staging, tất cả đạt:** giỏ khách giữ sau khi tải lại, gộp giỏ khi đăng nhập rồi sang checkout, cộng dồn số lượng, chặn vượt tồn kho, sửa xoá cập nhật tổng tiền, cùng giỏ trên trình duyệt khác. Tắt quyền admin bỏ qua bước duyệt của environment production.
* **Tổng hợp nghiệm thu cả nhóm:**
  * Nhân tìm 2 lỗi: thiếu bảng thông số (đã sửa ở #190), số trên biểu tượng giỏ (chuyển v0.2.0).
  * Hoàng 5/6 bước tài khoản đạt (bật MFA chuyển v0.2.0).
  * Tự chạy 4 kiểm tra kỹ thuật trên staging: cache CloudFront, 401, X-Ray, log lỗi.
  * Cập nhật checklist phát hành theo kết quả nghiệm thu (PR #196).

#### Thứ 5 (22/10/2026)

* **Khoá HMAC ẩn danh userId cho module sự kiện của Nhân ở cả 3 môi trường (dev, staging, prod):** SSM Parameter Store SecureString `/shop/<env>/events/hmac-key`, khoá sinh ngẫu nhiên trên máy, không đưa vào repo hay chat. Kiểm trên dev: `POST /api/v1/events` trả 202, bảng sự kiện ghi `userIdHash` đã băm và có TTL.
* **Phát hành `v0.1.0`:**
  * Mở PR `release/v0.1.0` → `main` (#205); CI đỏ ở commitlint vì khoảng 10 merge commit cũ do GitHub tạo dài hơn 100 ký tự.
  * Tìm nguyên nhân, thử bản sửa trên máy với toàn bộ 100 commit, sửa trên nhánh release (#207: bỏ qua merge commit có đuôi `(#số)`, commit thường vẫn giữ giới hạn); #205 xanh, Hoàng duyệt, merge vào `main`.
  * Gắn tag `v0.1.0`; Hoàng duyệt deploy môi trường production. Prod deploy lần đầu thành công: 2 stack `shop-prd-api`, `shop-prd-web`, smoke test 6/6, GitHub Release tự tạo.
  * Sửa release note: tóm tắt, kết quả nghiệm thu, phần chưa có trong bản này.
* **Cảnh báo prod:** xác nhận email nhận cảnh báo; diễn tập alarm `shop-prd-api-5xx` bằng `set-alarm-state`: nhận email ALARM rồi OK sau khoảng 1,5 phút.
* **Đưa nhánh release về `develop` (#209):** gỡ xung đột ở `api-stack.ts` (giữ hạ tầng sự kiện của Nhân và bản sửa pre sign-up), test infra qua, CI xanh, dev deploy lại.
* **Bắt đầu Sprint 2: SNS topic `order-confirmed` và hàng đợi SQS của module sự kiện (PR #211):**
  * Bắt buộc HTTPS, mã hoá SSE-SQS, nhận tin nguyên bản không bọc lớp SNS.
  * DLQ sau 5 lần lỗi, giữ 14 ngày; queue policy chỉ nhận từ đúng topic (threat model E9).
  * 5 test, synth dev và prod qua cdk-nag. Deploy dev xanh; gửi thử 1 tin lên topic, đọc lại từ hàng đợi đúng JSON gốc rồi xoá.
  * Gửi nhóm cách dùng: Hoàng gửi tin từ `order-processor`, Nhân viết consumer ghi `PURCHASE`.
* **Chuẩn bị API gợi ý:**
  * Đọc lõi pipeline mỗi đêm của Nhân (#202) và thiết kế bảng `recs`; gửi Nhân 5 câu hỏi (ai tạo bảng, ngày pipeline chạy trên dev, cách băm `userId`, nguồn hàng bán chạy, các cột).
  * Viết hợp đồng OpenAPI (PR #213): 3 route `/api/v1/recs/also-bought`, `for-you`, `bestsellers`, tách theo quyền vì JWT authorizer của HTTP API gắn theo từng route; web gửi danh sách hàng trong giỏ để loại, server bỏ hàng hết.
  * Kiểm bằng `openapi-spec-validator`; CI đỏ một lần do test web chập chờn, chạy lại xanh; chờ Hoàng review.
* **README hai bản cho người đọc nước ngoài (PR #215, #217):**
  * `README.md` tiếng Anh, `README.vi.md` tiếng Việt, có nút chuyển ngôn ngữ.
  * Thêm link demo prod, bảng tình trạng dự án, mục dự án thể hiện được gì; sửa các chỗ ghi WAF và pipeline gợi ý như đã xong cho đúng thực tế.
  * Đổi mô tả repo sang tiếng Anh; đổi huy hiệu sang trạng thái deploy dev (PR #217).
* **Chốt với Nhân cách hai phần gợi ý ghép với nhau:**
  * Định dạng bảng `recs`, cách băm `userId`, hàng bán chạy ghi sẵn theo loại.
  * Phát hiện generator sinh dữ liệu tới 30/09 nên hàng bán chạy 30 ngày sẽ rỗng, và prod chưa có đơn thì widget rỗng.
  * Thống nhất cách xếp dự phòng 30 → 90 → 365 ngày → hàng mới nhất; báo động khi pipeline không chạy (chỉ ở prod để không vượt 10 alarm miễn phí).
* **Bảng `recs` (PR #221):** khoá `pk`/`sk`, TTL, on-demand, cho pipeline của Nhân cấp quyền ghi; 5 test, kiểm bảng trên dev.
* **Lõi API gợi ý (PR #224):** luật chọn sản phẩm (giữ thứ hạng, bỏ hàng hết, ngừng bán, hàng đã trong giỏ) và 3 use case thường mua kèm, dành cho bạn, bán chạy, có dự phòng khi thiếu dữ liệu; 12 test, kiểm luật layer.

#### Thứ 6 (23/10/2026)

* **Tự học trên AWS Skill Builder:** làm tiếp **AWS Cloud Quest: Cloud Practitioner**, các nhiệm vụ dựng giải pháp trên môi trường AWS thật của Skill Builder.

#### Thứ 7 (24/10/2026)

* **Câu hỏi thiết kế đặt hàng của Hoàng, chốt hướng làm:**
  * Quyền đọc, ghi bảng của module khác gom vào một lần sửa ADR-0017 do Hoàng viết: ordering đọc `products`, `carts` và chỉ ghi cột `stock`; API gợi ý đọc `products`; pipeline gợi ý được Scan `products` chỉ đọc.
  * Bỏ email admin khỏi topic `order-confirmed`.
  * Alarm mới chỉ tạo ở prod để không vượt 10 alarm miễn phí.
  * Ghi đơn xong mà gửi SQS lỗi thì vẫn trả 201, ghi log và metric.
* **PR ordering Hoàng đã merge (#233, #234, #236, #237):** xem lại, nội dung khớp các điểm đã chốt; nhắc nhóm chờ một người duyệt trước khi merge PR hạ tầng và hợp đồng.
* **PR hàm băm `userId` dùng chung của Nhân (#225):** kiểm đã vào `develop`, để API gợi ý băm giống module sự kiện.

#### CN (25/10/2026)

* **Hoàn thành API gợi ý (PR #241):**
  * Adapter đọc bảng `recs` (giữ con trỏ phiên bản 60 giây trong Lambda, Query tối đa 20 dòng) và bảng `products` (BatchGetItem, gọi lại phần chưa xử lý).
  * Handler kiểm `limit` 1–10, `exclude` tối đa 20 mã, `category`.
  * Lambda và 3 route: thường mua kèm, bán chạy công khai, dành cho bạn cần đăng nhập; IAM chỉ đọc đúng bảng và đúng khoá HMAC.
  * Thêm nguồn `NEWEST` vào hợp đồng khi cửa hàng chưa đủ đơn để xếp hàng bán chạy.
* **Kiểm trên máy:** test đơn vị, test tích hợp với DynamoDB Local trong Docker, `cdk synth` qua cdk-nag, luật layer; CI xanh, Hoàng duyệt.
* **Sai tên cột mã sản phẩm:** API đọc sai tên cột so với điều đã chốt với Nhân; PR được merge trước khi bản sửa lên kịp nên mở PR sửa riêng (#246), báo Nhân đổi cột ở pipeline và ghi 20 dòng mỗi sản phẩm.
* **Deploy dev xanh, gọi thử 3 route trên dev:** trả danh sách rỗng khi pipeline chưa chạy, 400 khi tham số sai, 401 khi gọi dành cho bạn mà chưa đăng nhập.
* **Chốt với Hoàng 3 điểm cho worker xác nhận đơn:**
  * Lấy email khách bằng Cognito `ListUsers` lọc theo `sub`, chỉ đọc thuộc tính email, không lưu, không ghi log.
  * Tạo địa chỉ gửi SES một lần bằng CLI thay vì CDK vì dev, staging, prod chung một tài khoản và một vùng.
  * Giữ SES sandbox ở cả 3 môi trường (200 thư/ngày, 1 thư/giây), lưu ý Hoàng xử lý lỗi gửi quá nhanh.
  * Xác minh địa chỉ gửi, lưu vào secret của repo; sửa `deploy.yml` và `deploy-prod.yml` truyền địa chỉ gửi cho CDK như email cảnh báo, đổi tên bước E2E theo ADR-0018 (PR #261); deploy dev xanh, địa chỉ được che trong log.
* **PR đặt hàng của Hoàng đã merge trước khi duyệt (#248 handler đặt hàng và alarm `OrderEnqueueFailed`, #254 bỏ điều kiện IAM chặn trừ kho):** xem lại, ổn; ghi nhận tài khoản lên 12 alarm, vượt mức miễn phí khoảng 0,2 USD/tháng như nhóm đã chấp nhận.
* **PR định dạng bảng `recs` của Nhân (#250):** kiểm khớp API gợi ý.

### Kết quả đạt được tuần 6:

* Phát hành `v0.1.0`: nghiệm thu trên staging, gắn tag, prod deploy lần đầu thành công (smoke test 6/6), diễn tập alarm prod nhận đủ email ALARM và OK.
* Xử lý cảnh báo thật đầu tiên từ staging theo runbook: pre sign-up lên 512 MB, timeout 5 giây, khởi động lạnh còn 817 ms.
* Có khoá HMAC ở cả 3 môi trường và SNS topic `order-confirmed` với hàng đợi SQS cho luồng sự kiện mua hàng.
* API gợi ý chạy trên dev với 3 route, bảng `recs` và cách xếp dự phòng khi thiếu dữ liệu.
* Chốt với Hoàng thiết kế đặt hàng và worker xác nhận đơn; tài khoản có 12 alarm, vượt mức miễn phí khoảng 0,2 USD/tháng như nhóm đã chấp nhận.
* Bắt đầu tự học AWS Cloud Quest: Cloud Practitioner trên Skill Builder.
