---
title: "Các bài blogs đã đăng"
date: 2026-09-14
weight: 3
chapter: false
pre: " <b> 3. </b> "
---

Ba bài blog dùng chung của nhóm shop-ai, đăng trên group Facebook **AWS Study Group** (theo quy định, mỗi nhóm có 3 bài blog dùng chung).

### Blog 1: GitHub Actions vào AWS bằng OIDC

**Người viết:** Nguyễn Hiếu An

**Tiêu đề:** 🔐 GitHub Actions vào AWS bằng OIDC: hiểu cách cấp quyền mà không lưu access key dài hạn

**Nội dung:** Vì sao workflow CI/CD cần danh tính trên AWS, rủi ro của access key dài hạn lưu trong GitHub Secrets, và cách OIDC cho workflow nhận credentials tạm thời: GitHub cấp token, AWS STS kiểm tra trust policy (repo, nhánh, environment) rồi trả credentials của role. Giải thích `aud`, `sub`, cách khai báo provider và role, và lỗi hay gặp khi `sub` không khớp.

**Link:** https://www.facebook.com/groups/awsstudygroupfcj/permalink/2325503738214585/

### Blog 2: Diễn tập khôi phục DynamoDB bằng PITR

**Người viết:** Nguyễn Hiếu An

**Tiêu đề:** 🛟 Bật PITR rồi, bạn đã thử khôi phục bảng DynamoDB chưa?

**Nội dung:** RPO và RTO; Point-in-Time Recovery luôn khôi phục ra bảng mới; số liệu thật từ lần diễn tập ngày 09/11/2026 trên staging (23 = 23 item, bảng nhỏ khoảng 9 KB vẫn mất 3–10 phút, mốc muộn nhất chậm 4 phút 59 giây). Ba điều rút ra: waiter `table-exists` chỉ chờ khoảng 8 phút; bảng mới không mang theo TTL, PITR, deletion protection và tag; trỏ ứng dụng sang bảng mới vướng quyền IAM và IaC. Nói rõ phần chưa diễn tập (chép dữ liệu về, RTO đầy đủ).

**Link:** https://www.facebook.com/groups/awsstudygroupfcj/posts/2334688377296121/

### Blog 3: Amazon Cognito và Lambda trigger cho phần tài khoản

**Người viết:** Nguyễn Minh Hoàng (đăng ngày 18/11/2026)

**Tiêu đề:** 🔑 Đổi mật khẩu rồi mà điện thoại kia vẫn đăng nhập? Kinh nghiệm dùng Amazon Cognito và Lambda trigger

**Nội dung:** Cognito User Pool lo sẵn phần khó của tài khoản (lưu mật khẩu, mã xác minh, khoá khi sai nhiều lần, cấp JWT), còn luật riêng của shop thì phải gắn Lambda trigger. Trigger Pre sign-up kiểm tên trùng bị hết giờ ở lần khởi động lạnh: Cognito chỉ chờ trigger tối đa 5 giây, nên nhóm tăng bộ nhớ lên 512 MB và đặt timeout đúng 5 giây; alarm phải tách lỗi thật khỏi trường hợp từ chối đúng luật. Trigger không chặn được mọi cửa (gọi thẳng API của Cognito vẫn đi qua được). Quên mật khẩu: chống dò tài khoản, không có lệnh chỉ kiểm mã, email mặc định khoảng 50 thư/ngày. Cognito đặt lại mật khẩu nhưng không đăng xuất thiết bị khác, nên nhóm thêm trigger Post confirmation gọi đăng xuất toàn cục; bẫy là trigger chạy sau khi mật khẩu đã đổi, và access token cũ còn dùng được tới khi hết hạn.

**Link:** https://www.facebook.com/groups/awsstudygroupfcj/posts/2342420569856235/
