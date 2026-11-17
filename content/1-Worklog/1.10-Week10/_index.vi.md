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
* Review các PR của nhóm cho bản sau v1.0.0 (admin huỷ đơn, quên mật khẩu, trang tài khoản) và viết Blog 3.

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
|-----|-----------|--------------|-----------------|----------------|
| 2 | - Kiểm tra cảnh báo Free Tier của CloudWatch Alarms <br> - WAF prod chuyển core rule set sang chặn (#458) <br> - Chạy thử lab trên sandbox, thêm bước tạo khoá HMAC (#463) <br> - Review PR L9 của Nhân (#464) <br> - **Workshop:** <br>&emsp; + Dàn ý 7 chương, chia việc cho nhóm <br>&emsp; + Ước tính chi phí trên Pricing Calculator <br>&emsp; + Viết chương 5.2, 5.3, 5.6, 5.7 (song ngữ) <br>&emsp; + Dọn thật sandbox để chụp ảnh chương 5.7 | 16/11/2026 | 16/11/2026 | [CloudWatch pricing](https://aws.amazon.com/cloudwatch/pricing/) <br> [Free Tier usage](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/tracking-free-tier-usage.html) <br> [SQS DLQ redrive](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-configure-dead-letter-queue-redrive.html) <br> [Post confirmation trigger](https://docs.aws.amazon.com/cognito/latest/developerguide/user-pool-lambda-post-confirmation.html) <br> [Bản ước tính chi phí](https://calculator.aws/#/estimate?id=0798eb584d82c280a557b96fbf3c8fbe695a6ca0) <br> [cdk destroy](https://docs.aws.amazon.com/cdk/v2/guide/ref-cli-cmd-destroy.html) |
| 3 | - Chốt nghiệp vụ trang tài khoản với Hoàng (đổi username, đổi mật khẩu) <br> - Làm API `PATCH /api/v1/me` đổi username (#471, PR #472) <br> - Thử lỗi trigger Post confirmation (L9) trên dev cùng Nhân <br> - Chốt `shared/auth` cho trang tài khoản <br> - **Review và merge PR:** <br>&emsp; + #476 sửa L9 (Nhân), #484 sửa runbook <br>&emsp; + #485 khung trang `/account` (Hoàng) <br>&emsp; + #486 chương 5.5 Workshop (Nhân) <br> - Đề xuất cách sửa bug đăng xuất #487 <br> - Sửa trình bày worklog tuần 4–10 theo template <br> - Review PR #489 sửa bug #487 (Hoàng) <br> - Viết README cho repo báo cáo | 17/11/2026 | 17/11/2026 | [AdminUpdateUserAttributes](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_AdminUpdateUserAttributes.html) <br> [ListUsers](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_ListUsers.html) <br> [ChangePassword](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_ChangePassword.html) <br> [GlobalSignOut](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_GlobalSignOut.html) <br> [Lambda concurrency](https://docs.aws.amazon.com/lambda/latest/dg/configuration-concurrency.html) |

### Chi tiết công việc:

#### Thứ 2 (16/11/2026)

* **Cảnh báo Free Tier của CloudWatch Alarms:** mail AWS báo đã dùng hơn 85% hạn mức tháng 11 (8,5/10). Đếm được 13 alarm, đều ở `ap-southeast-1` và đều OK (prod 7, staging 3, dev 3). Alarm dùng metric math tính 2 metric nên tổng 20, vượt khoảng 10, tức khoảng $1/tháng trừ vào credit. Quyết định giữ nguyên vì alarm dev và staging bắt lỗi trước prod và là bằng chứng giám sát cho Workshop; muốn bỏ thì phải sửa `monitoring.ts`, không xoá tay.
* **WAF prod chuyển core rule set sang chặn (#458, issue #457):**
  * Log từ lúc deploy rc.1 (12/11 10:20) tới 16/11: 678 request, 0 BLOCK; core rule set đếm 74 request, đều là request thử XSS, 0 đếm nhầm.
  * Đổi `COMMON_RULE_SET_COUNT_ONLY` sang `false`, 132/132 test infra đạt; Nhân duyệt, merge vào `release/v1.0.0` để lên prod cùng `v1.0.0`.
  * Duyệt PR runbook WAF của Nhân (#460).
* **Chạy thử lab trên sandbox:** `cdk destroy` rồi `cdk deploy` lại từ đầu, khoảng 5 phút, nạp 104 sản phẩm. Phát hiện 2 lỗi cho tài liệu:
  * `cdk destroy` để sót log group của các Lambda phụ do CDK tạo.
  * Tài khoản thiếu khoá HMAC `/shop/sbx/events/hmac-key` nên tin `PURCHASE` vào DLQ. Tạo khoá, redrive DLQ, thêm bước tạo khoá vào `infra/README.md` (#463).
* **Review PR L9 của Nhân (#464):** trigger Post confirmation chỉ gọi `AdminUserGlobalSignOut` khi đặt lại mật khẩu, IAM đúng một quyền; approve. Lưu ý trigger chạy sau khi mật khẩu đã đổi, nhờ Nhân thử trường hợp lỗi trên dev.
* **Workshop:**
  * Chốt viết thành lab để người đọc tự deploy shop-ai bằng CDK, cả nhóm viết một bản chung; soạn dàn ý 7 chương, hạn bản nháp khoảng 24/11.
  * Chi phí: nhập 14 dịch vụ vào AWS Pricing Calculator theo cấu hình thật trong code, tổng **$24,86/tháng**; CloudFront nằm trong Free Tier nên chi phí thật khoảng $17,5/tháng, chủ yếu là WAF. Ghi lại 3 giá trị mặc định dễ làm sai số (DynamoDB provisioned, Cognito Advanced security, đơn vị của SNS/EventBridge).
  * Viết chương 5.6 Kiểm thử và vận hành, 5.2 Chuẩn bị, 5.3 Triển khai hạ tầng (song ngữ, đã lên site); kiểm trên sandbox thật: đủ header bảo mật, `/api/v1/products` trả 200, `/api/v1/me` chưa đăng nhập trả 401.
  * Dọn thật sandbox và viết chương 5.7 Dọn dẹp tài nguyên: `cdk destroy` khoảng 5 phút; xoá 5 log group còn sót và khoá HMAC; kiểm tra không còn bảng, bucket, user pool, stack. Chụp ảnh, cắt bỏ phần có account ID.
  * Chuẩn bị sơ đồ kiến trúc: lập danh sách khoảng 35 thành phần, 50 kết nối từ code `infra/lib/`; dùng một bản phác do AI tạo làm tham chiếu bố cục, đối chiếu code sửa 7 điểm; bản nộp sẽ tự vẽ trên draw.io theo nội quy.

#### Thứ 3 (17/11/2026)

* **Trang tài khoản của Hoàng:** đọc đề xuất (đổi username, đăng xuất ở `/account`, đổi mật khẩu). Chốt đổi mật khẩu xong thì **đăng xuất mọi nơi**, vì người đổi mật khẩu thường nghi bị lộ; Hoàng sửa trang quên mật khẩu khi đang đăng nhập; MFA cho khách để sau.
* **API `PATCH /api/v1/me` đổi username (#471, PR #472):**
  * Dùng lại luật username của bước đăng ký; kiểm trùng bằng `ListUsers`, đổi sang đúng tên của mình vẫn trả 200; ghi bằng `AdminUpdateUserAttributes`.
  * Lỗi: 400 `USERNAME_REQUIRED`, 400 `USERNAME_INVALID`, 409 `USERNAME_TAKEN`.
  * Lambda có 2 quyền Cognito chỉ trên đúng ARN user pool của môi trường.
  * Test api 592, web 462, infra 136 đều đạt; CI 9 check đạt; Hoàng duyệt và merge; deploy dev và E2E trên Cognito thật đạt.
  * Giới hạn đã biết: gọi thẳng Cognito vẫn ghi được `preferred_username` và `email`, nhờ Nhân ghi vào threat model.
* **Thử lỗi trigger Post confirmation (L9) trên dev cùng Nhân:** An bật tắt bằng CLI (dev chung tài khoản với prod nên không cấp quyền cho Nhân), Nhân thử trên web.
  * Ca B (chặn quyền `AdminUserGlobalSignOut`): web báo lỗi, nhưng **mật khẩu đã đổi** và máy khác **không bị đăng xuất**; message lỗi lộ account ID và tên role.
  * Ca A (reserved concurrency = 0): kết quả giống ca B; hàm bị throttle 4 lần nhưng Errors = 0, nên alarm phải cộng cả Throttles.
  * Alarm `shop-prd-lambda-throttles` báo ALARM 09:53 rồi về OK 09:58: alarm đếm throttle cả tài khoản, 4 lần đều của hàm dev, prod không bị gì.
  * Gỡ cả 2 ca và kiểm tra lại: hàm `Active`, role chỉ còn policy CDK.
* **Chốt `shared/auth` với Hoàng:** đọc code Amplify 6.21.2 thấy `signOut({ global: true })` nuốt mọi lỗi rồi vẫn xoá phiên máy này; đề xuất tự gọi `GlobalSignOut`, lỗi thì báo kèm nút Thử lại.
* **Review và merge PR:**
  * #476 (Nhân): trigger chỉ ném mã `SESSION_REVOKE_FAILED`, alarm Errors + Throttles chỉ ở prod. Góp 1 chỗ runbook, tự sửa ở #484.
  * #485 (Hoàng): khung trang `/account`, không có gì chặn; ghi 2 điểm cho PR sau.
  * #486 (Nhân): chương 5.5 Workshop, so với code thấy khớp; góp 6 chỗ để Nhân sửa ở PR sau.
* **Bug #487 (đăng xuất ở header khi đang ở trang cần đăng nhập thì sang `/login`):** đề xuất đếm số lần đăng xuất thay cho cờ bật/tắt, vì trạng thái phiên đổi bất đồng bộ.
* **Review PR #489 của Hoàng (sửa bug #487):** làm đúng cách đếm `signOutCount`, bỏ được `isLeaving`/`leavingRef` (góp ý ở #485), `/checkout` dùng chung `useRequireSignIn`, thêm 10 test. Lần CI đầu đỏ 1 test (`headerSignOut_goesHome_on /orders`): trên máy chạy 3 lần đều 8/8, chạy lại job trên CI thì xanh, nên là test chập chờn do ca đầu phải tải route lazy lần đầu, vượt mức chờ 1 giây của `findByRole`. Đăng nhận xét nhờ Hoàng tăng thời gian chờ hoặc import trước route trong `beforeAll`, sửa xong mới duyệt.
* **README cho repo `fcaj-report`:** repo chưa có README nên người vào GitHub không biết link trang. Thêm link trang (VI, EN), bảng link 7 mục, cách cập nhật và xem trước trên máy; kiểm từng link mở được (sửa link Blogs sai tên đường dẫn); đặt link trang vào ô About của repo trên GitHub.
* **Rà tiến độ nhóm và phần gợi ý của Nhân:** từ 10/11 đã merge 27 PR của Hoàng, 7 của An, 4 của Nhân. Pipeline gợi ý chạy đủ mỗi đêm 02:00 ở cả 3 môi trường; dev và staging mô hình hơn baseline (thường mua kèm precision@10 0,074 so với 0,032; dành cho bạn 0,056 so với 0,035), prod mới có 7 sự kiện thật nên gợi ý gần như rỗng, hàng bán chạy lùi về hàng mới nhất. Việc còn lại: chương 5.4 của Hoàng chưa bắt đầu (hạn nháp 24/11), Nhân sửa 5.5 và thêm ảnh, An viết 5.1, Proposal, Events; nhánh `release/v1.0.0` sau `develop` 112 commit, cần chốt bản `v1.0.0` gồm những gì.
* **Sửa trình bày worklog trên site báo cáo:** bảng tuần 10 có ô hơn 6.000 ký tự, mỗi dòng cao khoảng 4.500 px, đọc không được. So với template `fcj-workshop-template`: cùng là file `.md` và cùng bảng 5 cột, khác ở chỗ template ghi mỗi ô vài gạch ngắn.
  * Sửa tuần 10 làm mẫu: ô bảng chỉ còn gạch ngắn, nguồn là link có tên ngắn, chi tiết chuyển xuống mục "Chi tiết công việc" dưới bảng, viết mục Kết quả còn trống.
  * Thêm CSS cho bảng worklog (`layouts/partials/custom-header.html`): căn trên, cột Công việc rộng hơn, link không bị cắt giữa chữ; ẩn nút copy cạnh code trong dòng, giữ nút copy cho khối lệnh của Workshop.
  * Áp dụng cho tuần 4–9 (giao 2 agent chạy song song, tự kiểm lại): ô dài nhất giảm từ 1.000–4.900 xuống dưới 710 ký tự, số link trước sau bằng nhau; viết mục Kết quả còn trống cho tuần 5, 6 từ phần chi tiết đã có.
  * Chuyển thư mục worklog nháp cũ `D:\AWS\worklog` (tuần 1–4) vào `archive/worklog-nhap-cu` để khỏi nhầm với worklog thật trên site.

### Kết quả đạt được tuần 10:

* WAF prod đã chặn bằng core rule set, sẵn sàng lên `v1.0.0`.
* Workshop có 4 chương đã lên site (5.2, 5.3, 5.6, 5.7), kèm bản ước tính chi phí $24,86/tháng; chương 5.5 của Nhân đã có bản nháp trong repo shop-ai.
* Tài liệu lab được sửa theo lần chạy thử thật: bước tạo khoá HMAC, bước xoá log group còn sót.
* API đổi username chạy trên dev, E2E đạt.
* L9 được thử với lỗi thật: phát hiện và sửa lỗi lộ thông tin trong message, thêm alarm cho trigger.
* Còn lại trong tuần: chương 5.1 (chờ sơ đồ tự vẽ), Proposal, Events, tập demo trước `v1.0.0`.
