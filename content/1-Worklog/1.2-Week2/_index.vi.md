---
title: "Worklog Tuần 2"
date: 2026-09-14
weight: 2
chapter: false
pre: " <b> 1.2. </b> "
---

### Mục tiêu tuần 2:

* Thực hành VPC: nền mạng cho mọi lab sau và cho bài workshop.
* Chuẩn bị các lab Module 1 tiếp theo: EC2, IAM Role cho EC2, S3.
* Chọn lộ trình lab phù hợp định hướng fullstack developer.
* Bắt đầu tìm chủ đề workshop cho nhóm.

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
|-----|-----------|--------------|-----------------|----------------|
| 2 | [bạn điền] | 21/09/2026 | | |
| 3 | - **Thực hành** lab 000003 VPC ở region Singapore (ap-southeast-1):<br>  + Tạo VPC 10.10.0.0/16 với 4 subnet public/private trên 2 Availability Zone, Internet Gateway, Route Table cho subnet public<br>  + Tạo 3 Security Group; bật VPC Flow Logs kèm IAM role ghi log<br>  + Tạo key pair, chạy EC2 Public và EC2 Private; SSH vào EC2 Public rồi từ đó SSH sang EC2 Private (bastion)<br>  + Dọn dẹp toàn bộ tài nguyên theo đúng thứ tự<br>- Tổng hợp lại cả lab để ôn, gồm các phần chưa thực hành: NAT Gateway, Reachability Analyzer, EC2 Instance Connect Endpoint, Session Manager, CloudWatch alarm<br>- Lọc catalog 127 lab theo định hướng fullstack, chọn 25 lab cho 12 tuần<br>- Nắm quy định worklog hằng ngày và mẫu worklog của chương trình<br>- Bắt đầu tìm chủ đề workshop cho nhóm | 22/09/2026 | 22/09/2026 | https://000003.awsstudygroup.com<br>https://cloudjourney.awsstudygroup.com<br>https://workshop-sample.awsfcaj.com/1-worklog/ |
| 4 | - Đọc trước lý thuyết và các bước của lab 000048 (IAM Role cho EC2) và lab 000057 (website tĩnh trên S3); nắm các lệnh cần đổi khi chạy trên Amazon Linux 2023<br>- Tổng hợp và đọc lại lab 000004 Compute Essentials with EC2 (9 chương): Security Group, EBS, snapshot, AMI, ứng dụng Node.js + MariaDB trên một máy | 23/09/2026 | 23/09/2026 | https://000048.awsstudygroup.com<br>https://000057.awsstudygroup.com<br>https://000004.awsstudygroup.com |
| 5 | [bạn điền] | 24/09/2026 | | |
| 6 | [bạn điền] | 25/09/2026 | | |

### Kết quả đạt được tuần 2:

* Dựng được VPC theo kiến trúc chuẩn: subnet public và private tách bạch trên 2 AZ, Internet Gateway và route table cho subnet public.
* Hiểu route table mới quyết định subnet là public hay private, không phải tên subnet.
* Áp dụng least privilege trong Security Group: máy private chỉ nhận SSH từ Security Group của máy public, không mở ra Internet.
* Vào được máy private qua bastion; nắm thêm qua tài liệu hai cách an toàn hơn là EC2 Instance Connect Endpoint và Session Manager (không cần SSH key, không mở cổng 22).
* Bật VPC Flow Logs để ghi lại lưu lượng mạng trong VPC.
* Xử lý được lỗi thực tế: quyền file `.pem` trên Windows (`icacls`), SSH lồng SSH không nhận "yes" (`StrictHostKeyChecking=accept-new`).
* Dọn sạch tài nguyên sau lab, không để lại chi phí.
* Có lộ trình 25 lab theo hướng fullstack cho 12 tuần OJT.
