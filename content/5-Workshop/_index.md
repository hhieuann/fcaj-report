---
title: "Workshop"
date: 2026-09-14
weight: 5
chapter: false
pre: " <b> 5. </b> "
---

# Building a serverless web store with product recommendations on AWS

#### Overview

This workshop rebuilds **shop-ai (Black Magic)**, the team's store for laptops and PC parts, running fully serverless on AWS. You deploy the whole system into your own account with AWS CDK, then walk through each important part:

- **Infrastructure:** CloudFront as the single entry point for web and API, S3 for the static web, Cognito for sign-in, API Gateway checking JWTs, Lambda and DynamoDB per module.
- **Orders that are never lost or duplicated:** one DynamoDB transaction, `Idempotency-Key`, an SQS queue with a DLQ, email through SES.
- **Home-built recommendations:** a Python job run nightly by EventBridge Scheduler, user data hashed before storage, and a recommendation poisoning experiment.
- **Operations:** logs, traces, alarms, runbooks; deployment through GitHub Actions with OIDC; and cleaning up every resource at the end.

The workshop was written by the whole team: An (5.1, 5.2, 5.3, 5.6, 5.7), Hoàng (5.4), Nhân (5.5). Source code: [github.com/hhieuann/shop-ai](https://github.com/hhieuann/shop-ai).

#### Contents

1. [Introduction](5.1-introduction/)
2. [Prerequisites](5.2-prerequisite/)
3. [Deploying the Infrastructure](5.3-infrastructure/)
4. [Sales Flow](5.4-salesflow/)
5. [Recommendations and Data Security](5.5-recommendations/)
6. [Testing and Operations](5.6-testing/)
7. [Clean Up](5.7-cleanup/)

{{% notice info %}}
The lab runs in your own sandbox and costs close to nothing if you finish chapter 5.7. The cost of running for real for a month (about $24.86) is at the end of chapter 5.2.
{{% /notice %}}
