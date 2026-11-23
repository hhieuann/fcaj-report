---
title: "Worklog Tuần 11"
date: 2026-09-14
weight: 11
chapter: false
pre: " <b> 1.11. </b> "
---

### Mục tiêu tuần 11:

* Phát hành `v1.0.0` lên prod: ghi kết quả nghiệm thu, đưa `release/v1.0.0` vào `main`, gắn tag, deploy prod, kiểm WAF.
* Hoàn thiện báo cáo FCAJ trước hạn nộp khoảng 29/11: trang mở đầu chương 5 và 5.1, ghép 5.4 (Hoàng) và 5.5 (Nhân), Proposal, Events, Blog 3, Self-evaluation, Feedback.
* Duyệt, merge sửa lỗi 2 tab (#515) cho bản `v1.0.1`.

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
|-----|-----------|--------------|-----------------|----------------|
| 2 | - Làm việc tại văn phòng <br> - Rà tiến độ nhóm: PR, issue, release, báo cáo <br> - Nhắc Hoàng (5.4), Nhân (ảnh 5.5) hạn 24/11 <br> - **Nghiệm thu `v1.0.0` trên staging:** <br>&emsp; + N2 admin huỷ đơn, đối chiếu tồn kho trên DynamoDB <br>&emsp; + N3 quên mật khẩu, kiểm log trigger thu hồi phiên <br>&emsp; + N6 đăng nhập khi máy còn phiên cũ <br> - Ghi kết quả nghiệm thu, thêm giới hạn SES sandbox (#523) <br> - Duyệt, merge #515 (lỗi 2 tab, đóng #508) <br> - Merge #521 (test MFA chập chờn) và 3 PR Dependabot <br> - Sửa #519 (cdk-nag L1 khi `aws-cdk-lib` có runtime mới), merge <br> - **Phát hành `v1.0.0`:** <br>&emsp; + #523 vào `release/v1.0.0`, deploy staging xanh <br>&emsp; + #524 `release/v1.0.0` → `main` <br>&emsp; + Gắn tag `v1.0.0`, deploy prod xanh, kiểm WAF prod <br> - Soạn dàn ý Blog 3 (L9: đặt lại mật khẩu trên Cognito) <br> - Viết Proposal song ngữ, đưa lên site kèm sơ đồ kiến trúc <br>&emsp; + Kiểm nguồn gốc sơ đồ, ghi rõ AI hỗ trợ dựng bản đầu <br>&emsp; + Dọn thư mục kiến trúc, chuyển bản cũ vào archive <br> - Soạn tin gửi mentor, đối chiếu từng câu với sơ đồ <br>&emsp; + Rút gọn dưới 1.024 ký tự để gửi kèm ảnh trên WhatsApp <br> - Kiểm PR: #526 (chương 5.4) đã merge, đóng #522 <br> - Build lại thư mục `public/` cho khớp bản mới <br> - Viết trang Events: 3 sự kiện, 5 ảnh check-in <br> - Rà báo cáo theo thông báo nộp Workshop (hạn 30/11) <br> - Viết summary `.md` và file sơ đồ `.xml` để nhóm nộp <br> - Viết trang mở đầu chương 5 và chương 5.1 Giới thiệu | 23/11/2026 | 23/11/2026 | [Werner Vogels: key takeaways](https://builder.aws.com/content/3KBSLeKsdkk6uMUPq7d9Xxa5jbS/the-invariable-lessons-of-tech-and-humanity-key-takeaways-from-amazon-cto-werner-vogels) <br> [STAR method](https://en.wikipedia.org/wiki/Situation,_task,_action,_result) <br> [AWS WAF managed rules](https://docs.aws.amazon.com/waf/latest/developerguide/aws-managed-rule-groups-baseline.html) <br> [TransactWriteItems](https://docs.aws.amazon.com/amazondynamodb/latest/APIReference/API_TransactWriteItems.html) <br> [AdminUserGlobalSignOut](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_AdminUserGlobalSignOut.html) <br> [Amplify token refresh](https://docs.amplify.aws/react/build-a-backend/auth/connect-your-frontend/manage-user-sessions/) |

### Chi tiết công việc:

#### Thứ 2 (23/11/2026)

* **Làm việc tại văn phòng.**
* **Rà tiến độ nhóm:** `release/v1.0.0` đã trên staging, chưa có tag; #515 (Hoàng) đã đẩy commit sửa giỏ khách, CI xanh; 4 PR Dependabot (#519 đỏ) để sau tag. Báo cáo: 5.2, 5.3, 5.6, 5.7 đã lên site; còn trống trang mở đầu chương 5, 5.1, Proposal, Events, Self-evaluation, Feedback, Blog 3; 5.4 chưa có trong repo, 5.5 thiếu ảnh. Nhắc Hoàng, Nhân hạn nháp 24/11.
* **N2, admin huỷ đơn `CONFIRMED`:** đặt đơn `01M8369G…` (2 món, 17.080.000₫) bằng tài khoản khách, đơn sang `CONFIRMED` sau 2 giây. Admin (có MFA) huỷ ở `/admin`: hộp hỏi lại ghi đúng mã đơn, số món, tổng tiền; đơn sang `CANCELLED`, khách thấy "Đã huỷ". Tồn kho đọc trên DynamoDB: 10 → 9 → 10 và 30 → 29 → 30. Lần đọc tồn kho đầu bị muộn (đơn đã đặt), đối chiếu lại bằng dữ liệu gốc `products.json` và danh sách đơn chứa 2 món.
* **N3, quên mật khẩu:** đặt lại bằng mã gửi về email, đăng nhập được bằng mật khẩu mới. Log trigger Post confirmation staging 10:24:28 và 10:32:59: "Đã đăng xuất mọi thiết bị sau khi đặt lại mật khẩu", không lỗi. Rút ra: xoá cookie `accessToken` không ép được web làm mới token vì Amplify coi như chưa đăng nhập; muốn thấy `Refresh Token has been revoked` trên trình duyệt thì chờ access token cũ hết hạn (1 giờ). "Máy khác" phải là trình duyệt khác hẳn, vì các cửa sổ ẩn danh Chrome dùng chung cookie.
* **N6:** đạt cả 3 ca: F5 ở `/orders` không sang `/login` (#500); đang là A đăng nhập B sai mật khẩu thì báo lỗi và A bị đăng xuất, đúng mật khẩu thì vào đúng B (#507).
* **#523 vào `release/v1.0.0`:** ghi kết quả N1–N8 (đạt cả 8), thêm "SES còn sandbox ở cả 3 môi trường" vào Giới hạn đã biết, đánh dấu xong các bước cắt lại release. Chờ Hoàng hoặc Nhân duyệt.
* **#515:** commit sửa `8401a92` đúng ý góp ý: tab đang là khách chỉ tải lại, không xoá giỏ khách, khoá gộp và ý định đặt hàng; có test mới, cập nhật `dang-nhap.md`. Duyệt và merge vào `develop` (không ảnh hưởng `release/v1.0.0`), #508 tự đóng; nhờ Hoàng thử trên dev thêm ca 2 tab đều là khách có món trong giỏ.
* **Các PR còn lại vào `develop`:**
  * #521 (Hoàng): test trang xác thực hai lớp chỉ chờ lần vẽ đầu 1 giây nên CI chậm thì đỏ ngẫu nhiên; nới lên 5 giây cho các lần chờ vẽ đầu, không đổi code chạy. Merge.
  * Dependabot #516 (`setup-uv`), #518 (`uv-build`) xanh, merge. #517 (`ruff`) đỏ đúng vì test MFA chập chờn; sau khi Dependabot cập nhật nhánh theo `develop` có #521, chạy lại CI thì xanh, merge. Deploy dev `d62348a` xanh.
* **#519 (Dependabot, 24 thư viện):** job Infra đỏ vì `aws-cdk-lib` 2.277 thêm runtime `nodejs26.x`, `python3.15`, nên luật cdk-nag `AwsSolutions-L1` (runtime phải mới nhất) báo cả 15 Lambda đang dùng `nodejs24.x`, `python3.14`. Chọn ghi lý do chấp nhận L1 cho cả app trong `infra/bin/shop.ts` thay vì nâng runtime sát hạn nộp: Node 24 là LTS, khớp `.nvmrc`; Python 3.14 khớp `data/`; cả hai còn được AWS hỗ trợ; nâng runtime để PR riêng có thử trên dev. Thử trên máy: synth có cdk-nag qua, 137/137 test infra, Prettier, ESLint sạch; CI xanh, merge vào `develop`.
* **Phát hành `v1.0.0`:**
  * #523 (kết quả nghiệm thu) Hoàng duyệt và merge 11:33; deploy staging `06cf3ac` xanh.
  * #524 `release/v1.0.0` → `main`: Hoàng duyệt, merge 11:42 (`2e78553`), giữ nhánh release.
  * Gắn tag `v1.0.0` trên `main` và đẩy lên; workflow Deploy prod chờ duyệt environment `production`, Hoàng duyệt. Mọi bước xanh (OIDC, `cdk deploy shop-prd`, smoke test), GitHub Release `v1.0.0` tạo lúc 11:52.
  * Kiểm WAF prod (`shop-prd-web`, us-east-1): `AWSManagedRulesCommonRuleSet` ở chế độ chặn (OverrideAction `None`, không có luật con bị nới). Gọi thử: `/` và `/api/v1/products` 200; query có `<script>` 403; `../../etc/passwd` 403. #457 đã đóng.
  * Còn trong checklist: đặt một đơn test trên prod rồi huỷ; ghi bài học cuối `docs/releases/v1.0.0.md`. `release/v1.0.0` không có bản sửa nào chưa về `develop`.
* **Dàn ý Blog 3:** chủ đề L9: đặt lại mật khẩu trên Cognito không đăng xuất các thiết bị khác. Dựa trên chuyện thật của nhóm: thử trên dev 11/11 (refresh token cũ vẫn dùng được), sửa bằng trigger Post confirmation gọi `AdminUserGlobalSignOut` (16/11), cố tình làm hỏng trigger (17/11: Cognito đổi mật khẩu trước rồi mới gọi trigger, và trả nguyên lỗi có account ID về trình duyệt), log staging hôm nay. Kèm bảng số liệu để đối chiếu khi viết.
* **Hỏi về chatbot AI:** hệ gợi ý hiện có là phần Nhân làm (pipeline mỗi đêm #202, #250, #275, thí nghiệm đầu độc #290), dùng thuật toán cổ điển (luật kết hợp, item-based CF), không phải mô hình AI kiểu chatbot. Chatbot dùng Amazon Bedrock ước tính 4–5 ngày cho một người, trùng lúc viết báo cáo, chưa rõ Free plan dùng được model nào; chốt ghi vào "hướng mở rộng" của Proposal, làm sau khi nộp nếu còn thời gian.
* **Proposal:** viết song ngữ 9 mục (tóm tắt, vấn đề, giải pháp, kiến trúc, bảo mật, kế hoạch, chi phí, rủi ro, kết quả), dùng số liệu thật: ngày tag (`v0.1.0` 22/10, `v0.2.0` 05/11, rc.1 11/11, `v1.0.0` 23/11), Pricing Calculator $24,86/tháng, Lighthouse 94–99, API p95 108 ms, đặt hàng p95 269 ms, nghiệm thu 8/8. Ghi rõ không dùng Personalize theo góp ý mentor (ADR-0016) và gọi đúng tên thuật toán gợi ý.
  * Sơ đồ nhóm gửi: kiểm file `.drawio` thấy 202/204 ô có ID đặt bằng code, không ô nào có ID draw.io tự sinh, tức khung được sinh bằng AI rồi nhóm sửa. Nhóm xác nhận và vẫn dùng; Proposal ghi rõ "dựng bản đầu với AI hỗ trợ, nhóm đối chiếu từng thành phần, mũi tên, quyền đọc/ghi bảng với code CDK rồi sửa". Nội dung sơ đồ khớp code (7 bảng, quyền R/W, 2 DLQ, giới hạn C1, SES sandbox).
  * Đưa file vào `D:\AWS\architecture`; các sơ đồ cũ, script sinh sơ đồ và ảnh cũ chuyển vào `archive/2026-11-23-ban-truoc-khi-nhom-sua`. Ảnh PNG xuất từ draw.io đặt cạnh trang Proposal; kiểm trên site: ảnh 2540×1809 hiện ở cả 2 bản.
* **Tin gửi mentor:** viết thành đoạn văn theo STAR, đối chiếu từng câu với sơ đồ cuối và sửa 4 chỗ lệch (7 bảng dùng chung thay vì mỗi module một bảng, dẫn theo số bước 1–13, đủ 2 DLQ, admin huỷ đơn hoàn kho và email qua worker); thêm 4 ý sơ đồ có (giới hạn C1, SES sandbox, quy trình deploy, CloudFront Function và cache). Kèm link Proposal và web prod để mentor xem trước. Rút gọn còn khoảng 950 ký tự vì WhatsApp giới hạn chú thích ảnh khoảng 1.024 ký tự: giữ 2 link, 4 nhóm bước 1–5, 6–9, 10–11, 12–13, giới hạn C1 và SES sandbox, câu AI hỗ trợ dựng sơ đồ, kết quả `v1.0.0`.
* **Kiểm PR:** không còn PR mở. #526 (chương 5.4 của Hoàng, 317 dòng mỗi bản) đã merge vào `develop`; đọc nhanh thấy khớp code (8 bước, `Idempotency-Key`, DLQ, hoàn kho, SES sandbox), còn thiếu ảnh (các chỗ 📷), trong đó sơ đồ luồng bán hàng phải tự vẽ. Đóng #522 vì #523 merge vào nhánh release nên GitHub không tự đóng.
* **Thư mục `public/`:** là bản build Hugo trên máy, bị `.gitignore` bỏ qua và site không dùng (GitHub Actions tự build), nên vẫn là bản 17/11 với kiến trúc cũ. Build lại bằng `hugo --cleanDestinationDir`: Proposal mới, ảnh kiến trúc cũ đã xoá, đủ worklog tuần 1–12.
* **Trang Events:** 3 sự kiện tại văn phòng AWS, Bitexco: Kickoff FCAJ Buildrathon 2026 và talkshow cùng CMC Global (26/09), AWS Cloud & AI Day 2026 (29/09), Fireside Chat with Dr. Werner Vogels (02/10). Mỗi sự kiện có thời gian, địa điểm, vai trò, nội dung chính, ảnh, cảm nghĩ và bài học nối với dự án (DLQ, diễn tập PITR, đo p95, review code do AI viết). Phần Werner đối chiếu với bài tổng hợp trên AWS Builder Center và ghi nguồn. 5 ảnh thu nhỏ còn 1600px, bỏ EXIF (vị trí GPS) trước khi đưa lên.
* **Rà báo cáo theo thông báo nộp Workshop (hạn hết 30/11):** phần cá nhân (Worklog, Events, Self-evaluation, Feedback) phải tự viết; phần nhóm (Proposal, Workshop, Blogs) giống nhau ở cả 3 người; đại diện nhóm nộp thêm summary `.md` và sơ đồ `.xml`. Còn thiếu: số điện thoại ở trang đầu, mục lục worklog tuần 5–12, Self-evaluation, Feedback, Blog 3, chương 5.4, 5.5 trên site, video demo. Lên kế hoạch từng ngày tới 29/11.
* **File nộp đại diện nhóm** (`D:\AWS
op-bai`): `shop-ai-summary.md` tiếng Anh, không ảnh, đủ Problem statement, Proposed solution (8 quyết định thiết kế theo ADR), End-to-end workflow (13 bước khớp số trên sơ đồ), Additional (bảo mật, số đo, mốc phát hành, giới hạn đã biết), Cost ($24,86/tháng kèm giả định và đối chiếu hoá đơn thật). `shop-ai-architecture.xml`: file `.drawio` đã ở dạng XML không nén (`mxfile`, 204 ô).
* **Workshop:** trang mở đầu chương 5 (tên lab, tổng quan, ai viết chương nào, mục lục) và chương 5.1 Giới thiệu song ngữ: bài toán, bảng việc làm ở từng chương, sơ đồ kiến trúc và giải thích bước 1–13, bảng 12 dịch vụ AWS với lý do chọn (dẫn ADR), lý do không dùng Personalize, giới hạn đã biết. Đối chiếu mô tả từng chương với nội dung thật của 5.5, 5.6, 5.7 trước khi viết.

### Kết quả đạt được tuần 11:

* Nghiệm thu `v1.0.0` trên staging đạt cả 8 ca, kết quả và giới hạn SES sandbox đã ghi vào checklist (#523).
* **`v1.0.0` đã lên prod** (tag, GitHub Release, smoke test xanh); WAF prod chặn đúng bằng core rule set.
* Proposal song ngữ kèm sơ đồ kiến trúc đã lên site, tin gửi mentor xem trước đã soạn xong.
* Trang Events đủ 3 sự kiện kèm ảnh check-in.
* Workshop có trang mở đầu và chương 5.1; summary `.md` và sơ đồ `.xml` để nhóm nộp đã sẵn sàng.
* Lỗi lệch tài khoản giữa 2 tab đã sửa trên `develop` (#515), cùng test MFA hết chập chờn (#521) và 3 bản cập nhật thư viện.
* Còn lại trong tuần: đơn test trên prod, viết và đăng Blog 3, các phần báo cáo còn trống trước khoảng 29/11.
