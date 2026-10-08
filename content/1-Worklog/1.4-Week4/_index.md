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
| 2 | - Updated the team lead's week-1 checklist (PR #33)<br>- Reviewed and merged 5 Dependabot dependency PRs (TypeScript 6, Vite 8, React Router 7, jsdom 30); resolved `pnpm-lock.yaml` conflicts between them<br>- Upgraded `@vitejs/plugin-react` to the version that supports Vite 8 (PR #40)<br>- Manually tested the web app against the mock API (Prism) after the React Router 7 upgrade: home, list, detail, cart, checkout, login, 404 | 05/10/2026 | 05/10/2026 | https://docs.github.com/en/code-security/dependabot<br>https://reactrouter.com/upgrading/component-routes<br>https://vite.dev/guide/migration<br>https://github.com/hhieuann/shop-ai/pull/40 |
| 3 | - Self-study on AWS Skill Builder, AWS Cloud Practitioner Essentials, Module 10 - Monitoring, Compliance and Governance in the AWS Cloud: Amazon CloudWatch, AWS CloudTrail, AWS Trusted Advisor, governance and compliance | 06/10/2026 | 06/10/2026 | https://skillbuilder.aws/learn/94T2BEN85A/aws-cloud-practitioner-essentials/ |
| 4 | - Self-study on AWS Skill Builder, AWS Cloud Practitioner Essentials, Module 11 - Pricing and Support: pricing models, AWS Pricing Calculator, AWS Budgets, Cost Explorer, support plans | 07/10/2026 | 07/10/2026 | https://skillbuilder.aws/learn/94T2BEN85A/aws-cloud-practitioner-essentials/ |
| 5 | - Fixed sandbox deployment on Windows (`cp` in the seed Lambda bundling step, PR #64); the sandbox now loads 104 demo products automatically<br>- **Practice:** web stack with AWS CDK (PR #66):<br>&emsp; + Private S3 bucket read by CloudFront through Origin Access Control<br>&emsp; + CloudFront Function rewriting React routes to `index.html`<br>&emsp; + `/api/*` behavior routed to the HTTP API, same domain so no CORS<br>&emsp; + Deployed to the sandbox, then to dev through GitHub Actions (OIDC)<br>- `/api/v1/health` endpoint and a 60-second CloudFront cache for the product list (PR #68); verified `X-Cache` Miss then Hit<br>- Read the workshop rules and scoring criteria; set up the report site from the FCAJ template<br>- **Practice:** sign-in with Amazon Cognito (PR #70): user pool with email sign-in, optional TOTP MFA, SRP app client without a secret; JWT authorizer for the HTTP API; `GET /api/v1/me` reading identity from the token<br>- Pre sign-up Lambda trigger rejecting duplicate usernames; CloudFront serves `/config.json` so the web reads its Cognito settings at runtime (PR #72)<br>- Web sign-in, sign-up, email confirmation and two-factor step with Amplify v6 (SRP), tokens in Secure cookies; 29 UI tests (PR #74)<br>- Smoke tests after every deploy through CloudFront (health, products, 404, 401, web, config.json); handled the BASE_URL name clash with Vite (PR #76)<br>- Activated the `project`, `env`, `module` cost allocation tags to track cost per project, environment and module<br>- Wrote the account and demo-scale business docs: assumed figures, k6 load levels, budget (PR #81)<br>- Dedicated Cognito app client for E2E tests (`USER_PASSWORD_AUTH` flow, dev and staging only, PR #83); created the E2E user on dev and set the dev environment variables and secret on GitHub<br>- Reviewed and merged Hoàng's PR #79 (the web no longer retries API calls on 4xx errors)<br>- Approved Hoàng's architecture proposal for the cart module and wrote ADR-0017 (PR #85): cart only reads the products table with BatchGetItem, ordering only uses UpdateItem inside the order transaction, one idempotency table per module<br>- Shared idempotency helper built on Powertools for AWS Lambda (PR #87): repeating the same `Idempotency-Key` returns the stored result, a different payload returns 422, a request still in progress returns 409; 5 integration tests on DynamoDB Local; handled a gitleaks false positive | 08/10/2026 | 08/10/2026 | https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/private-content-restricting-access-to-s3.html<br>https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/cloudfront-functions.html<br>https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/controlling-the-cache-key.html<br>https://hcm-rules.awsfcaj.com/3-project/<br>https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-pools.html<br>https://docs.aws.amazon.com/apigateway/latest/developerguide/http-api-jwt-authorizer.html<br>https://docs.aws.amazon.com/cognito/latest/developerguide/user-pool-lambda-pre-sign-up.html<br>https://docs.amplify.aws/react/build-a-backend/auth/connect-your-frontend/sign-in/<br>https://vitest.dev/config/#env<br>https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/cost-alloc-tags.html |

### Week 4 Achievements:

* The team's web app runs on dev behind CloudFront with 104 demo products; every merge into `develop` updates both the API and the web.
* Understood protecting an S3 bucket with Origin Access Control: the bucket stays private and only CloudFront can read it.
* Understood CloudFront cache keys: the product list is cached for 60 seconds per query string, other APIs are not cached, and the health endpoint is never cached.
* Learned to handle a batch of dependency PRs: merge one at a time, let Dependabot rebase on lockfile conflicts, and re-check the final combination.
* Lessons learned: shell commands in CDK bundling steps must work on both Windows and Linux; CI synthesizes without an account, so cdk-nag acknowledgement IDs must not depend on it.
* Self-studied AWS Cloud Practitioner Essentials on Skill Builder, modules 10–11: core AWS service knowledge applied in labs and the project.
