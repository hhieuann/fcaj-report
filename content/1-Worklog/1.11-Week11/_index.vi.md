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
| 2 | - Làm việc tại văn phòng <br> - Rà tiến độ nhóm: PR, issue, release, báo cáo <br> - Nhắc Hoàng (5.4), Nhân (ảnh 5.5) hạn 24/11 <br> - **Nghiệm thu `v1.0.0` trên staging:** <br>&emsp; + N2 admin huỷ đơn, đối chiếu tồn kho trên DynamoDB <br>&emsp; + N3 quên mật khẩu, kiểm log trigger thu hồi phiên <br>&emsp; + N6 đăng nhập khi máy còn phiên cũ <br> - Ghi kết quả nghiệm thu, thêm giới hạn SES sandbox (#523) <br> - Duyệt, merge #515 (lỗi 2 tab, đóng #508) <br> - Merge #521 (test MFA chập chờn) và 3 PR Dependabot <br> - Sửa #519 (cdk-nag L1 khi `aws-cdk-lib` có runtime mới), merge <br> - **Phát hành `v1.0.0`:** <br>&emsp; + #523 vào `release/v1.0.0`, deploy staging xanh <br>&emsp; + #524 `release/v1.0.0` → `main` <br>&emsp; + Gắn tag `v1.0.0`, Deploy prod chờ duyệt | 23/11/2026 | 23/11/2026 | [TransactWriteItems](https://docs.aws.amazon.com/amazondynamodb/latest/APIReference/API_TransactWriteItems.html) <br> [AdminUserGlobalSignOut](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_AdminUserGlobalSignOut.html) <br> [Amplify token refresh](https://docs.amplify.aws/react/build-a-backend/auth/connect-your-frontend/manage-user-sessions/) |

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
  * Gắn tag `v1.0.0` trên `main` và đẩy lên; workflow Deploy prod chạy và chờ một người khác An duyệt environment `production`. Deploy xong: kiểm smoke test, WAF chặn bằng core rule set (đóng #457), đặt một đơn test trên prod rồi huỷ.

### Kết quả đạt được tuần 11:

* Nghiệm thu `v1.0.0` trên staging đạt cả 8 ca, kết quả và giới hạn SES sandbox đã ghi vào checklist (#523).
* `v1.0.0` đã vào `main` và có tag, đang chờ duyệt deploy prod.
* Lỗi lệch tài khoản giữa 2 tab đã sửa trên `develop` (#515), cùng test MFA hết chập chờn (#521) và 3 bản cập nhật thư viện.
* Còn lại trong tuần: duyệt deploy prod và kiểm WAF, Blog 3, các phần báo cáo còn trống trước khoảng 29/11.
