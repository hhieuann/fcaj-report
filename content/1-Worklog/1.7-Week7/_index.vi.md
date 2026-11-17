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
| 2 | - **Rà PR merge cuối tuần chưa có người duyệt:** <br>&emsp; + #263 hạ tầng worker, #265 email SES của worker <br>&emsp; + #267 sửa E2E đặt hàng, #269 job gợi ý mỗi đêm <br> - Duyệt và merge 4 PR Dependabot <br> - Chuẩn bị Blog 1 về deploy bằng OIDC (chưa đăng) <br> - Chốt review OIDC (#180), sửa ADR-0013 (PR #278) <br> - Sửa 2 alarm báo nhầm trên staging (PR #280) <br> - Soạn checklist nghiệm thu v0.2.0, chốt phạm vi (#282, #286) <br> - Review lại #263, #284 của Hoàng (quyền gửi thư SES) <br> - Kiểm worker trên dev bằng AWS CLI | 26/10/2026 | 26/10/2026 | [SES access control](https://docs.aws.amazon.com/ses/latest/dg/control-user-access.html) <br> [CloudWatch metric math](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/using-metric-math.html) <br> [Metric filter syntax](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/FilterAndPatternSyntax.html) <br> [GitHub Actions OIDC](https://docs.github.com/en/actions/reference/security/oidc) <br> [AssumeRoleWithWebIdentity](https://docs.aws.amazon.com/STS/latest/APIReference/API_AssumeRoleWithWebIdentity.html) <br> [Lambda SQS scaling](https://docs.aws.amazon.com/lambda/latest/dg/services-sqs-scaling.html) |
| 3 | - Kiểm lần chạy thật đầu tiên của job gợi ý trên dev <br> - **Widget gợi ý:** <br>&emsp; + Trang chủ (PR #289) <br>&emsp; + "Thường mua kèm" ở trang chi tiết (PR #292) <br>&emsp; + Kiểm widget trang chủ trên dev bằng log API Gateway <br> - Review và approve PR #294 của Nhân (log JSON) <br> - Duyệt kế hoạch quét OWASP ZAP baseline <br> - Soát Blog 1 trước khi đăng <br> - Đọc kết quả ZAP, tạo issue #299, merge PR #301 <br> - Sửa lỗi cache gợi ý khi đăng xuất (PR #303) | 27/10/2026 | 27/10/2026 | [TanStack dependent queries](https://tanstack.com/query/latest/docs/framework/react/guides/dependent-queries) <br> [HTTP API logging](https://docs.aws.amazon.com/apigateway/latest/developerguide/http-api-logging.html) <br> [ZAP baseline scan](https://www.zaproxy.org/docs/docker/baseline-scan/) <br> [AWS penetration testing](https://aws.amazon.com/security/penetration-testing/) <br> [Response headers policies](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/understanding-response-headers-policies.html) <br> [EventBridge Scheduler](https://docs.aws.amazon.com/scheduler/latest/UserGuide/what-is-scheduler.html) |
| 4 | - Làm việc tại văn phòng cùng Hoàng <br> - Rà tiến độ v0.2.0, chạy trước mục 1 checklist <br> - Nghiệm thu O7 trên dev (thư SES) <br> - Chia việc bảo mật với Nhân (WAF, C1) <br> - Giới hạn `ListUsers` của Lambda pre sign-up theo tag `env` | 28/10/2026 | 28/10/2026 | [Cognito actions and resources](https://docs.aws.amazon.com/service-authorization/latest/reference/list_cognito-idp.html) <br> [IAM tags](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_tags.html) <br> [SES sending activity](https://docs.aws.amazon.com/ses/latest/dg/monitor-sending-activity.html) |
| 5 | - Merge PR #305 (pre sign-up chỉ đọc pool cùng `env`), kiểm dev <br> - Đối chiếu đề xuất WAF với gói CloudFront flat-rate <br> - Chốt WAF trả theo dùng, chỉ ở prod <br> - Kiểm CDK bootstrap ở us-east-1 cho stack WAF <br> - Chốt phạm vi cuối v0.2.0: dời R4 (#306, PR #308) | 29/10/2026 | 29/10/2026 | [CloudFront flat-rate plans](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/flat-rate-pricing-plan.html) <br> [IAM Policy Simulator](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_testing-policies.html) <br> [CDK bootstrapping](https://docs.aws.amazon.com/cdk/v2/guide/bootstrapping.html) |

### Chi tiết công việc:

#### Thứ 2 (26/10/2026)

* **Rà các PR nhóm merge cuối tuần chưa có người duyệt:** hạ tầng worker xác nhận đơn (#263), email SES của worker (#265), sửa E2E đặt hàng (#267), job gợi ý mỗi đêm của Nhân (#269).
  * Worker có quyền IAM hẹp; chỉ worker được gửi tin lên topic `order-confirmed`; giới hạn 2 lần chạy cùng lúc cho hợp mức 1 thư/giây của SES sandbox.
  * Code email đúng các điểm đã chốt nhưng chưa bật, vì còn chờ PR quyền SES, Cognito.
  * Tìm nguyên nhân deploy đỏ của #265: E2E chờ đơn `PENDING` trong khi worker đã xác nhận; đã được #267 sửa.
* **Duyệt và merge 4 PR Dependabot:** 17 gói npm (gồm AWS SDK, CDK, vite), ruff, uv-build, action setup-uv. Deploy dev cuối cùng xanh; kiểm lại `uv.lock` và `pnpm-lock.yaml` vẫn khớp.
* **Chuẩn bị Blog 1 về deploy bằng OIDC (chưa đăng):**
  * Đối chiếu từng chi tiết kỹ thuật với template `github-oidc.yaml`, workflow, PR #25, lượt chạy đo thời gian, quy tắc GitHub Environments và tài liệu GitHub về subject bất biến.
  * Cập nhật số liệu: 118 lượt deploy thành công trên `develop`.
  * Sửa phần sự cố cho đúng những gì còn nhớ và đối chiếu được.
  * Sơ đồ minh họa làm với công cụ AI hỗ trợ, có ghi rõ trong bài.
* **Chốt review OIDC (#180) theo hướng chấp nhận rủi ro:**
  * Kiểm lại 5 lớp chặn trên GitHub: environment chỉ nhận đúng nhánh/tag; `production` cần 1 trong 3 người duyệt; chặn tự duyệt; admin không được bỏ qua; chỉ admin tạo tag `v*`.
  * Sửa ADR-0013 cho đúng việc role deploy vẫn dùng `cfn-exec-role` có AdministratorAccess; cập nhật threat model C6, E9 (PR #278, chờ duyệt).
* **Sửa 2 alarm báo nhầm trên staging ngày 21/10 (PR #280, chờ duyệt):**
  * `api-latency-p95` dùng metric math, chỉ tính chu kỳ có từ 20 request.
  * `presignup-errors` trừ lần từ chối đăng ký (đếm bằng metric filter trên log Lambda), vẫn báo lỗi thật như timeout.
* **Checklist nghiệm thu v0.2.0 (PR #282):**
  * Soạn theo mẫu v0.1.0: rà phạm vi sprint 2; bảng test đặt hàng, gợi ý, bảo mật chia cho người không viết phần đó.
  * Phát hiện 4 phần còn hở: email SES chưa bật; web chưa gửi sự kiện và chưa có widget gợi ý; chưa có nút huỷ đơn; admin và WAF chưa bắt đầu. Gửi nhóm chốt trước 30/10.
  * Nhóm chốt phạm vi v0.2.0: admin và WAF dời sang `v1.0.0-rc.1`, An làm widget gợi ý; cập nhật checklist (PR #286).
  * Sửa nhận định sai của mình về nút huỷ đơn (đã có sẵn trên web).
* **Review lại 2 PR Hoàng merge khi chưa có người duyệt:** hạ tầng worker (#263) và quyền gửi thư (#284). Đối chiếu tài liệu SES về IAM khi còn sandbox, chấp nhận `ses:SendEmail` trên `identity/*` kèm điều kiện `ses:FromAddress`.
* **Kiểm worker trên dev bằng AWS CLI:** đủ biến môi trường; log chỉ có cảnh báo `not_verified` cho đơn E2E (không `AccessDenied`, không lộ email); hàng đợi đơn và DLQ trống.

#### Thứ 3 (27/10/2026)

* **Lần chạy thật đầu tiên của job gợi ý mỗi đêm trên dev (Nhân nhờ kiểm):**
  * Lịch 02:00 chạy 1 lần, 0 lỗi, 4 giây; không có `AccessDeniedException`.
  * Con trỏ `ACTIVE#` của 3 loại gợi ý đã chuyển sang bản mới; hàng bán chạy theo `PURCHASES_30D`.
  * Gọi thử API trên dev: hàng bán chạy và "thường mua kèm" có kết quả hợp lý.
  * Hỏi lại Nhân việc trộn sự kiện giả lập trên prod và log chưa ở dạng JSON.
* **Widget gợi ý trên trang chủ (PR #289):**
  * Chưa đăng nhập hiện "Đang được mua nhiều", đã đăng nhập hiện "Dành cho bạn"; lỗi hoặc rỗng thì ẩn khối.
  * Chờ biết trạng thái đăng nhập rồi mới gọi API để tiêu đề không nháy.
  * 5 test mới, chạy thử với API dev bằng Vite proxy; đã merge và deploy dev.
* **Widget "Thường mua kèm" cuối trang chi tiết sản phẩm (PR #292):** loại các món đã có trong giỏ bằng tham số `exclude`, giỏ đổi thì tự lấy lại; không có gợi ý thì không hiện khối. Ghi chú của Nhân về dữ liệu giả lập vào release note v0.2.0.
* **Kiểm widget trang chủ trên dev sau khi đăng nhập (log truy cập API Gateway):** `/recs/for-you` trả 200 (266–414 ms); tài khoản chưa có lịch sử nhận hàng bán chạy đúng thiết kế; widget đạt trên dev. Ghi nhận `/recs/bestsellers` mất khoảng 1,5 giây khi Lambda khởi động lạnh, để tối ưu sau v0.2.0.
* **Review và approve PR #294 của Nhân:** job gợi ý chỉ in 1 dòng JSON để lọc bằng Logs Insights; góp ý đưa số đếm của bước kiểm tra dữ liệu vào dòng tổng kết.
* **Duyệt kế hoạch quét OWASP ZAP baseline (chỉ quét thụ động) vào web dev cho threat model C3, L4:** chỉ dev, không AJAX spider, không chạy lúc đang deploy hay ngày tách release.
* **Soát Blog 1 trước khi đăng:** câu đã sửa; bài và sơ đồ không lộ account ID; còn phải che account ID trong các ảnh chụp màn hình.
* **Kết quả ZAP baseline của Nhân (0 FAIL, 12 WARN, 56 PASS), tự kiểm header của web dev:**
  * Cảnh báo "Server lộ phiên bản" thực ra chỉ là `Server: AmazonS3`.
  * Cảnh báo SRI do link Google Fonts.
  * Không nên bật COEP vì sẽ chặn font.
  * Tạo issue #299 cho response headers policy của CloudFront (L4), làm sau v0.2.0, ghi rõ CSP phải mở cho Cognito và Google Fonts.
* **Review, approve và merge PR #301 của Nhân:** threat model L4 ghi đúng mức các cảnh báo ZAP; SRI và COEP chuyển sang chấp nhận có lý do.
* **Sửa lỗi Hoàng phát hiện khi review #289 (PR #303):** đăng xuất chưa xoá cache gợi ý nên người đăng nhập sau trên cùng máy thoáng thấy "Dành cho bạn" của người trước. Thêm xoá cache `recs` khi đăng xuất, viết test đỏ trước rồi mới sửa; Hoàng duyệt, đã deploy dev.

#### Thứ 4 (28/10/2026)

* **Làm việc tại văn phòng cùng Hoàng.**
* **Rà tiến độ v0.2.0, chạy trước mục 1 checklist:** deploy dev xanh; không còn cảnh báo bảo mật (Dependabot, CodeQL, secret scanning đều 0); chi phí tháng 10 theo tag `env` gần như bằng 0.
* **Nghiệm thu O7 trên dev:** đọc log worker và thống kê SES (5 thư gửi, 0 bị trả về); Hoàng xác nhận đã nhận đủ thư xác nhận và thư huỷ đơn.
* **Chia việc bảo mật với Nhân:** Nhân làm WAF trọn gói, An làm C1 (chặn gọi thẳng `execute-api`). Lưu ý chi phí WAF khi chưa dùng gói CloudFront Free, và WebACL phải ở us-east-1.
* **Quyền `ListUsers` của Lambda pre sign-up (từ danh sách lỗi được review):**
  * Lambda có quyền `ListUsers` trên mọi user pool, kể cả prod vì chung tài khoản.
  * Đối chiếu Service Authorization Reference (resource `userpool` hỗ trợ `aws:ResourceTag`), thêm điều kiện tag `env`.
  * Kiểm tag trên 4 user pool thật; viết test đỏ trước rồi mới sửa; CI xanh, chờ Nhân duyệt.

#### Thứ 5 (29/10/2026)

* **Merge PR #305 sau khi Hoàng và Nhân cùng duyệt:** Lambda pre sign-up chỉ `ListUsers` trên user pool có tag `env` cùng môi trường. Kiểm sau deploy dev:
  * Đăng ký username mới thành công.
  * Gọi thẳng Lambda bằng sự kiện giả thì trùng username bị từ chối (`USERNAME_TAKEN`).
  * IAM Policy Simulator trên role thật: pool dev cho phép, pool staging và prod bị từ chối.
  * Đồng thời xác nhận metric filter của #280 đếm đúng lần từ chối, alarm không kêu nhầm.
* **Đề xuất WAF của Nhân:** đối chiếu với tài liệu CloudFront flat-rate pricing plans thì tài khoản đang ở AWS Free Tier không được đăng ký gói, nên hướng WAF miễn phí của ADR-0006 không làm được. Chốt WAF trả theo dùng, chỉ gắn ở prod (khoảng $10/tháng, trừ vào credit); Nhân sửa ADR-0006 trong PR WAF.
* **Kiểm CDK bootstrap:** `CDKToolkit` có ở us-east-1 và ap-southeast-1 (phiên bản 32); role deploy GitHub assume được các role ở us-east-1, đủ để đặt stack WAF ở us-east-1.
* **Chốt phạm vi cuối của v0.2.0:** web gửi sự kiện xem hàng và thêm giỏ (R4) dời sang `v1.0.0-rc.1` vì Hoàng chưa kịp, tạo issue #306. Cập nhật checklist (PR #308): dời R4, ghi O7 đạt trên dev.

### Kết quả đạt được tuần 7:

* Gợi ý sản phẩm chạy thật trên dev: job mỗi đêm của Nhân ghi bảng `recs` (lần chạy thật đầu tiên 27/10, không lỗi), API gợi ý trả dữ liệu, 2 widget trên web ("Đang được mua nhiều" / "Dành cho bạn" ở trang chủ, "Thường mua kèm" ở trang chi tiết). Đăng xuất xoá cả dữ liệu gợi ý riêng của người vừa dùng (#303).
* Đặt hàng hoàn chỉnh trên dev: worker xác nhận đơn và gửi thư qua SES; nghiệm thu O7 đạt (Hoàng nhận đủ thư xác nhận và thư huỷ).
* Bảo mật: đóng review OIDC (#180, chấp nhận rủi ro có lớp chặn trên GitHub); sửa 2 alarm báo nhầm (#280); Lambda pre sign-up chỉ đọc user pool cùng môi trường (#305), kiểm bằng IAM Policy Simulator; chốt WAF trả theo dùng, chỉ prod, sau khi tìm ra tài khoản Free Tier không dùng được gói CloudFront Free.
* Chốt phạm vi v0.2.0 với nhóm: admin, WAF, màn hình MFA, web gửi sự kiện dời sang `v1.0.0-rc.1`; checklist nghiệm thu chia người test không phải người viết.
* Chưa đạt: không tách `release/v0.2.0` đúng hạn 30/10, dời sang tuần 8. Blog 1 đã soát xong nội dung nhưng chưa đăng (còn che account ID trong ảnh chụp màn hình).
* Bài học: nhiều PR bị merge khi chưa có người duyệt ở đầu tuần; sau khi nhắc, các PR cuối tuần đều chờ approve rồi mới merge.
