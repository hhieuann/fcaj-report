---
title: "Các bài blogs đã đăng"
date: 2026-09-14
weight: 3
chapter: false
pre: " <b> 3. </b> "
---

Các bài đăng trên group Facebook **AWS Study Group** (tối thiểu 3 bài).

### Blog 1: GitHub Actions vào AWS bằng OIDC

**Tiêu đề:** 🔐 GitHub Actions vào AWS bằng OIDC: hiểu cách cấp quyền mà không lưu access key dài hạn

**Nội dung:** Vì sao workflow CI/CD cần danh tính trên AWS, rủi ro của access key dài hạn lưu trong GitHub Secrets, và cách OIDC cho workflow nhận credentials tạm thời: GitHub cấp token, AWS STS kiểm tra trust policy (repo, nhánh, environment) rồi trả credentials của role. Giải thích `aud`, `sub`, cách khai báo provider và role, và lỗi hay gặp khi `sub` không khớp.

**Link:** https://www.facebook.com/groups/awsstudygroupfcj/permalink/2325503738214585/

### Blog 2: Diễn tập khôi phục DynamoDB bằng PITR

**Tiêu đề:** 🛟 Bật PITR rồi, bạn đã thử khôi phục bảng DynamoDB chưa?

**Nội dung:** RPO và RTO; Point-in-Time Recovery luôn khôi phục ra bảng mới; số liệu thật từ lần diễn tập ngày 09/11/2026 trên staging (23 = 23 item, bảng nhỏ khoảng 9 KB vẫn mất 3–10 phút, mốc muộn nhất chậm 4 phút 59 giây). Ba điều rút ra: waiter `table-exists` chỉ chờ khoảng 8 phút; bảng mới không mang theo TTL, PITR, deletion protection và tag; trỏ ứng dụng sang bảng mới vướng quyền IAM và IaC. Nói rõ phần chưa diễn tập (chép dữ liệu về, RTO đầy đủ).

**Link:** https://www.facebook.com/groups/awsstudygroupfcj/posts/2334688377296121/

### Blog 3

Đang chuẩn bị.
