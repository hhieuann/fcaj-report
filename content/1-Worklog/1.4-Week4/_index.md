---
title: "Week 4 Worklog"
date: 2026-09-14
weight: 4
chapter: false
pre: " <b> 1.4. </b> "
---

### Week 4 Objectives:

* Put the team's web app on the dev environment on AWS: S3 + CloudFront, with `/api/*` routed to API Gateway (project week-2 milestone).
* Keep the project's dependencies current and safe; review teammates' PRs.
* Understand the program's report and workshop rules, and set up the report site from the template.

### Tasks to be carried out this week:

| Day | Task | Start Date | Completion Date | Reference Material |
|-----|------|------------|-----------------|-------------------|
| 2 | - Updated the team lead's week-1 checklist (PR #33) <br> - Reviewed and merged 5 Dependabot PRs, fixed lockfile conflicts <br> - Upgraded `@vitejs/plugin-react` for Vite 8 (PR #40) <br> - Manually tested the web app against the mock API (Prism) | 05/10/2026 | 05/10/2026 | [Dependabot](https://docs.github.com/en/code-security/dependabot) <br> [React Router upgrading](https://reactrouter.com/upgrading/component-routes) <br> [Vite migration](https://vite.dev/guide/migration) <br> [PR #40](https://github.com/hhieuann/shop-ai/pull/40) |
| 3 | - Self-study: Cloud Practitioner Essentials, Module 10 <br> - CloudWatch, CloudTrail, Trusted Advisor, governance and compliance | 06/10/2026 | 06/10/2026 | [Cloud Practitioner Essentials](https://skillbuilder.aws/learn/94T2BEN85A/aws-cloud-practitioner-essentials/) |
| 4 | - Self-study: Cloud Practitioner Essentials, Module 11 <br> - Pricing models, Pricing Calculator, Budgets, Cost Explorer, support plans | 07/10/2026 | 07/10/2026 | [Cloud Practitioner Essentials](https://skillbuilder.aws/learn/94T2BEN85A/aws-cloud-practitioner-essentials/) |
| 5 | - Fixed sandbox deployment on Windows (PR #64) <br> - **Web stack with CDK:** <br>&emsp; + S3 + CloudFront through OAC, deployed to dev (PR #66) <br>&emsp; + Health endpoint, 60-second product list cache (PR #68) <br> - **Sign-in with Cognito:** <br>&emsp; + User pool, JWT authorizer, `GET /api/v1/me` (PR #70) <br>&emsp; + Pre sign-up, `/config.json`, Amplify v6 web (PR #72, #74) <br>&emsp; + Dedicated E2E app client (PR #83) <br> - Smoke tests, cost allocation tags, demo docs (PR #76, #81) <br> - Reviewed PR #79, wrote ADR-0017 for the cart (PR #85) <br> - Idempotency, X-Ray, authorizer 401 (PR #87, #91, #95) <br> - Read the workshop rules, set up the report site | 08/10/2026 | 08/10/2026 | [S3 through OAC](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/private-content-restricting-access-to-s3.html) <br> [CloudFront Functions](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/cloudfront-functions.html) <br> [Cache key](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/controlling-the-cache-key.html) <br> [FCAJ project rules](https://hcm-rules.awsfcaj.com/3-project/) <br> [Cognito user pools](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-pools.html) <br> [JWT authorizer](https://docs.aws.amazon.com/apigateway/latest/developerguide/http-api-jwt-authorizer.html) <br> [Pre sign-up trigger](https://docs.aws.amazon.com/cognito/latest/developerguide/user-pool-lambda-pre-sign-up.html) <br> [Amplify sign-in](https://docs.amplify.aws/react/build-a-backend/auth/connect-your-frontend/sign-in/) <br> [Vitest env](https://vitest.dev/config/#env) <br> [Cost allocation tags](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/cost-alloc-tags.html) <br> [Powertools Idempotency](https://docs.powertools.aws.dev/lambda/typescript/latest/features/idempotency/) <br> [Powertools Tracer](https://docs.powertools.aws.dev/lambda/typescript/latest/core/tracer/) |
| 6 | - Reviewed Hoàng's two cart PRs (#94, #97) <br> - Checked readiness for cutting `release/v0.1.0` <br> - Smoke tests read the URL from stack outputs (PR #99) <br> - Checked the trust policies of the 3 OIDC deploy roles <br> - Wrote the v0.1.0 release checklist (PR #101) <br> - Fixed a flaky idempotency test on CI (PR #103) <br> - Seeded the 104 demo products on prod (PR #105) | 09/10/2026 | 09/10/2026 | [GitHub environments](https://docs.github.com/en/actions/managing-workflow-runs-and-deployments/managing-deployments/managing-environments-for-deployment) <br> [cdk deploy](https://docs.aws.amazon.com/cdk/v2/guide/ref-cli-cmd-deploy.html) |

### Task details:

#### Monday (05/10/2026)

* **Week-1 checklist:** updated the team lead's week-1 checklist (PR #33).
* **5 Dependabot dependency PRs:** reviewed and merged TypeScript 6, Vite 8, React Router 7 and jsdom 30; resolved `pnpm-lock.yaml` conflicts between the PRs.
* **`@vitejs/plugin-react`:** upgraded to the version that supports Vite 8 (PR #40).
* **Manual test after the React Router 7 upgrade:** ran the web app against the mock API (Prism) through home, list, detail, cart, checkout, login and 404.

#### Tuesday (06/10/2026)

* **Self-study on AWS Skill Builder:** AWS Cloud Practitioner Essentials, Module 10 - Monitoring, Compliance and Governance in the AWS Cloud: Amazon CloudWatch, AWS CloudTrail, AWS Trusted Advisor, governance and compliance.

#### Wednesday (07/10/2026)

* **Self-study on AWS Skill Builder:** AWS Cloud Practitioner Essentials, Module 11 - Pricing and Support: pricing models, AWS Pricing Calculator, AWS Budgets, Cost Explorer, support plans.

#### Thursday (08/10/2026)

* **Fixed sandbox deployment on Windows (PR #64):** the `cp` command in the seed Lambda bundling step; the sandbox now loads 104 demo products automatically.
* **Practice: web stack with AWS CDK (PR #66):**
  * Private S3 bucket read by CloudFront through Origin Access Control.
  * CloudFront Function rewriting React routes to `index.html`.
  * `/api/*` behavior routed to the HTTP API, same domain so no CORS.
  * Deployed to the sandbox, then to dev through GitHub Actions (OIDC).
* **Health and cache (PR #68):** `/api/v1/health` endpoint and a 60-second CloudFront cache for the product list; verified `X-Cache` Miss then Hit.
* **FCAJ report:** read the workshop rules and scoring criteria; set up the report site from the FCAJ template.
* **Practice: sign-in with Amazon Cognito (PR #70):** user pool with email sign-in, optional TOTP MFA, SRP app client without a secret; JWT authorizer for the HTTP API; `GET /api/v1/me` reading identity from the token.
* **Pre sign-up and runtime config (PR #72):** Pre sign-up Lambda trigger rejecting duplicate usernames; CloudFront serves `/config.json` so the web reads its Cognito settings at runtime.
* **Web sign-in (PR #74):** sign-in, sign-up, email confirmation and two-factor step with Amplify v6 (SRP), tokens in Secure cookies; 29 UI tests.
* **Smoke tests (PR #76):** run after every deploy through CloudFront (health, products, 404, 401, web, config.json); handled the BASE_URL name clash with Vite.
* **Cost:** activated the `project`, `env`, `module` cost allocation tags to track cost per project, environment and module.
* **Account and demo-scale business docs (PR #81):** assumed figures, k6 load levels, budget.
* **E2E (PR #83):** dedicated Cognito app client for E2E tests (`USER_PASSWORD_AUTH` flow, dev and staging only); created the E2E user on dev and set the dev environment variables and secret on GitHub.
* **Reviewed Hoàng's PR #79:** the web no longer retries API calls on 4xx errors; merged.
* **ADR-0017 for the cart module (PR #85):** approved Hoàng's architecture proposal:
  * cart only reads the products table with BatchGetItem;
  * ordering only uses UpdateItem inside the order transaction;
  * one idempotency table per module.
* **Shared idempotency helper (PR #87):** built on Powertools for AWS Lambda.
  * Repeating the same `Idempotency-Key` returns the stored result, a different payload returns 422, a request still in progress returns 409.
  * 5 integration tests on DynamoDB Local; handled a gitleaks false positive.
* **Practice: AWS X-Ray with Powertools Tracer (PR #91):**
  * Subsegments for the handler and for each DynamoDB and Cognito call.
  * Cold start, route and status code annotations to filter failing traces.
  * Responses are not recorded in traces to avoid leaking personal data.
* **The JWT authorizer's 401 does not follow RFC 9457 (PR #95):**
  * Tried a CloudFront custom error response, but `cdk synth` showed CloudFront does not accept code 401.
  * Compared Lambda@Edge and verifying the JWT inside Lambda; documented the behavior in the OpenAPI contract and ADR-0007 and added a smoke test.

#### Friday (09/10/2026)

* **Reviewed Hoàng's two server-side cart PRs (#94, #97) against ADR-0017:** the cart Lambda only has `BatchGetItem` on the products table, a dedicated `cart-idempotency` table, every route behind the JWT authorizer.
* **Readiness for cutting `release/v0.1.0`:** `staging` environment (only `release/*` branches), `production` (only `v*` tags, approval by another member), OIDC roles; found that staging and production had no URL for smoke tests.
* **Smoke tests read the URL (PR #99):** the CloudFront URL is read from the stack outputs after each deploy, removing a manual step from the release; verified on dev: smoke tests ran against the URL read from the outputs, 6/6 passed.
* **Trust policies of the three OIDC deploy roles (dev, staging, production):** each role accepts only this repository and its own environment.
* **v0.1.0 release checklist (PR #101):**
  * Scope, branch commands, E2E user for staging.
  * 18 acceptance steps split by person (the tester is never the author).
  * Tagging and prod deploy approval, what to do if the deploy fails.
  * Found that prod has no product data yet.
* **Flaky idempotency test on CI (PR #103):** two requests raced to write the IN_PROGRESS record; the test now waits for the first use case to start before sending the second.
* **Seeded the 104 demo products on prod for the v0.1.0 demo (PR #105):** prod only inserts missing products with a conditional `PutItem` (`attribute_not_exists`), never overwriting stock or prices; the role only has `PutItem`.

### Week 4 Achievements:

* The team's web app runs on dev behind CloudFront with 104 demo products; every merge into `develop` updates both the API and the web.
* Understood protecting an S3 bucket with Origin Access Control: the bucket stays private and only CloudFront can read it.
* Understood CloudFront cache keys: the product list is cached for 60 seconds per query string, other APIs are not cached, and the health endpoint is never cached.
* Learned to handle a batch of dependency PRs: merge one at a time, let Dependabot rebase on lockfile conflicts, and re-check the final combination.
* Lessons learned: shell commands in CDK bundling steps must work on both Windows and Linux; CI synthesizes without an account, so cdk-nag acknowledgement IDs must not depend on it.
* Self-studied AWS Cloud Practitioner Essentials on Skill Builder, modules 10–11: core AWS service knowledge applied in labs and the project.
