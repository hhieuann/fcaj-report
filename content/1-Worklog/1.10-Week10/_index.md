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
* Review the team's PRs for the release after v1.0.0 (admin order cancellation, forgot password, account page) and write Blog 3.

### Tasks to be carried out this week:

| Day | Task | Start Date | Completion Date | Reference Material |
|-----|------|------------|-----------------|--------------------|
| 2 | - Checked the CloudWatch Alarms Free Tier alert <br> - Switched the prod WAF core rule set to block (#458) <br> - Trial run of the lab on a sandbox, added the HMAC key step (#463) <br> - Reviewed Nhân's L9 PR (#464) <br> - **Workshop:** <br>&emsp; + 7-chapter outline, split the work in the team <br>&emsp; + Cost estimate in Pricing Calculator <br>&emsp; + Wrote chapters 5.2, 5.3, 5.6, 5.7 (bilingual) <br>&emsp; + Cleaned up the sandbox for real to take the 5.7 screenshots | 16/11/2026 | 16/11/2026 | [CloudWatch pricing](https://aws.amazon.com/cloudwatch/pricing/) <br> [Free Tier usage](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/tracking-free-tier-usage.html) <br> [SQS DLQ redrive](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-configure-dead-letter-queue-redrive.html) <br> [Post confirmation trigger](https://docs.aws.amazon.com/cognito/latest/developerguide/user-pool-lambda-post-confirmation.html) <br> [Cost estimate](https://calculator.aws/#/estimate?id=0798eb584d82c280a557b96fbf3c8fbe695a6ca0) <br> [cdk destroy](https://docs.aws.amazon.com/cdk/v2/guide/ref-cli-cmd-destroy.html) |
| 3 | - Agreed the account page rules with Hoàng (change username, change password) <br> - Built the `PATCH /api/v1/me` API to change the username (#471, PR #472) <br> - Tested Post confirmation trigger failures (L9) on dev with Nhân <br> - Agreed `shared/auth` for the account page <br> - **Reviewed and merged PRs:** <br>&emsp; + #476 L9 fixes (Nhân), #484 runbook fix <br>&emsp; + #485 `/account` layout (Hoàng) <br>&emsp; + #486 Workshop chapter 5.5 (Nhân) <br> - Proposed a fix for sign-out bug #487 | 17/11/2026 | 17/11/2026 | [AdminUpdateUserAttributes](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_AdminUpdateUserAttributes.html) <br> [ListUsers](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_ListUsers.html) <br> [ChangePassword](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_ChangePassword.html) <br> [GlobalSignOut](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_GlobalSignOut.html) <br> [Lambda concurrency](https://docs.aws.amazon.com/lambda/latest/dg/configuration-concurrency.html) |

### Task details:

#### Monday (16/11/2026)

* **CloudWatch Alarms Free Tier alert:** AWS emailed that more than 85% of the November allowance was used (8.5/10). Counted 13 alarms, all in `ap-southeast-1` and all OK (prod 7, staging 3, dev 3). Alarms with metric math count as 2 metrics, 20 in total, about 10 over, roughly $1/month taken from credits. Decided to keep them because the dev and staging alarms catch errors before prod and are monitoring evidence for the Workshop; removing them means changing `monitoring.ts`, not deleting by hand.
* **Prod WAF core rule set switched to block (#458, issue #457):**
  * Logs from the rc.1 deploy (12/11 10:20) to 16/11: 678 requests, 0 BLOCK; the core rule set counted 74 requests, all of them XSS test requests, 0 false matches.
  * Changed `COMMON_RULE_SET_COUNT_ONLY` to `false`, 132/132 infra tests pass; approved by Nhân, merged into `release/v1.0.0` to reach prod with `v1.0.0`.
  * Reviewed Nhân's WAF runbook PR (#460).
* **Trial run of the lab on a sandbox:** `cdk destroy` then `cdk deploy` from scratch, about 5 minutes, 104 products seeded. Found 2 gaps for the docs:
  * `cdk destroy` leaves the log groups of the CDK helper Lambdas.
  * The account lacked the HMAC key `/shop/sbx/events/hmac-key`, so `PURCHASE` messages went to the DLQ. Created the key, redrove the DLQ, added the key step to `infra/README.md` (#463).
* **Reviewed Nhân's L9 PR (#464):** the Post confirmation trigger calls `AdminUserGlobalSignOut` only after a password reset, with exactly one IAM permission; approved. Noted that the trigger runs after the password has changed, and asked Nhân to test the failure case on dev.
* **Workshop:**
  * Decided to write it as a lab where readers deploy shop-ai themselves with CDK, one shared version for the team; drafted a 7-chapter outline, drafts due around 24/11.
  * Cost: entered 14 services into AWS Pricing Calculator using the real configuration in the code, total **$24.86/month**; CloudFront fits in the Free Tier, so the real cost is about $17.5/month, mostly WAF. Recorded 3 defaults that distort the result (DynamoDB provisioned, Cognito Advanced security, SNS/EventBridge units).
  * Wrote chapters 5.6 Testing and Operations, 5.2 Prerequisites, 5.3 Deploying the Infrastructure (bilingual, live on the site); checked on the real sandbox: all security headers present, `/api/v1/products` returns 200, `/api/v1/me` without sign-in returns 401.
  * Cleaned up the sandbox for real and wrote chapter 5.7 Clean Up: `cdk destroy` took about 5 minutes; deleted the 5 leftover log groups and the HMAC key; checked that no table, bucket, user pool or stack remained. Took screenshots with the account ID cropped out.
  * Prepared the architecture diagram: listed about 35 components and 50 connections from the `infra/lib/` code; used an AI-generated sketch as a layout reference and fixed 7 points against the code; the submitted diagram will be drawn by hand in draw.io as the rules require.

#### Tuesday (17/11/2026)

* **Hoàng's account page:** read the proposal (change username, sign out on `/account`, change password). Decided that a password change **signs out everywhere**, because people change their password when they suspect a leak; Hoàng fixes the forgot-password page when signed in; customer MFA is postponed.
* **`PATCH /api/v1/me` API to change the username (#471, PR #472):**
  * Reuses the sign-up username rules; checks duplicates with `ListUsers`, and changing to your own name still returns 200; writes with `AdminUpdateUserAttributes`.
  * Errors: 400 `USERNAME_REQUIRED`, 400 `USERNAME_INVALID`, 409 `USERNAME_TAKEN`.
  * The Lambda has 2 Cognito permissions, only on the environment's exact user pool ARN.
  * api 592, web 462, infra 136 tests pass; CI 9 checks pass; Hoàng approved and merged; dev deploy and E2E against real Cognito pass.
  * Known limit: calling Cognito directly can still write `preferred_username` and `email`; asked Nhân to record it in the threat model.
* **Tested Post confirmation trigger failures (L9) on dev with Nhân:** An toggled each case with the CLI (dev shares the account with prod, so Nhân was not given access), Nhân tested on the web.
  * Case B (deny `AdminUserGlobalSignOut`): the web shows an error, but **the password was changed** and other devices **stayed signed in**; the error message exposed the account ID and role name.
  * Case A (reserved concurrency = 0): same outcome as case B; the function was throttled 4 times with Errors = 0, so the alarm must also count Throttles.
  * The `shop-prd-lambda-throttles` alarm went to ALARM at 09:53 and back to OK at 09:58: it counts throttles across the whole account; all 4 came from the dev function, prod was fine.
  * Removed both cases and checked again: function `Active`, role has only the CDK policy.
* **Agreed `shared/auth` with Hoàng:** reading Amplify 6.21.2 showed `signOut({ global: true })` swallows every error and still clears this device; proposed calling `GlobalSignOut` directly and showing an error with a Retry button.
* **Reviewed and merged PRs:**
  * #476 (Nhân): the trigger only throws `SESSION_REVOKE_FAILED`, Errors + Throttles alarm in prod only. One runbook comment, fixed myself in #484.
  * #485 (Hoàng): the `/account` layout, nothing blocking; 2 notes for later PRs.
  * #486 (Nhân): Workshop chapter 5.5, matches the code; 6 comments for Nhân to fix in a follow-up PR.
* **Bug #487 (signing out from the header on a signed-in-only page goes to `/login`):** proposed counting sign-outs instead of an on/off flag, because the session state updates asynchronously.

### Week 10 Achievements:

* The prod WAF now blocks with the core rule set, ready for `v1.0.0`.
* 4 Workshop chapters are live on the site (5.2, 5.3, 5.6, 5.7), with a $24.86/month cost estimate; Nhân's chapter 5.5 has a draft in the shop-ai repo.
* The lab docs were fixed from a real trial run: the HMAC key step and the leftover log group step.
* The change-username API runs on dev, E2E passes.
* L9 was tested with real failures: found and fixed an information leak in the error message, added an alarm for the trigger.
* Remaining this week: chapter 5.1 (waiting for the hand-drawn diagram), Proposal, Events, demo rehearsal before `v1.0.0`.
