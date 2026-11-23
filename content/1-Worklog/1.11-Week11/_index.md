---
title: "Week 11 Worklog"
date: 2026-09-14
weight: 11
chapter: false
pre: " <b> 1.11. </b> "
---

### Week 11 Objectives:

* Release `v1.0.0` to prod: record acceptance results, merge `release/v1.0.0` into `main`, tag, deploy prod, check the WAF.
* Finish the FCAJ report before the ~29/11 deadline: chapter 5 intro and 5.1, merge 5.4 (Hoàng) and 5.5 (Nhân), Proposal, Events, Blog 3, Self-evaluation, Feedback.
* Review and merge the 2-tab fix (#515) for `v1.0.1`.

### Tasks to be carried out this week:

| Day | Task | Start Date | Completion Date | Reference Material |
|-----|------|------------|-----------------|--------------------|
| 2 | - Worked at the office <br> - Reviewed team progress: PRs, issues, release, report <br> - Reminded Hoàng (5.4), Nhân (5.5 screenshots) of the 24/11 deadline <br> - **`v1.0.0` acceptance on staging:** <br>&emsp; + N2 admin cancels an order, checked stock in DynamoDB <br>&emsp; + N3 forgot password, checked the session-revoke trigger logs <br>&emsp; + N6 sign-in with an old session on the device <br> - Recorded acceptance results, added the SES sandbox limitation (#523) <br> - Approved and merged #515 (2-tab bug, closes #508) <br> - Merged #521 (flaky MFA tests) and 3 Dependabot PRs <br> - Fixed #519 (cdk-nag L1 after `aws-cdk-lib` added new runtimes), merged <br> - **Released `v1.0.0`:** <br>&emsp; + #523 into `release/v1.0.0`, staging deploy green <br>&emsp; + #524 `release/v1.0.0` → `main` <br>&emsp; + Tagged `v1.0.0`, prod deploy green, checked the prod WAF <br> - Outlined Blog 3 (L9: password reset on Cognito) | 23/11/2026 | 23/11/2026 | [AWS WAF managed rules](https://docs.aws.amazon.com/waf/latest/developerguide/aws-managed-rule-groups-baseline.html) <br> [TransactWriteItems](https://docs.aws.amazon.com/amazondynamodb/latest/APIReference/API_TransactWriteItems.html) <br> [AdminUserGlobalSignOut](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_AdminUserGlobalSignOut.html) <br> [Amplify token refresh](https://docs.amplify.aws/react/build-a-backend/auth/connect-your-frontend/manage-user-sessions/) |

### Task details:

#### Monday (23/11/2026)

* **Worked at the office.**
* **Team progress:** `release/v1.0.0` is on staging, not tagged yet; #515 (Hoàng) has a fix commit for the guest cart, CI green; 4 Dependabot PRs (#519 red) wait until after the tag. Report: 5.2, 5.3, 5.6, 5.7 are live; still empty: chapter 5 intro, 5.1, Proposal, Events, Self-evaluation, Feedback, Blog 3; 5.4 is not in the repo yet, 5.5 lacks screenshots. Reminded Hoàng and Nhân of the 24/11 draft deadline.
* **N2, admin cancels a `CONFIRMED` order:** placed order `01M8369G…` (2 items, 17,080,000₫) as a customer, it became `CONFIRMED` within 2 seconds. The admin (with MFA) cancelled it in `/admin`: the confirm dialog showed the right order ID, item count and total; the order became `CANCELLED` and the customer sees "Đã huỷ". Stock read in DynamoDB: 10 → 9 → 10 and 30 → 29 → 30. The first stock read came too late (the order was already placed), so it was cross-checked against the `products.json` seed and the list of orders containing both items.
* **N3, forgot password:** reset with the emailed code, signed in with the new password. Staging Post confirmation trigger logs at 10:24:28 and 10:32:59: "Đã đăng xuất mọi thiết bị sau khi đặt lại mật khẩu", no errors. Lesson: deleting the `accessToken` cookie does not force a token refresh because Amplify treats it as signed out; to see `Refresh Token has been revoked` in the browser, wait for the old access token to expire (1 hour). The "other device" must be a different browser, since Chrome incognito windows share cookies.
* **N6:** all 3 cases passed: F5 on `/orders` does not go to `/login` (#500); signed in as A, signing in as B with a wrong password shows the error and signs A out, the right password gets into B (#507).
* **#523 into `release/v1.0.0`:** recorded N1–N8 (all 8 passed), added "SES is still in sandbox in all 3 environments" to Known limitations, ticked the re-cut steps. Waiting for Hoàng or Nhân to review.
* **#515:** fix commit `8401a92` matches the feedback: a guest tab only reloads and keeps the guest cart, merge key and checkout intent; new test, `dang-nhap.md` updated. Approved and merged into `develop` (does not touch `release/v1.0.0`), #508 closed automatically; asked Hoàng to test on dev including 2 guest tabs with items in the cart.
* **Other PRs into `develop`:**
  * #521 (Hoàng): the two-factor page tests waited only 1 second for the first render, so a slow CI failed at random; raised to 5 seconds for first-render waits only, no runtime code change. Merged.
  * Dependabot #516 (`setup-uv`), #518 (`uv-build`) green, merged. #517 (`ruff`) was red from that same flaky MFA test; after Dependabot rebased onto `develop` with #521, CI passed and it was merged. Dev deploy `d62348a` green.
* **#519 (Dependabot, 24 libraries):** the Infra job was red because `aws-cdk-lib` 2.277 added `nodejs26.x` and `python3.15`, so the cdk-nag rule `AwsSolutions-L1` (latest runtime) flagged all 15 Lambdas on `nodejs24.x` and `python3.14`. Chose an app-wide L1 acknowledgement with a reason in `infra/bin/shop.ts` instead of upgrading runtimes right before the deadline: Node 24 is LTS and matches `.nvmrc`; Python 3.14 matches `data/`; both are still supported by AWS; the runtime upgrade goes in a separate PR tested on dev. Checked locally: synth with cdk-nag passes, 137/137 infra tests, Prettier and ESLint clean; CI green, merged into `develop`.
* **Released `v1.0.0`:**
  * #523 (acceptance results) reviewed and merged by Hoàng at 11:33; staging deploy `06cf3ac` green.
  * #524 `release/v1.0.0` → `main`: reviewed by Hoàng, merged at 11:42 (`2e78553`), release branch kept.
  * Tagged `v1.0.0` on `main` and pushed; the Deploy prod workflow waited for the `production` environment approval, given by Hoàng. All steps green (OIDC, `cdk deploy shop-prd`, smoke test), GitHub Release `v1.0.0` created at 11:52.
  * Checked the prod WAF (`shop-prd-web`, us-east-1): `AWSManagedRulesCommonRuleSet` is blocking (OverrideAction `None`, no rule overrides). Requests: `/` and `/api/v1/products` 200; a query with `<script>` 403; `../../etc/passwd` 403. #457 is closed.
  * Left in the checklist: place and cancel a test order on prod; write lessons at the end of `docs/releases/v1.0.0.md`. `release/v1.0.0` has no fixes missing from `develop`.
* **Blog 3 outline:** topic L9: a password reset on Cognito does not sign out other devices. Built on the team's real story: dev test 11/11 (old refresh token still works), the fix with a Post confirmation trigger calling `AdminUserGlobalSignOut` (16/11), breaking the trigger on purpose (17/11: Cognito changes the password before calling the trigger, and returns the raw error with the account ID to the browser), today's staging logs. Includes a fact table to check against while writing.

### Week 11 Achievements:

* `v1.0.0` acceptance on staging passed all 8 cases; results and the SES sandbox limitation are recorded in the checklist (#523).
* **`v1.0.0` is live on prod** (tag, GitHub Release, smoke test green); the prod WAF blocks correctly with the core rule set.
* The account mismatch between 2 tabs is fixed on `develop` (#515), along with the flaky MFA tests (#521) and 3 library updates.
* Remaining this week: a test order on prod, write and post Blog 3, the empty report sections before ~29/11.
