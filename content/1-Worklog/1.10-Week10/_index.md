---
title: "Week 10 Worklog"
date: 2026-09-14
weight: 10
chapter: false
pre: " <b> 1.10. </b> "
---

### Week 10 Objectives:

* Prepare `v1.0.0` (23/11): switch the WAF core rule set to block after reading the prod logs, merge the release branch back into `develop`, rehearse the demo on prod.
* Write the empty FCAJ report sections before the ~29/11 deadline: Proposal, Workshop (architecture, step by step, testing, cost, clean-up), Events; add the P1–P3 numbers to the report.
* Review the team's PRs for the release after v1.0.0 (admin order cancellation, forgot password) and write Blog 3.

### Tasks to be carried out this week:

| Day | Task | Start Date | Completion Date | Reference Material |
|-----|------|------------|-----------------|-------------------|
| 2 | - Read the AWS Free Tier alert email (15/11): CloudWatch Alarms passed 85% of the free monthly limit for November (8.5/10). Counted alarms in every region: 13 alarms, all in `ap-southeast-1` and all OK (prod 7, staging 3, dev 3). AWS bills per metric, so alarms using metric math (latency p95, presignup errors, recs pipeline) count as 2, 20 alarm metrics in total, about 10 over, roughly $1/month taken from credits. Decided to keep them because the dev and staging alarms catch errors before prod and are monitoring evidence for the workshop; removing them would mean changing `monitoring.ts`, not deleting by hand | 16/11/2026 | 16/11/2026 | https://aws.amazon.com/cloudwatch/pricing/<br>https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/tracking-free-tier-usage.html |

### Week 10 Achievements:

* In progress.
