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
| 3 | - Agreed the account page rules with Hoàng (change username, change password) <br> - Built the `PATCH /api/v1/me` API to change the username (#471, PR #472) <br> - Tested Post confirmation trigger failures (L9) on dev with Nhân <br> - Agreed `shared/auth` for the account page <br> - **Reviewed and merged PRs:** <br>&emsp; + #476 L9 fixes (Nhân), #484 runbook fix <br>&emsp; + #485 `/account` layout (Hoàng) <br>&emsp; + #486 Workshop chapter 5.5 (Nhân) <br> - Proposed a fix for sign-out bug #487 <br> - Reformatted worklog weeks 4–10 to match the template <br> - Reviewed PR #489 fixing bug #487 (Hoàng) <br> - Wrote a README for the report repo <br> - Reviewed team progress, drafted the `v1.0.0` plan <br> - Workshop chapter status table for the team (#491) <br> - Fixed the flaky test breaking CI (#492, PR #493) <br> - Cleaned up finished issues still open <br> - Reviewed PR #496 change password (Hoàng) | 17/11/2026 | 17/11/2026 | [AdminUpdateUserAttributes](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_AdminUpdateUserAttributes.html) <br> [ListUsers](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_ListUsers.html) <br> [ChangePassword](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_ChangePassword.html) <br> [GlobalSignOut](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_GlobalSignOut.html) <br> [Lambda concurrency](https://docs.aws.amazon.com/lambda/latest/dg/configuration-concurrency.html) |

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
* **Reviewed Hoàng's PR #489 (fix for bug #487):** follows the `signOutCount` approach, removes `isLeaving`/`leavingRef` (comment on #485), `/checkout` uses the shared `useRequireSignIn`, 10 new tests. The first CI run failed 1 test (`headerSignOut_goesHome_on /orders`): locally the file passed 8/8 three times and a CI rerun passed, so it is a flaky test: the first case loads the lazy route for the first time and exceeds the 1-second default wait of `findByRole`. Commented asking Hoàng to raise the wait or preload the routes in `beforeAll`, approval after the fix.
* **README for the `fcaj-report` repo:** the repo had no README, so visitors on GitHub could not find the site link. Added the site links (VI, EN), a table of links to the 7 sections, how to update and preview locally; checked every link opens (fixed a wrong Blogs path); set the site link in the repo's About box on GitHub.
* **Reviewed team progress and Nhân's recommendations:** since 10/11, 27 PRs merged from Hoàng, 7 from An, 4 from Nhân. The recommendation pipeline runs every night at 02:00 in all 3 environments; on dev and staging the models beat the baseline (also-bought precision@10 0.074 vs 0.032; for-you 0.056 vs 0.035), prod has only 7 real events so its recommendations are almost empty and bestsellers fall back to newest products. Remaining: Hoàng has not started chapter 5.4 (draft due 24/11), Nhân fixes 5.5 and adds screenshots, An writes 5.1, Proposal, Events; `release/v1.0.0` is 112 commits behind `develop`, so the team must decide what goes into `v1.0.0`. Drafted a message to the team: proposed re-cutting `release/v1.0.0` from `develop` (Hoàng finishes #479 before 19/11, cut and deploy staging 20/11, acceptance 20–21/11, tag 23/11), reminded the 24/11 Workshop draft deadline, demo data on prod (place a few real orders, or demo recommendations on staging), fix the flaky test from #489, close duplicate issues #482, #488; asked the team to answer the `v1.0.0` point before 18/11.
* **Workshop chapter status table (issue #490, PR #491):** Hoàng asked where to see which chapters are missing; the status and outline lived only on An's machine. Added to `docs/workshop/README.md`: a table of page 5, 5.1–5.7 and the demo video (author, status, links to finished chapters on the site), outlines for 5.1 and 5.4, the writing pattern and how to submit drafts, the 24/11 and 29/11 deadlines. Sent Hoàng the 5.4 outline and links to the example chapters.
* **CI failure on PR #491 (docs only):** the log showed the `signOutRedirect.test.tsx` test from #489: the first case waits more than 1 second for the account button because the first render loads code for the first time, and CI is slower than a laptop. A rerun failed again, so it fails almost every time, not just occasionally; Hoàng merged #489 without fixing it, so every PR was affected. Opened issue #492 and PR #493 raising the first wait to 5 seconds (test only); 5/5 local runs pass, CI 9 checks pass, merged. Updated the #491 branch from `develop`, CI green, merged #491. The 11:11 dev deploy failure was `ECONNRESET` in the cart E2E (a transient network error); the next deploy passed, so no fix was needed.
* **Issue clean-up:** closed 5 finished issues that were still open: #452, #428 (done in #453, #429, but those PRs targeted the release branch so GitHub did not close them), #482, #488 (duplicates of #483, fixed in #484), #494 (duplicate of #492, fixed in #493). Kept #457 (WAF blocking, waiting for `v1.0.0` on prod), #479 (Hoàng's account page 3/3), #183 (customer MFA, later) open.
* **Reviewed Hoàng's PR #496 (account page 3/3, change password and forgot password while signed in, #479):** matches what was agreed: after a change, sign out everywhere then `/login`; if global sign-out fails, keep the session with a Retry button; forgot password while signed in no longer redirects, the email is read-only; threat model adds L10; 15 new tests, CI 9 checks pass. One agreed item is missing: the forgot-password page does not handle the L9 trigger failure separately (`UserLambdaValidationException`, `UnexpectedLambdaException` with a message starting with `PostConfirmation`); the password has already changed but the web shows a generic error; reading `develop` confirmed nothing catches these 2 errors. Commented proposing to treat it as a completed reset, go to `/login` with its own message, with tests, required before merging.
* **Reformatted the worklog on the report site:** week 10 had table cells over 6,000 characters, each row about 4,500 px tall, unreadable. Compared with the `fcj-workshop-template`: same `.md` files and the same 5-column table, but the template keeps each cell to a few short items.
  * Rewrote week 10 as the example: table cells hold short items, references are named links, details moved to a "Task details" section under the table, filled in the empty Achievements section.
  * Added CSS for the worklog table (`layouts/partials/custom-header.html`): top-aligned cells, a wider Task column, links no longer broken mid-word; hid the copy button next to inline code, kept it for Workshop code blocks.
  * Applied it to weeks 4–9 (2 agents in parallel, checked afterwards): the longest cell went from 1,000–4,900 to under 710 characters, link counts unchanged; wrote the empty Achievements for weeks 5 and 6 from the existing details.
  * Moved the old draft worklog folder `D:\AWS\worklog` (weeks 1–4) into `archive/worklog-nhap-cu` so it is not confused with the real worklog on the site.

### Week 10 Achievements:

* The prod WAF now blocks with the core rule set, ready for `v1.0.0`.
* 4 Workshop chapters are live on the site (5.2, 5.3, 5.6, 5.7), with a $24.86/month cost estimate; Nhân's chapter 5.5 has a draft in the shop-ai repo.
* The lab docs were fixed from a real trial run: the HMAC key step and the leftover log group step.
* The change-username API runs on dev, E2E passes.
* L9 was tested with real failures: found and fixed an information leak in the error message, added an alarm for the trigger.
* Remaining this week: chapter 5.1 (waiting for the hand-drawn diagram), Proposal, Events, demo rehearsal before `v1.0.0`.
