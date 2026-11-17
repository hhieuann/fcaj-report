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
| 2 | - **Reviewed PRs merged over the weekend without review:** <br>&emsp; + #263 worker infrastructure, #265 worker SES emails <br>&emsp; + #267 order E2E fix, #269 nightly recommendations job <br> - Reviewed and merged 4 Dependabot PRs <br> - Prepared Blog 1 on deploying with OIDC (not published) <br> - Closed the OIDC review (#180), fixed ADR-0013 (PR #278) <br> - Fixed 2 falsely firing staging alarms (PR #280) <br> - Drafted the v0.2.0 acceptance checklist, set scope (#282, #286) <br> - Re-reviewed Hoàng's #263, #284 (SES email permissions) <br> - Verified the worker on dev with the AWS CLI | 26/10/2026 | 26/10/2026 | [SES access control](https://docs.aws.amazon.com/ses/latest/dg/control-user-access.html) <br> [CloudWatch metric math](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/using-metric-math.html) <br> [Metric filter syntax](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/FilterAndPatternSyntax.html) <br> [GitHub Actions OIDC](https://docs.github.com/en/actions/reference/security/oidc) <br> [AssumeRoleWithWebIdentity](https://docs.aws.amazon.com/STS/latest/APIReference/API_AssumeRoleWithWebIdentity.html) <br> [Lambda SQS scaling](https://docs.aws.amazon.com/lambda/latest/dg/services-sqs-scaling.html) |
| 3 | - Checked the first real run of the recommendations job on dev <br> - **Recommendation widgets:** <br>&emsp; + Home page (PR #289) <br>&emsp; + "Thường mua kèm" on the product page (PR #292) <br>&emsp; + Verified the home widget on dev with API Gateway logs <br> - Reviewed and approved Nhân's PR #294 (JSON log) <br> - Approved the OWASP ZAP baseline scan plan <br> - Checked Blog 1 before publishing <br> - Read the ZAP results, created issue #299, merged PR #301 <br> - Fixed the recommendations cache on sign-out (PR #303) | 27/10/2026 | 27/10/2026 | [TanStack dependent queries](https://tanstack.com/query/latest/docs/framework/react/guides/dependent-queries) <br> [HTTP API logging](https://docs.aws.amazon.com/apigateway/latest/developerguide/http-api-logging.html) <br> [ZAP baseline scan](https://www.zaproxy.org/docs/docker/baseline-scan/) <br> [AWS penetration testing](https://aws.amazon.com/security/penetration-testing/) <br> [Response headers policies](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/understanding-response-headers-policies.html) <br> [EventBridge Scheduler](https://docs.aws.amazon.com/scheduler/latest/UserGuide/what-is-scheduler.html) |
| 4 | - Worked at the office with Hoàng <br> - Reviewed v0.2.0 progress, ran checklist section 1 early <br> - O7 acceptance on dev (SES emails) <br> - Split security work with Nhân (WAF, C1) <br> - Limited the pre sign-up Lambda's `ListUsers` by `env` tag | 28/10/2026 | 28/10/2026 | [Cognito actions and resources](https://docs.aws.amazon.com/service-authorization/latest/reference/list_cognito-idp.html) <br> [IAM tags](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_tags.html) <br> [SES sending activity](https://docs.aws.amazon.com/ses/latest/dg/monitor-sending-activity.html) |
| 5 | - Merged PR #305 (pre sign-up reads only its `env` pool), checked dev <br> - Checked the WAF proposal against CloudFront flat-rate plans <br> - Decided on pay-as-you-go WAF, prod only <br> - Checked CDK bootstrap in us-east-1 for the WAF stack <br> - Settled the final v0.2.0 scope: R4 deferred (#306, PR #308) | 29/10/2026 | 29/10/2026 | [CloudFront flat-rate plans](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/flat-rate-pricing-plan.html) <br> [IAM Policy Simulator](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_testing-policies.html) <br> [CDK bootstrapping](https://docs.aws.amazon.com/cdk/v2/guide/bootstrapping.html) |

### Task details:

#### Monday (26/10/2026)

* **Reviewed the PRs the team merged over the weekend without review:** order confirmation worker infrastructure (#263), worker SES emails (#265), order E2E fix (#267), Nhân's nightly recommendations job (#269).
  * The worker has narrow IAM permissions; only the worker may publish to the `order-confirmed` topic; concurrency is capped at 2 to fit the SES sandbox limit of 1 email/second.
  * The email code follows the agreed decisions but is not enabled yet, pending the SES and Cognito permissions PR.
  * Found why the #265 deploy failed: E2E expected a `PENDING` order while the worker had already confirmed it; fixed by #267.
* **Reviewed and merged 4 Dependabot PRs:** 17 npm packages (including AWS SDK, CDK, vite), ruff, uv-build, the setup-uv action. Final dev deploy green; verified `uv.lock` and `pnpm-lock.yaml` still match.
* **Prepared Blog 1 on deploying with OIDC (not published yet):**
  * Checked every technical detail against the `github-oidc.yaml` template, the workflow, PR #25, the timed run, GitHub Environments rules and GitHub's docs on immutable subjects.
  * Updated the numbers: 118 successful deploys on `develop`.
  * Corrected the incident section to what I remember and can verify.
  * The illustration was made with AI tool assistance, disclosed in the post.
* **Closed the OIDC review (#180) by accepting the risk:**
  * Re-checked the 5 GitHub safeguards: environments accept only the right branch/tag; `production` needs 1 of 3 reviewers; self-review blocked; admins cannot bypass; only admins create `v*` tags.
  * Fixed ADR-0013 to state that deploy roles still use `cfn-exec-role` with AdministratorAccess; updated threat model C6 and E9 (PR #278, awaiting review).
* **Fixed 2 alarms that fired falsely on staging on 21/10 (PR #280, awaiting review):**
  * `api-latency-p95` uses metric math to count only periods with at least 20 requests.
  * `presignup-errors` subtracts sign-up rejections (counted by a metric filter on the Lambda logs), while still alerting on real errors such as timeouts.
* **v0.2.0 acceptance checklist (PR #282):**
  * Based on the v0.1.0 one: reviewed sprint 2 scope; test tables for ordering, recommendations and security assigned to someone other than the author.
  * Found 4 gaps: SES email not enabled; web not sending events and no recommendation widget; no cancel order button; admin and WAF not started. Asked the team to decide before 30/10.
  * The team settled v0.2.0 scope: admin and WAF move to `v1.0.0-rc.1`, I build the recommendation widget; updated the checklist (PR #286).
  * Corrected my wrong claim about the cancel order button (it already exists on the web).
* **Reviewed 2 PRs Hoàng merged without approval:** worker infrastructure (#263) and email permissions (#284). Checked the SES docs on IAM in the sandbox and accepted `ses:SendEmail` on `identity/*` with an `ses:FromAddress` condition.
* **Verified the worker on dev with the AWS CLI:** all environment variables present; logs only show `not_verified` warnings for E2E orders (no `AccessDenied`, no email addresses); orders queue and DLQ empty.

#### Tuesday (27/10/2026)

* **First real run of the nightly recommendations job on dev (asked by Nhân):**
  * The 02:00 schedule ran once, 0 errors, 4 seconds; no `AccessDeniedException`.
  * The `ACTIVE#` pointers of all 3 recommendation types moved to the new version; bestsellers based on `PURCHASES_30D`.
  * Called the API on dev: bestsellers and "frequently bought together" returned sensible results.
  * Asked Nhân whether prod mixes synthetic events and about logs not yet in JSON.
* **Homepage recommendation widget (PR #289):**
  * Guests see "Đang được mua nhiều", signed-in users see "Dành cho bạn"; the block hides on error or empty results.
  * It waits for the sign-in status before calling the API so the title does not flicker.
  * 5 new tests, tried against the dev API through the Vite proxy; merged and deployed to dev.
* **"Thường mua kèm" widget at the bottom of the product page (PR #292):** excludes items already in the cart with the `exclude` parameter and refetches when the cart changes; hides the block when there are no recommendations. Added Nhân's note on synthetic data to the v0.2.0 release notes.
* **Verified the homepage widget on dev after signing in (API Gateway access logs):** `/recs/for-you` returned 200 (266–414 ms); an account with no history got bestsellers as designed; the widget passes on dev. Noted that `/recs/bestsellers` takes about 1.5 seconds on a Lambda cold start, to optimize after v0.2.0.
* **Reviewed and approved Nhân's PR #294:** the recommendations job now prints a single JSON line for Logs Insights; suggested adding the data check counts to the summary line.
* **Approved the plan for an OWASP ZAP baseline scan (passive only) of the dev site for threat model C3 and L4:** dev only, no AJAX spider, not during a deploy or on the release cut day.
* **Checked Blog 1 before publishing:** the sentence is fixed; the post and diagram do not expose the account ID; screenshots still need the account ID masked.
* **Nhân's ZAP baseline results (0 FAIL, 12 WARN, 56 PASS), checked the dev site headers myself:**
  * The "Server leaks version" warning is only `Server: AmazonS3`.
  * The SRI warning comes from the Google Fonts link.
  * COEP should not be enabled because it would block the fonts.
  * Created issue #299 for a CloudFront response headers policy (L4) after v0.2.0, noting that the CSP must allow Cognito and Google Fonts.
* **Reviewed, approved and merged Nhân's PR #301:** threat model L4 now rates the ZAP warnings correctly; SRI and COEP accepted with reasons.
* **Fixed a bug Hoàng found while reviewing #289 (PR #303):** signing out did not clear the recommendations cache, so the next person signing in on the same device briefly saw the previous user's "Dành cho bạn". Signing out now clears the `recs` cache, with a test that fails before the fix; approved by Hoàng and deployed to dev.

#### Wednesday (28/10/2026)

* **Worked at the office with Hoàng.**
* **Reviewed v0.2.0 progress and ran section 1 of the checklist early:** dev deploy green; no open security alerts (Dependabot, CodeQL, secret scanning all 0); October cost by `env` tag close to zero.
* **O7 acceptance on dev:** read the worker logs and SES sending statistics (5 emails sent, 0 bounces); Hoàng confirmed receiving both the confirmation and cancellation emails.
* **Split security work with Nhân:** Nhân owns WAF end to end, I own C1 (blocking direct `execute-api` calls). Flagged WAF cost while not on the CloudFront Free plan, and that the WebACL must live in us-east-1.
* **`ListUsers` permission of the pre sign-up Lambda (from a reviewed bug list):**
  * The Lambda could call `ListUsers` on every user pool, including prod since all environments share one account.
  * Checked the Service Authorization Reference (the `userpool` resource supports `aws:ResourceTag`) and added an `env` tag condition.
  * Verified the tag on all 4 real user pools; wrote a failing test before the fix; CI green, awaiting Nhân's review.

#### Thursday (29/10/2026)

* **Merged PR #305 after both Hoàng and Nhân approved it:** the pre sign-up Lambda can only call `ListUsers` on the user pool tagged with the same `env`. Checked after the dev deploy:
  * Signing up with a new username works.
  * Invoking the Lambda directly with a test event rejects a duplicate username (`USERNAME_TAKEN`).
  * The IAM Policy Simulator on the real role allows the dev pool and denies the staging and prod pools.
  * This also confirmed that the #280 metric filter counts the rejection and the alarm does not fire falsely.
* **Nhân's WAF proposal:** according to the CloudFront flat-rate pricing plans docs, accounts on the AWS Free Tier cannot subscribe to a plan, so the free WAF approach of ADR-0006 is not possible. Decided on pay-as-you-go WAF on prod only (about $10/month, paid from credits); Nhân will update ADR-0006 in the WAF PR.
* **Checked CDK bootstrap:** `CDKToolkit` exists in us-east-1 and ap-southeast-1 (version 32), and the GitHub deploy roles can assume the us-east-1 roles, enough to put the WAF stack in us-east-1.
* **Settled the final v0.2.0 scope:** web view and add-to-cart events (R4) move to `v1.0.0-rc.1` because Hoàng could not finish in time, tracked in issue #306. Updated the checklist (PR #308) to defer R4 and record O7 as passed on dev.

### Week 7 Achievements:

* Product recommendations working on dev: Nhân's nightly job writes the `recs` table (first real run 27/10, no errors), the recommendations API returns data, and 2 web widgets ("Đang được mua nhiều" / "Dành cho bạn" on the home page, "Thường mua kèm" on the product page). Signing out also clears the previous user's personal recommendations (#303).
* Ordering complete on dev: the worker confirms orders and sends emails through SES; O7 acceptance passed (Hoàng received both the confirmation and cancellation emails).
* Security: closed the OIDC review (#180, risk accepted with GitHub safeguards); fixed 2 false-positive alarms (#280); the pre sign-up Lambda can only read the user pool of its own environment (#305), verified with the IAM Policy Simulator; decided on pay-as-you-go WAF on prod only after finding that a Free Tier account cannot use the CloudFront Free plan.
* Agreed the v0.2.0 scope with the team: admin, WAF, the MFA screen and web events move to `v1.0.0-rc.1`; the acceptance checklist assigns each area to someone who did not build it.
* Not achieved: `release/v0.2.0` was not cut on 30/10 as planned and moved to week 8. Blog 1 was fully reviewed but not published (account ID in screenshots still to be masked).
* Lesson: several PRs were merged without review early in the week; after a reminder, PRs later in the week all waited for approval before merging.
