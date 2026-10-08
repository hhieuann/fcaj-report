---
title: "Worklog Tuần 1"
date: 2026-09-14
weight: 1
chapter: false
pre: " <b> 1.1. </b> "
---

### Mục tiêu tuần 1:

* Kết nối với mentor và các thành viên First Cloud AI Journey; nắm nội quy, thang điểm và điều kiện nhận mộc thực tập.
* Thiết lập môi trường thực hành AWS an toàn: tài khoản Free plan, cảnh báo chi phí, MFA, IAM user riêng.
* Bắt đầu Module 1 Explore AWS Services: IAM và lý thuyết VPC.

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
|-----|-----------|--------------|-----------------|----------------|
| 7 | - Đọc email trúng tuyển và toàn bộ nội quy đơn vị thực tập<br>- **Thực hành:**<br>  + Tạo AWS account (Free plan), kích hoạt $100 credit<br>  + Thiết lập 3 AWS Budgets, bật Free Tier alerts<br>  + Bật MFA cho root user (passkey + Authenticator)<br>  + Tạo IAM user `hieuan-admin` (AdministratorAccess + MFA) để dùng hàng ngày | 13/09/2026 | 13/09/2026 | https://hcm-rules.awsfcaj.com<br>https://000001.awsstudygroup.com<br>https://000007.awsstudygroup.com |
| 2 | - Nhận hướng dẫn tuần 1 tự học từ mentor, tham gia nhóm WhatsApp FCAJ<br>- Xác định tài liệu học, lập lộ trình Module 1: IAM → VPC → EC2 → S3 → RDS → CloudWatch → Lambda<br>- **Thực hành:**<br>  + Lab 000002 IAM: tạo IAM Group, IAM User, IAM Role; Switch Role; dọn dẹp<br>  + Hoàn thành 4/5 activity "Earn AWS credits": Lambda, EC2, RDS, Bedrock → nhận $80 credit<br>  + Dọn dẹp tài nguyên sau activity (terminate EC2, xoá RDS, xoá Lambda), rà soát Budgets | 14/09/2026 | 14/09/2026 | https://000002.awsstudygroup.com<br>https://000001.awsstudygroup.com/vi/4-h%C6%B0%E1%BB%9Bng-d%E1%BA%ABn-chi-ti%E1%BA%BFt-5-nhi%E1%BB%87m-v%E1%BB%A5-ki%E1%BA%BFm-ti%E1%BB%81n/ |
| 3 | - Nộp giấy tờ xác minh tài khoản AWS theo yêu cầu "Account On Hold"<br>- Tìm hiểu lý thuyết VPC (lab 000003 chương 1–2):<br>  + VPC, Subnet, Route Table<br>  + Internet Gateway, NAT Gateway<br>  + Security Group và Network ACL<br>  + Kiến trúc lab: VPC /16, 4 subnet /24 trên 2 Availability Zone | 15/09/2026 | 15/09/2026 | https://000003.awsstudygroup.com |
| 4 | - Tự học trên AWS Skill Builder, khoá AWS Cloud Practitioner Essentials, Module 1 - Introduction to the Cloud: điện toán đám mây là gì, lợi ích của AWS Cloud, hạ tầng toàn cầu, mô hình trách nhiệm chung | 16/09/2026 | 16/09/2026 | https://skillbuilder.aws/learn/94T2BEN85A/aws-cloud-practitioner-essentials/ |
| 5 | - Tự học trên AWS Skill Builder, khoá AWS Cloud Practitioner Essentials, Module 2 - Compute in the Cloud: Amazon EC2, các loại instance, cách tính giá, mở rộng với Auto Scaling và Elastic Load Balancing | 17/09/2026 | 17/09/2026 | https://skillbuilder.aws/learn/94T2BEN85A/aws-cloud-practitioner-essentials/ |
| 6 | - Tự học trên AWS Skill Builder, khoá AWS Cloud Practitioner Essentials, Module 3 - Exploring Compute Services: serverless với AWS Lambda, container với Amazon ECS, EKS và AWS Fargate | 18/09/2026 | 18/09/2026 | https://skillbuilder.aws/learn/94T2BEN85A/aws-cloud-practitioner-essentials/ |

### Kết quả đạt được tuần 1:

* Nắm nội quy, thang điểm (Workshop 5 · Thái độ 2 · Chuyên cần 1.5 · Worklog/Blog 1 · Bonus 0.5) và điều kiện nhận mộc; kết nối được với mentor và nhóm.
* Có môi trường AWS an toàn: Free plan $100 credit, 3 budget cảnh báo về email, MFA cho root, IAM user riêng cho công việc hàng ngày — không dùng root.
* Hoàn thành lab IAM: hiểu quan hệ Group / User / Policy / Role và cơ chế Switch Role — cấp quyền tạm thời thay vì gán cứng.
* Nhận thêm $80 credit từ 4 activity; tổng credit $180 cho cả kỳ thực tập.
* Biết dọn dẹp tài nguyên đúng cách sau mỗi lần thực hành: terminate EC2, xoá RDS không giữ snapshot, xoá Lambda, rà soát Budgets.
* Nắm lý thuyết VPC: phân biệt public/private subnet qua route table, vai trò Internet Gateway và NAT Gateway, khác biệt Security Group (stateful, gắn instance) và Network ACL (stateless, gắn subnet).
* Xử lý tình huống tài khoản AWS bị tạm giữ chờ xác minh: nộp giấy tờ đúng hạn, dùng thời gian chờ để học lý thuyết.
* Tự học khoá AWS Cloud Practitioner Essentials trên Skill Builder, module 1–3: nắm kiến thức nền về dịch vụ AWS để áp dụng vào lab và dự án.
