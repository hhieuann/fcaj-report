---
title: "Week 2 Worklog"
date: 2026-09-14
weight: 2
chapter: false
pre: " <b> 1.2. </b> "
---

### Week 2 Objectives:

* Practice VPC: the network foundation for every later lab and for the workshop.
* Prepare the next Module 1 labs: EC2, IAM Roles for EC2, S3.
* Choose a lab path aligned with the fullstack developer direction.
* Start looking for a workshop topic for the team.

### Tasks to be carried out this week:

| Day | Task | Start Date | Completion Date | Reference Material |
|-----|------|------------|-----------------|-------------------|
| 2 | [fill in] | 21/09/2026 | | |
| 3 | - **Practice** lab 000003 VPC in the Singapore region (ap-southeast-1):<br>  + Create VPC 10.10.0.0/16 with 4 public/private subnets across 2 Availability Zones, an Internet Gateway, a Route Table for the public subnets<br>  + Create 3 Security Groups; enable VPC Flow Logs with an IAM role for log delivery<br>  + Create a key pair, launch EC2 Public and EC2 Private; SSH into EC2 Public, then SSH from it into EC2 Private (bastion)<br>  + Clean up all resources in the correct order<br>- Summarized the whole lab for review, including the parts not practiced: NAT Gateway, Reachability Analyzer, EC2 Instance Connect Endpoint, Session Manager, CloudWatch alarm<br>- Filtered the 127-lab catalog for the fullstack path, selected 25 labs for 12 weeks<br>- Learned the daily worklog rules and the program's worklog template<br>- Started looking for a workshop topic for the team | 22/09/2026 | 22/09/2026 | https://000003.awsstudygroup.com<br>https://cloudjourney.awsstudygroup.com<br>https://workshop-sample.awsfcaj.com/1-worklog/ |
| 4 | - Read ahead the theory and steps of lab 000048 (IAM Roles for EC2) and lab 000057 (static website on S3); noted the commands that change on Amazon Linux 2023<br>- Summarized and reviewed lab 000004 Compute Essentials with EC2 (9 chapters): Security Groups, EBS, snapshots, AMIs, a Node.js + MariaDB app on one instance | 23/09/2026 | 23/09/2026 | https://000048.awsstudygroup.com<br>https://000057.awsstudygroup.com<br>https://000004.awsstudygroup.com |
| 5 | [fill in] | 24/09/2026 | | |
| 6 | [fill in] | 25/09/2026 | | |

### Week 2 Achievements:

* Built a VPC following the standard architecture: public and private subnets separated across 2 AZs, an Internet Gateway and a route table for the public subnets.
* Understood that the route table, not the subnet name, decides whether a subnet is public or private.
* Applied least privilege in Security Groups: the private instance accepts SSH only from the public instance's Security Group, never from the Internet.
* Reached the private instance through a bastion; learned from the docs two safer options, EC2 Instance Connect Endpoint and Session Manager (no SSH keys, no open port 22).
* Enabled VPC Flow Logs to record network traffic in the VPC.
* Solved real issues: `.pem` file permissions on Windows (`icacls`), nested SSH not accepting "yes" (`StrictHostKeyChecking=accept-new`).
* Cleaned up every resource after the lab, leaving no ongoing cost.
* Have a 25-lab path for the fullstack direction across the 12-week internship.
