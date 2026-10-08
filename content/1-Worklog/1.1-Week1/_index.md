---
title: "Week 1 Worklog"
date: 2026-09-14
weight: 1
chapter: false
pre: " <b> 1.1. </b> "
---

### Week 1 Objectives:

* Connect with the mentor and First Cloud AI Journey members; understand the rules, grading scheme and requirements for the internship stamp.
* Set up a safe AWS practice environment: Free plan account, cost alerts, MFA, a dedicated IAM user.
* Start Module 1 Explore AWS Services: IAM and VPC theory.

### Tasks to be carried out this week:

| Day | Task | Start Date | Completion Date | Reference Material |
|-----|------|------------|-----------------|-------------------|
| 7 | - Read the acceptance email and all internship unit regulations<br>- **Practice:**<br>  + Create an AWS account (Free plan), activate $100 credit<br>  + Set up 3 AWS Budgets, enable Free Tier alerts<br>  + Enable MFA for the root user (passkey + Authenticator)<br>  + Create IAM user `hieuan-admin` (AdministratorAccess + MFA) for daily use | 13/09/2026 | 13/09/2026 | https://hcm-rules.awsfcaj.com<br>https://000001.awsstudygroup.com<br>https://000007.awsstudygroup.com |
| 2 | - Received week-1 self-study guidance from the mentor, joined the FCAJ WhatsApp group<br>- Identified learning materials and planned Module 1: IAM → VPC → EC2 → S3 → RDS → CloudWatch → Lambda<br>- **Practice:**<br>  + Lab 000002 IAM: create IAM Group, IAM User, IAM Role; Switch Role; clean up<br>  + Completed 4/5 "Earn AWS credits" activities: Lambda, EC2, RDS, Bedrock → earned $80 credit<br>  + Cleaned up resources after activities (terminate EC2, delete RDS, delete Lambda), reviewed Budgets | 14/09/2026 | 14/09/2026 | https://000002.awsstudygroup.com<br>https://000001.awsstudygroup.com/vi/4-h%C6%B0%E1%BB%9Bng-d%E1%BA%ABn-chi-ti%E1%BA%BFt-5-nhi%E1%BB%87m-v%E1%BB%A5-ki%E1%BA%BFm-ti%E1%BB%81n/ |
| 3 | - Submitted account verification documents as required by the "Account On Hold" notice<br>- Studied VPC theory (lab 000003, chapters 1–2):<br>  + VPC, Subnet, Route Table<br>  + Internet Gateway, NAT Gateway<br>  + Security Group and Network ACL<br>  + Lab architecture: VPC /16, four /24 subnets across 2 Availability Zones | 15/09/2026 | 15/09/2026 | https://000003.awsstudygroup.com |
| 4 | - Self-study on AWS Skill Builder, AWS Cloud Practitioner Essentials, Module 1 - Introduction to the Cloud: what cloud computing is, benefits of the AWS Cloud, global infrastructure, the shared responsibility model | 16/09/2026 | 16/09/2026 | https://skillbuilder.aws/learn/94T2BEN85A/aws-cloud-practitioner-essentials/ |
| 5 | - Self-study on AWS Skill Builder, AWS Cloud Practitioner Essentials, Module 2 - Compute in the Cloud: Amazon EC2, instance types, pricing options, scaling with Auto Scaling and Elastic Load Balancing | 17/09/2026 | 17/09/2026 | https://skillbuilder.aws/learn/94T2BEN85A/aws-cloud-practitioner-essentials/ |
| 6 | - Self-study on AWS Skill Builder, AWS Cloud Practitioner Essentials, Module 3 - Exploring Compute Services: serverless with AWS Lambda, containers with Amazon ECS, EKS and AWS Fargate | 18/09/2026 | 18/09/2026 | https://skillbuilder.aws/learn/94T2BEN85A/aws-cloud-practitioner-essentials/ |

### Week 1 Achievements:

* Understood the regulations, grading scheme (Workshop 5 · Attitude 2 · Attendance 1.5 · Worklog/Blog 1 · Bonus 0.5) and stamp requirements; connected with the mentor and the group.
* Have a safe AWS environment: Free plan with $100 credit, 3 budgets alerting by email, MFA on root, a dedicated IAM user for daily work — root is not used.
* Completed the IAM lab: understood the Group / User / Policy / Role relationship and the Switch Role mechanism — temporary privilege instead of hard-coded permissions.
* Earned an additional $80 credit from 4 activities; total credit $180 for the internship.
* Learned to clean up properly after each practice: terminate EC2, delete RDS without keeping snapshots, delete Lambda, review Budgets.
* Grasped VPC theory: public vs private subnets are determined by the route table, the roles of Internet Gateway and NAT Gateway, the difference between Security Groups (stateful, instance-level) and Network ACLs (stateless, subnet-level).
* Handled an AWS account hold pending verification: submitted documents on time and used the waiting time for theory.
* Self-studied AWS Cloud Practitioner Essentials on Skill Builder, modules 1–3: core AWS service knowledge applied in labs and the project.
