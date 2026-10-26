---
title: "Week 7 Worklog"
date: 2026-09-14
weight: 7
chapter: false
pre: " <b> 1.7. </b> "
---

### Week 7 Objectives:

* Prepare the `v0.2.0` release: cut the release branch on 30/10, tag on 02/11.
* Get product recommendations working on dev: Nhân's nightly pipeline writes the `recs` table, the recommendations API returns data, widgets on the web.
* Finish and publish Blog 1.

### Tasks to be carried out this week:

| Day | Task | Start Date | Completion Date | Reference Material |
|-----|------|------------|-----------------|-------------------|
| 2 | - Reviewed the PRs the team merged over the weekend without review: order confirmation worker infrastructure (#263), worker SES emails (#265), order E2E fix (#267), Nhân's nightly recommendations job (#269). The worker has narrow IAM permissions, only the worker may publish to the `order-confirmed` topic, and concurrency is capped at 2 to fit the SES sandbox limit of 1 email/second; the email code follows the agreed decisions but is not enabled yet, pending the SES and Cognito permissions PR. Found why the #265 deploy failed (E2E expected a `PENDING` order while the worker had already confirmed it), fixed by #267<br>- Reviewed and merged 4 Dependabot PRs (17 npm packages including AWS SDK, CDK, vite; ruff; uv-build; the setup-uv action); final dev deploy green, verified `uv.lock` and `pnpm-lock.yaml` still match<br>- Prepared Blog 1 on deploying with OIDC: checked every technical detail against the `github-oidc.yaml` template, the workflow, PR #25, the timed run, GitHub Environments rules and GitHub's docs on immutable subjects; updated the numbers (118 successful deploys on `develop`); corrected the incident section to what I remember and can verify; the illustration was made with AI tool assistance, disclosed in the post. Not published yet | 26/10/2026 | 26/10/2026 | https://docs.github.com/en/actions/reference/security/oidc<br>https://docs.aws.amazon.com/STS/latest/APIReference/API_AssumeRoleWithWebIdentity.html<br>https://docs.aws.amazon.com/lambda/latest/dg/services-sqs-scaling.html |

### Week 7 Achievements:

* In progress.
