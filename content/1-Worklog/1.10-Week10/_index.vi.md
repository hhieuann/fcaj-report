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
| 3 | - Chốt nghiệp vụ trang tài khoản với Hoàng (đổi username, đổi mật khẩu) <br> - Làm API `PATCH /api/v1/me` đổi username (#471, PR #472) <br> - Thử lỗi trigger Post confirmation (L9) trên dev cùng Nhân <br> - Chốt `shared/auth` cho trang tài khoản <br> - **Review và merge PR:** <br>&emsp; + #476 sửa L9 (Nhân), #484 sửa runbook <br>&emsp; + #485 khung trang `/account` (Hoàng) <br>&emsp; + #486 chương 5.5 Workshop (Nhân) <br> - Đề xuất cách sửa bug đăng xuất #487 <br> - Sửa trình bày worklog tuần 4–10 theo template <br> - Review PR #489 sửa bug #487 (Hoàng) <br> - Viết README cho repo báo cáo <br> - Rà tiến độ nhóm, soạn kế hoạch chốt `v1.0.0` <br> - Bảng trạng thái chương Workshop cho nhóm (#491) <br> - Sửa test chập chờn làm đỏ CI (#492, PR #493) <br> - Dọn issue đã xong còn mở <br> - Review, duyệt và merge PR #496 đổi mật khẩu (Hoàng) <br> - Duyệt và merge PR #500 sửa lỗi tải lại trang bị sang `/login`, mở #501 | 17/11/2026 | 17/11/2026 | [AdminUpdateUserAttributes](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_AdminUpdateUserAttributes.html) <br> [ListUsers](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_ListUsers.html) <br> [ChangePassword](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_ChangePassword.html) <br> [GlobalSignOut](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_GlobalSignOut.html) <br> [Lambda concurrency](https://docs.aws.amazon.com/lambda/latest/dg/configuration-concurrency.html) |
| 4 | - Chốt 9 điểm thiết kế xác thực hai lớp (TOTP) với Hoàng <br> - Đổi cách xác minh khi khách mất thiết bị tạo mã <br> - Nêu việc bắt buộc admin bật MFA (BR-10) <br> - Tìm hiểu kiến trúc chuẩn bị chương 5.1 <br>&emsp; + 3 luồng: xem hàng, đặt hàng, gợi ý mỗi đêm <br> - Rà PR, issue; nhắc Hoàng, Nhân việc tuần này | 18/11/2026 | 18/11/2026 | [Cognito TOTP MFA](https://docs.aws.amazon.com/cognito/latest/developerguide/user-pool-settings-mfa-totp.html) <br> [AdminSetUserMFAPreference](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_AdminSetUserMFAPreference.html) <br> [qrcode](https://www.npmjs.com/package/qrcode) |

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
* **Rà tiến độ nhóm và phần gợi ý của Nhân:** từ 10/11 đã merge 27 PR của Hoàng, 7 của An, 4 của Nhân. Pipeline gợi ý chạy đủ mỗi đêm 02:00 ở cả 3 môi trường; dev và staging mô hình hơn baseline (thường mua kèm precision@10 0,074 so với 0,032; dành cho bạn 0,056 so với 0,035), prod mới có 7 sự kiện thật nên gợi ý gần như rỗng, hàng bán chạy lùi về hàng mới nhất. Việc còn lại: chương 5.4 của Hoàng chưa bắt đầu (hạn nháp 24/11), Nhân sửa 5.5 và thêm ảnh, An viết 5.1, Proposal, Events; nhánh `release/v1.0.0` sau `develop` 112 commit, cần chốt bản `v1.0.0` gồm những gì. Soạn tin gửi nhóm: đề xuất tách lại `release/v1.0.0` từ `develop` (Hoàng xong #479 trước 19/11, tách và deploy staging 20/11, nghiệm thu 20–21/11, tag 23/11), nhắc hạn nháp Workshop 24/11, dữ liệu demo trên prod (đặt vài đơn thật hoặc demo gợi ý trên staging), sửa test chập chờn ở #489, đóng issue trùng #482, #488; nhờ nhóm trả lời mục `v1.0.0` trước 18/11.
* **Bảng trạng thái chương Workshop (issue #490, PR #491):** Hoàng hỏi xem chương nào còn thiếu ở đâu; trạng thái và dàn ý trước chỉ nằm trên máy An. Thêm vào `docs/workshop/README.md`: bảng trang 5 và 5.1–5.7 cùng video demo (người viết, trạng thái, link bản đã xong trên site), dàn ý 5.1 và 5.4, khuôn viết và cách nộp bản nháp, hạn 24/11 và 29/11. Gửi Hoàng dàn ý 5.4 và link các chương mẫu.
* **CI đỏ ở PR #491 (chỉ sửa tài liệu):** đọc log thấy là test `signOutRedirect.test.tsx` của #489: ca đầu chờ nút tài khoản quá 1 giây vì lần render đầu phải nạp code, CI chậm hơn máy. Chạy lại vẫn đỏ, tức là đỏ gần như mỗi lần chứ không thỉnh thoảng; Hoàng đã merge #489 mà chưa sửa nên mọi PR đều bị. Mở issue #492, PR #493 nới thời gian chờ lần đầu lên 5 giây (chỉ sửa test); trên máy 5/5 lần đạt, CI 9 check đạt, merge. Cập nhật nhánh #491 theo `develop`, CI xanh, merge #491. Lần deploy dev lỗi lúc 11:11 là `ECONNRESET` ở E2E giỏ hàng (lỗi mạng tạm), lần deploy sau đạt nên không cần sửa.
* **Dọn issue:** đóng 5 issue đã xong mà còn mở: #452, #428 (xong ở #453, #429 nhưng PR vào nhánh release nên GitHub không tự đóng), #482, #488 (trùng #483, đã sửa ở #484), #494 (trùng #492, đã sửa ở #493). Để mở #457 (WAF chặn, chờ `v1.0.0` lên prod), #479 (trang tài khoản 3/3 của Hoàng), #183 (MFA cho khách, để sau).
* **Review PR #496 của Hoàng (trang tài khoản 3/3, đổi mật khẩu và quên mật khẩu khi đang đăng nhập, #479):** làm đúng các điểm đã chốt: đổi xong đăng xuất mọi nơi rồi sang `/login`; đăng xuất mọi nơi lỗi thì giữ phiên kèm nút Thử lại; quên mật khẩu khi đang đăng nhập không bị đẩy đi, email chỉ đọc; threat model thêm L10; 15 test mới, CI 9 check đạt. Thiếu 1 việc đã thống nhất: trang quên mật khẩu chưa báo riêng khi trigger L9 lỗi (`UserLambdaValidationException`, `UnexpectedLambdaException` có message bắt đầu bằng `PostConfirmation`), lúc đó mật khẩu đã đổi nhưng web báo lỗi chung; đọc code `develop` xác nhận chưa có chỗ nào bắt 2 lỗi này. Đăng nhận xét đề xuất coi như đặt lại xong, sang `/login` với câu báo riêng, có test, cần có trước khi merge. Hoàng thêm trong 2 commit: loại lỗi `resetDone` (chỉ khi message bắt đầu bằng `PostConfirmation`, lỗi Pre sign-up giữ như cũ), hàm `finishReset` dùng chung cho thành công và trigger lỗi, 6 test dùng đúng message thật trên dev, ghi vào `dang-nhap.md`; CI 9 check đạt. Duyệt và merge, #479 đóng. Còn thử trên dev: đổi mật khẩu thật, quên mật khẩu khi đang đăng nhập, câu báo `resetDone` với lỗi trigger thật (chưa kiểm được Amplify có giữ nguyên message). Deploy dev sau khi merge (commit `ca3ecbb`) đạt: `cdk deploy`, smoke test, E2E API; web dev trả 200 cho `/account/password`, `/forgot-password`, bản build có `ChangePasswordPage`.
* **PR #500 của Hoàng (bug #499, tải lại trang cần đăng nhập bị đẩy sang `/login` dù phiên còn):** web nạp Cognito song song với lúc vẽ trang, trước khi nạp xong hỏi token trả `null` nên trang tưởng là khách. Sửa bằng `setAuthReady`: `getAccessToken()`, `getUserProfile()` chờ nạp Cognito xong, nạp lỗi không treo; 4 test, kiểm ngược, tái hiện trên bản build cho Cognito nạp chậm 3 giây; CI 9 check đạt. Duyệt và merge, #499 đóng. Mở issue #501: trang đăng nhập đang coi `UserAlreadyAuthenticatedException` là đăng nhập xong, trên máy dùng chung người sau có thể vào bằng phiên người trước; đề xuất xoá phiên máy này rồi đăng nhập lại, kèm thời gian chờ tối đa cho `setAuthReady`. PR #498 (đổi username ở trang tài khoản) An đã merge lúc 15:52.
* **Sửa trình bày worklog trên site báo cáo:** bảng tuần 10 có ô hơn 6.000 ký tự, mỗi dòng cao khoảng 4.500 px, đọc không được. So với template `fcj-workshop-template`: cùng là file `.md` và cùng bảng 5 cột, khác ở chỗ template ghi mỗi ô vài gạch ngắn.
  * Sửa tuần 10 làm mẫu: ô bảng chỉ còn gạch ngắn, nguồn là link có tên ngắn, chi tiết chuyển xuống mục "Chi tiết công việc" dưới bảng, viết mục Kết quả còn trống.
  * Thêm CSS cho bảng worklog (`layouts/partials/custom-header.html`): căn trên, cột Công việc rộng hơn, link không bị cắt giữa chữ; ẩn nút copy cạnh code trong dòng, giữ nút copy cho khối lệnh của Workshop.
  * Áp dụng cho tuần 4–9 (giao 2 agent chạy song song, tự kiểm lại): ô dài nhất giảm từ 1.000–4.900 xuống dưới 710 ký tự, số link trước sau bằng nhau; viết mục Kết quả còn trống cho tuần 5, 6 từ phần chi tiết đã có.
  * Chuyển thư mục worklog nháp cũ `D:\AWS\worklog` (tuần 1–4) vào `archive/worklog-nhap-cu` để khỏi nhầm với worklog thật trên site.

#### Thứ 4 (18/11/2026)

* **Thiết kế xác thực hai lớp (BR-10) của Hoàng:** chỉ làm TOTP (passkey ghi vào hướng mở rộng vì Cognito chỉ cho passkey thay mật khẩu, phải lên gói Essentials), không đổi hạ tầng (user pool đã `Mfa.OPTIONAL`, TOTP, gói Lite). Đối chiếu với code: bước nhập mã TOTP khi đăng nhập đã có (`LoginPage`, `CodeStep`), CSP đã cho `img-src data:` để hiện mã QR, thư viện `qrcode` 1.5.4 giấy phép MIT. Đồng ý 8/9 điểm (nút, trang, ô mã không tự gửi, ô QR, gợi ý, tắt MFA mức Thấp trong threat model, thư viện QR, tên "Black Magic").
  * **Đổi điểm 6 (khách mất thiết bị tạo mã):** địa chỉ người gửi thư giả được và mã đơn có trong thư xác nhận, nên admin trả lời vào email tài khoản một mã ngẫu nhiên, khách gửi lại đúng mã đó kèm mã đơn mới tắt bằng `AdminSetUserMFAPreference`, rồi `AdminUserGlobalSignOut`.
  * **Thêm:** BR-10 bắt buộc admin bật MFA khi có trang quản trị, trang quản trị đã có mà chưa có gì bắt; đề xuất admin chưa bật MFA vào `/admin` thì chuyển sang trang bật, An bật MFA cho tài khoản admin ở cả 3 môi trường.
  * Ghi thêm vào threat model: Cognito không gửi thư báo khi bật hay tắt MFA.
* **Tìm hiểu kiến trúc để chuẩn bị chương 5.1 và tự vẽ sơ đồ:** đọc lại `infra/lib/` và `docs/business/`, chia hệ thống thành phần nền và 3 luồng.
  * Phần nền: CloudFront + WAF là cửa duy nhất (web tĩnh trên S3 qua OAC, `/api/*` sang HTTP API, cache `/api/v1/products*`); Cognito cấp JWT, HTTP API kiểm bằng JWT authorizer; mỗi module một Lambda ngoài VPC và một bảng DynamoDB (ADR-0005, 0006, 0007, 0012).
  * Luồng 1 xem hàng: `catalog` đọc sản phẩm, `recommendation` chỉ đọc gợi ý tính sẵn trong bảng `recs`; `events` ghi `VIEW`, `ADD_TO_CART` với `userId` băm HMAC, từ chối `PURCHASE` từ trình duyệt.
  * Luồng 2 đặt hàng: một giao dịch DynamoDB (kiểm giá, trừ kho, tối đa 3 đơn mở, ghi `PENDING`) rồi đẩy SQS; worker chuyển `CONFIRMED`, gửi SNS `order-confirmed` để `events` ghi `PURCHASE`, gửi email SES; lỗi quá 3 lần vào DLQ.
  * Luồng 3 gợi ý mỗi đêm: EventBridge Scheduler 02:00 gọi Lambda Python, đọc thẳng bảng `events` và `products` (bucket training-data có tạo nhưng pipeline chưa dùng), tính thường mua kèm, dành cho bạn, bán chạy; mô hình chỉ được bật khi hơn baseline, đổi con trỏ `ACTIVE#<type>`.
  * Phần rìa sơ đồ: GitHub Actions OIDC + CDK, CloudWatch alarm và runbook, 3 môi trường chung một tài khoản (ADR-0008, 0013). Bố cục dự định: người dùng bên trái, CloudFront giữa, backend bên phải, đánh số 3 luồng mỗi luồng một màu.
* **Rà PR và issue, nhắc nhóm:** không còn PR mở (PR cuối #500 merge 17/11); issue mở #183 (MFA, Hoàng chưa mở nhánh), #501, #457 (chờ `v1.0.0` lên prod). Soạn tin nhắc Hoàng làm MFA theo thiết kế đã chốt, #501, chương 5.4; nhắc Nhân sửa 5.5 theo góp ý #486, thêm ảnh, phần chi phí; hạn nháp 24/11.

### Kết quả đạt được tuần 10:

* WAF prod đã chặn bằng core rule set, sẵn sàng lên `v1.0.0`.
* Workshop có 4 chương đã lên site (5.2, 5.3, 5.6, 5.7), kèm bản ước tính chi phí $24,86/tháng; chương 5.5 của Nhân đã có bản nháp trong repo shop-ai.
* Tài liệu lab được sửa theo lần chạy thử thật: bước tạo khoá HMAC, bước xoá log group còn sót.
* API đổi username chạy trên dev, E2E đạt.
* L9 được thử với lỗi thật: phát hiện và sửa lỗi lộ thông tin trong message, thêm alarm cho trigger.
* Còn lại trong tuần: chương 5.1 (chờ sơ đồ tự vẽ), Proposal, Events, tập demo trước `v1.0.0`.
