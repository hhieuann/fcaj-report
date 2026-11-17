---
title: "Week 9 Worklog"
date: 2026-09-14
weight: 9
chapter: false
pre: " <b> 1.9. </b> "
---

### Week 9 Objectives:

* Finish the work due before cutting `release/v1.0.0` (10/11): k6 load test, PITR restore drill, missing runbooks, checklist update.
* Acceptance testing of `v1.0.0-rc.1` on staging 10–11/11, tag to prod on 12/11.
* Measure performance and recovery numbers (P1–P3) for the report.

### Tasks to be carried out this week:

| Day | Task | Start Date | Completion Date | Reference Material |
|-----|------|------------|-----------------|-------------------|
| 2 | - Merged #393 and #395 (k6 scripts `smoke.js`, `tiers.js`) <br> - Wrote runbooks `pitr-restore.md`, `cost-spike.md` (#401) <br> - **PITR drill on staging (P3)** <br> - Checked Hoàng's pre-check of admin AD1–AD3 on dev <br> - Updated the rc.1 checklist (#403), got Lighthouse P1 numbers <br> - Settled C1 with Nhân, recorded C1 and P1 (#407) <br> - Merged 3 Dependabot PRs, held #400; approved Hoàng's #405 | 09/11/2026 | 09/11/2026 | [DynamoDB PITR](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/PointInTimeRecovery.html) <br> [k6 scenarios](https://grafana.com/docs/k6/latest/using-k6/scenarios/) <br> [AWS Budgets](https://docs.aws.amazon.com/cost-management/latest/userguide/budgets-managing-costs.html) |
| 3 | - Checked the 09/11 nightly run (ZAP, Schemathesis, k6 smoke) <br> - **Cut `release/v1.0.0`** (`ae8fcfa`); merged #409, #411, #400 <br> - **Acceptance preparation:** <br>&emsp; + Ran the recommendations job on staging <br>&emsp; + Reset the staging E2E user's password <br>&emsp; + Created a staging admin, enabled TOTP with the CLI <br> - **P2 load test on staging:** reads and orders, 0 failures <br> - **Acceptance on staging:** <br>&emsp; + Admin AD1–AD3, security S1–S5 passed <br>&emsp; + Technical checks, results sent to the team <br>&emsp; + Checked Hoàng's R1–R3, Nhân's W1–W3, E1 <br> - Reviewed and merged fix #419 into the release branch <br> - Budgets activity (Earn AWS credits), 4th attempt | 10/11/2026 | 10/11/2026 | [k6 thresholds](https://grafana.com/docs/k6/latest/using-k6/thresholds/) <br> [Cognito password settings](https://docs.aws.amazon.com/cognito/latest/developerguide/user-pool-settings-passwords.html) <br> [GitHub Actions secrets](https://docs.github.com/en/actions/security-for-github-actions/security-guides/using-secrets-in-github-actions) |
| 4 | - Read the 10/11 nightly run (ZAP 0 FAIL, 6 WARN) <br> - Clarified the repeated Lighthouse run with Hoàng <br> - Recorded acceptance results in the rc.1 checklist (#429) <br> - Brought `release/v1.0.0` into `main` (#430, `ee4fd22`) <br> - Earn AWS credits still 4 of 5, prepared a Support case <br> - Checked Nhân's favicon 403 issue (#431) <br> - Agreed the Forgot password approach, reviewed threat model | 11/11/2026 | 11/11/2026 | [ZAP alerts](https://www.zaproxy.org/docs/alerts/) <br> [Ruleset rules](https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/managing-rulesets/available-rules-for-rulesets) |
| 5 | - Wrote and posted **Blog 2** on the PITR drill, updated Blogs <br> - **Tagged `v1.0.0-rc.1`, prod deploy green** <br> - Checked prod against checklist section 6 (WAF, API, headers) <br> - Marked the GitHub Release as pre-release <br> - Reviewed Hoàng's #440 (Forgot password docs) <br> - Set up the prod admin account with TOTP <br> - Decided 8 design points for admin order cancellation (BR-11) | 12/11/2026 | 12/11/2026 | [PITR restore](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/pointintimerecovery_restores.html) <br> [table-exists waiter](https://docs.aws.amazon.com/cli/latest/reference/dynamodb/wait/table-exists.html) <br> [WAF logging](https://docs.aws.amazon.com/waf/latest/developerguide/logging.html) |
| 6 | - Ran the WAF Logs Insights query for Nhân's check <br> - Checked the first nightly recommendations job on prod <br> - Admin acceptance on prod <br> - Recorded sections 4–6 of the rc.1 checklist (#453) | 13/11/2026 | 13/11/2026 | [WAF log fields](https://docs.aws.amazon.com/waf/latest/developerguide/logging-fields.html) <br> [Cognito TOTP MFA](https://docs.aws.amazon.com/cognito/latest/developerguide/user-pool-settings-mfa-totp.html) <br> [RFC 6238](https://datatracker.ietf.org/doc/html/rfc6238) |

### Task details:

#### Monday (09/11/2026)

* **Merged #393 (Hoàng's status in the rc.1 checklist) and #395 (k6 scripts):**
  * `smoke.js` runs nightly against staging through CloudFront.
  * `tiers.js` measures 50 and 200 virtual users directly against API Gateway, with a place-then-cancel order path when E2E credentials are set.
  * Trial runs: smoke 105 requests, 0 failures, p95 162 ms; tiers with 5 virtual users p95 220 ms.
* **Runbooks `pitr-restore.md` and `cost-spike.md` (#401):** corrected the plan's "restore to a new table and point back to it": Lambda has no IAM access to the new table, so the needed items must be copied back to the old table.
* **PITR drill on staging (P3):**
  * Restored the `orders` table (23 items) to 5 minutes before the run; the new table became ACTIVE after 3–10 minutes, 23 = 23 items, GSI `byStatus` ACTIVE.
  * The new table has no TTL, PITR or deletion protection, as expected. Deleted the temporary table.
  * Learned that even a 9 KB table takes several minutes, and the `table-exists` waiter only waits about 8 minutes.
* **Checked Hoàng's pre-check of admin AD1–AD3 on dev:** deploy `9f9adf0` green, the 2 E2E 401/403 tests passed, the dev `admin` group only contains Hoàng's account. Recorded as a pre-check because the author tested his own feature; the official acceptance is still on staging.
* **Updated the rc.1 checklist (#403):**
  * Dev deploy green (E2E 46/46); nightly ZAP and Schemathesis green; no High Dependabot or CodeQL alerts; the release ruleset blocks branch deletion.
  * Costs 01–09/11: dev $0.022, staging $0.020, prod $0.001.
  * MFA #183 moved to after `v1.0.0`. Sent the team the acceptance schedule for 10–11/11.
* **Hoàng's Lighthouse P1 numbers on dev:** mobile 99/98/99, largest median LCP 2.11 s, largest CLS 0.043, JS 131–147 KB, all 4 targets met.
* **Settled C1 (direct `execute-api` calls bypass WAF) with Nhân:** recorded as a known limitation for rc.1. Checked Nhân's reasons in the code: stage throttle 50/100, every write route has the JWT authorizer, event spam stays at 0.3–2% per the poisoning experiment. Recorded C1 and P1 in the checklist (#407).
* **Dependabot:** merged 3 PRs that do not touch running code (#397–#399); held #400 (17 web and Lambda libraries) until after the cut so rc.1 keeps the exact code measured for P1.
* **Reviewed and approved Hoàng's #405 (order page spec):** checked it against the code; reminded him to open the 5 follow-up code PRs after the cut.

#### Tuesday (10/11/2026)

* **09/11 nightly run:** ZAP and Schemathesis green; k6 smoke ran for real for the first time, 73 requests, 0 failures, p95 823 ms (close to the 1000 ms threshold because staging Lambdas start cold at night).
* **Cut `release/v1.0.0` (`ae8fcfa`):** merged `main` at cut time so the PR into `main` is not `BEHIND`. Verified the running code on the branch is identical to `9f9adf0` (the build measured for P1 and pre-checked for admin). Staging deploy green: smoke test, E2E.
* **Reviewed Hoàng's PRs:** #409 (API returns `firstItemName`, the admin API keeps 5 fields through its own type) and #411 (`Button`, `Badge`, `EmptyState`, `ConfirmDialog` with `tone`); approved and merged them into `develop` after the cut, together with #400. Dev deploy green.
* **Ran the recommendations job manually on staging:** 104 products, all 3 recommendation types activated version `20261110` (ALSO_BOUGHT hit rate 0.54 vs. baseline 0.28), no errors in the log.
* **P2 load test on staging:**
  * Reads: 50 virtual users p95 127 ms; 200 virtual users (36 requests/second) p95 108 ms; 0 failures, no throttling.
  * Placed and cancelled 80 orders: p95 269 ms, 0 failures.
  * Afterwards no open orders remained and the queues and DLQs were at 0.
* **Staging E2E user's password:** did not remember it, so I set a new random password on Cognito and in the GitHub secret without showing it on screen; reran the staging deploy and the order E2E tests passed 9/9 with the new secret.
* **Mistake:** the first k6 command still had the placeholder `<user E2E>` and failed; the next one used the real values from the GitHub environment variables.
* **Created my own staging admin account:** enabled TOTP through the CLI, following the process agreed in threat model L5 (the web has no MFA setup screen yet). The first attempt failed with `Missing required parameter PASSWORD` because pasting the whole block made `Read-Host` read an empty line; running the blocks one at a time worked, and the account now has `SOFTWARE_TOKEN_MFA`.
* **Admin acceptance AD1–AD3 on staging, passed:**
  * Moved an order `CONFIRMED` → `SHIPPED` → `DELIVERED` with a confirmation dialog each time and no way back.
  * The customer sees "Đang giao" and the Cancel button is gone.
  * A customer account opening `/admin` is told it has no permission; a signed-out visitor is sent to `/login`; the admin API returns 401 without a token.
* **Security checks S1–S5 on staging, passed:** another account viewing or cancelling A's order gets 404; no token gets 401; a wrong price gets 409 `PRICE_CHANGED` with no order created; reusing an `Idempotency-Key` with a different body gets 422. Cancelled the test order; stock unchanged.
* **Technical checks after acceptance:**
  * Queues and DLQs at 0; no errors in the metrics of 17 Lambdas.
  * The logs only show `not_verified` email warnings (SES sandbox) and 2 `TransactionConflictException` errors when k6 cancelled orders right after placing them; SQS redelivered and processed them.
  * Security headers present on all 3 CloudFront behaviours; a real sign-in with TOTP is not blocked by the CSP.
* **Sent the team** the day's acceptance results and what is still pending (Hoàng R1–R3, Nhân W1–W3, E1).
* **First bug fix into the release branch:** reviewed and merged Hoàng's #419 (issue #417, the "3 undelivered orders" message on the checkout page now uses the warning colour instead of the error colour, as documented). Checked that the branch came from `release/v1.0.0`, has a test that catches the bug, and CI is green; staging redeploy green. Asked Hoàng to retest R2.
* **"Set up a cost budget using AWS Budgets" activity (Earn AWS credits, $20), 4th attempt:**
  * Chose Start activity and created a new budget "My Monthly Cost Budget" from the guided flow with `tutorialId`.
  * Confirmed the budget through the API; the widget still shows "Not started", 4 of 5 ($80).
  * Waiting until 11/11; if it is still not credited, open an AWS Support case of type Account and billing.
* **Checked Hoàng's R1–R3 results on staging:**
  * Test orders cancelled; queues and DLQs at 0; the 4th order blocked with the yellow message; recommendation APIs return the right `source`.
  * Found that Hoàng still had 3 `CONFIRMED` orders left from R2, and that his repeated Lighthouse run did not state the environment (dev and staging now serve different web builds); asked him to cancel the orders and confirm where he measured.
* **Checked Nhân's W1–W3 and E1 results:**
  * The staging `events` table has `VIEW` and `ADD_TO_CART` at 20:03; `userIdHash` is a 64-character hash with no email or raw `userId`; `ttl` 90 days.
  * The add-to-cart while signed out was not recorded because the route needs a JWT; a missing product returns 404.
  * Confirmed his note that `/favicon.ico` returns 403 (S3 does not allow listing), left for after rc.1.
  * By the end of the day every acceptance row in section 3 had passed; only tonight's ZAP run is pending.

#### Wednesday (11/11/2026)

* **10/11 nightly run against staging on the rc.1 build:**
  * ZAP 0 FAIL, 6 WARN (previously 12), 62 PASS; the warnings for missing CSP, HSTS, X-Frame-Options, X-Content-Type-Options, Permissions-Policy and the `Server` header are gone thanks to #299.
  * New warning "CSP: Wildcard Directive" from `img-src https:`, to tighten after rc.1.
  * Schemathesis 2,893 cases passed; k6 smoke p95 783 ms.
* **Lighthouse with Hoàng:** confirmed his repeated run was on staging with the rc.1 build (`643af49`): 98 / 98 / 94, still passing. Checked he cancelled his 3 test orders; no open orders left on staging.
* **Recorded all acceptance results in the rc.1 checklist (#429, issue #428):**
  * Section 2, AD1–AD3, W1–W3, E1, R1–R3, S1, P1–P3, technical checks, ZAP, bug #417.
  * Changed `tiers.js` to print the Cognito error type when the E2E sign-in fails, tested with a fake client.
  * Pushing one more commit after Hoàng approved dismissed his approval, so I asked him to approve again, then merged; staging redeploy green.
* **Brought `release/v1.0.0` into `main` (#430):**
  * Not `BEHIND`, because `main` was merged at cut time.
  * CI failed the first time because the flaky `HomePage` test hit its 15-second timeout on the runner; the code was identical to a green run, so I reran the job and it passed. Asked Hoàng or Nhân to approve.
  * #430 approved by Hoàng and Nhân and merged into `main` (`ee4fd22`). Verified `main` is identical to the release branch and that **`release/v1.0.0` still exists** thanks to the deletion rule in the ruleset (v0.2.0 lost its branch).
  * Sent the team the plan to tag `v1.0.0-rc.1` to prod on 12/11.
* **Earn AWS credits:** the widget still shows 4 of 5 after the 4th attempt; prepared the text for an AWS Support case.
* **Nhân's issue #431 (favicon 403):** `index.html` has no `<link rel="icon">` and `apps/web/public` is empty, as described; to be fixed after rc.1 on `develop`.
* **Forgot password for the release after v1.0.0 (answered Hoàng):**
  * Checked the user pool already has `accountRecovery: EMAIL_ONLY` and `preventUserExistenceErrors`.
  * Decided: after the reset the user returns to `/login` with the email filled in; the default Cognito email template is kept; the documentation PR comes before the code.
  * Found that threat model L1, L3 and L8 say "WAF rate limiting", but the web calls `cognito-idp` directly so WAF does not apply; asked Nhân to fix them.
  * Decided that if old refresh tokens still work after a password reset, signing out all devices is done right away (a Post confirmation trigger calling `AdminUserGlobalSignOut`, in a separate infra PR).

#### Thursday (12/11/2026)

* **Blog 2 "PITR is on, but have you tried restoring a DynamoDB table?" (posted on the AWS Study Group):**
  * Content: RPO/RTO; PITR always restores to a new table; real numbers from the 09/11 drill (23 = 23 items, 3–10 minutes, latest restorable time 4 min 59 s behind); three lessons.
  * Checked every detail against the AWS documentation before posting: the `table-exists` waiter polls every 20 seconds, at most 25 times; the list of settings to set again after a restore; the AWS CLI merges pages for `scan --select COUNT`.
  * Fixed an arrow in the diagram and renumbered the references.
  * Updated the report's Blogs section with the links to Blog 1 and Blog 2.
* **Tagged `v1.0.0-rc.1` on `main` (`ee4fd22`, the accepted build):** prod deploy approved by Hoàng, green after about 5 minutes; first creation of the `shop-prd-waf` stack in us-east-1, `shop-prd-api` and `shop-prd-web` updated, smoke test green.
* **Checked prod against section 6 of the checklist:**
  * Web ACL `shop-prd-web` attached to CloudFront, 5 rules (2 rate-limit rules blocking, the core rule set counting until about 15/11).
  * WAF logging works, 15 test requests all ALLOW with no false matches.
  * Product and recommendation APIs return 200; `/orders` and `/admin/orders` without a token return 401; all security headers present.
  * The home recommendation block shows "Có thể bạn quan tâm" as expected when `source` is `NEWEST`; 7 prod alarms OK.
* **GitHub Release:** the auto-created release was not marked pre-release and showed as Latest.
  * Wrote extra notes with "Fixes during acceptance" and "Not in this release" (Forgot password, MFA, favicon, CSP `img-src`, C1, core rule set in count mode, SES sandbox).
  * Fixed it with `gh release edit --prerelease`; Latest is back to `v0.2.0`.
  * Still pending: Nhân places and cancels an order on prod to check the WAF log; an admin status change on prod; the 13/11 nightly job.
* **Reviewed Hoàng's #440 (Forgot password documentation):** matches the 3 decisions; the threat model drops WAF from L1, L3, L8 and adds L9 (old sessions survive a password reset, with evidence from a dev test). Asked him to fix one contradictory line in dang-nhap.md and to note that L3 and the sign-out-other-devices criterion depend on L9.
* **Set up the prod admin account:**
  * Added my own account to the `admin` group and enabled TOTP. Prod has no E2E app client, so the access token came from the cookie after signing in on the web.
  * The first attempt returned `Code mismatch` because the 52-character secret was mistyped into the app.
  * Wrote a PowerShell script that computes the TOTP code locally to compare with the app (checked against the RFC 6238 test vector); redid it and got `SOFTWARE_TOKEN_MFA`; the script never writes the secret to a file.
* **Decided the 8 design points for Hoàng's admin order cancellation (BR-11) for the release after v1.0.0, after checking the code:**
  * Cancelling a `SHIPPED` order does not decrement `#OPEN` twice because the transaction checks `isOpen`; the existing cancellation email is neutral.
  * Added 3 notes: find the order through the `byStatus` index to tell 404 from 409; test the metric dimension so the alarm does not go blind; threat model L6/O3.

#### Friday (13/11/2026)

* **WAF runbook Logs Insights query for Nhân's check (87 requests, 09:25–09:50):**
  * The XSS test request was counted by the core rule set under the right sub-rule `CrossSiteScripting_QUERYARGUMENTS`, 0 BLOCK; adding to cart, placing and cancelling the order matched no rule.
  * 49 other requests were counted because their `Referer` carried the test payload (50/50 counted requests had the payload, 37/37 without it were not counted), so not false positives.
  * Suggested Nhân fix the runbook: run the test request in a separate window and show the sub-rule name in the query.
* **First nightly recommendations job on prod:** one run at 02:00, 0 errors, 104 products; prod has only 1 real event so no recommendation version was activated and the API still returns `NEWEST` as designed; 7 prod alarms OK.
* **Admin acceptance on prod:** signed in with TOTP, placed a test order with fake data and moved it `CONFIRMED` → `SHIPPED` → `DELIVERED` in `/admin`; the customer sees "Đã giao" with no Cancel button; queues and DLQs at 0, Lambda 0 errors.
* **Recorded sections 4–6 of the rc.1 checklist (#453, issue #452):** approved by Nhân, merged into `release/v1.0.0`, staging deploy green. Section 6 has 6 of 7 rows passed; switching the core rule set to block is due around 15/11.

### Week 9 Achievements:

* **`v1.0.0-rc.1` reached prod on 12/11 as planned:** cut `release/v1.0.0` on 10/11, acceptance 10–11/11, merged into `main` on 11/11, tagged and deployed to prod on 12/11 (approved by Hoàng). First time with WAF in prod (`shop-prd-waf` stack, us-east-1); the GitHub Release is marked pre-release
* Full acceptance on staging: admin AD1–AD3, security S1–S5, recommendations R1–R3 (Hoàng), events W1–W3 and E1 (Nhân); one fix during acceptance (#419). On prod: WAF checked by the runbook (0 BLOCK), an admin with TOTP moved a test order to `DELIVERED`, 7 alarms OK, the nightly recommendation job had 0 errors
* Numbers for the report: P1 Lighthouse mobile 98/98/94 on staging; P2 k6 reads p95 127 ms (50 virtual users) and 108 ms (200 virtual users), orders p95 269 ms, 0 errors; P3 PITR drill restored 23 = 23 items in 3–10 minutes
* Posted Blog 2 (PITR drill) on the AWS Study Group and updated the report's Blogs section (2 of 3 posts)
* Decided features for the release after v1.0.0: Forgot password (done by Hoàng on `develop`) and admin order cancellation (8 design points); fixed threat model L1, L3, L8 (the web calls `cognito-idp` directly, so WAF does not apply) and added L9
* Not achieved: the Budgets activity of Earn AWS credits is still not credited after the 4th attempt; the WAF core rule set is still in count mode; the release branch has not been merged back into `develop`
* Lessons: do not push more commits after a review is in; the release workflow should mark `-rc` tags as pre-release; when checking WAF, send the test request from a separate window so its `Referer` does not get later requests counted; run `Read-Host` on its own, not inside a pasted block
