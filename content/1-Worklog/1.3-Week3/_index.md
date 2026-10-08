---
title: "Week 3 Worklog"
date: 2026-09-14
weight: 3
chapter: false
pre: " <b> 1.3. </b> "
---

### Week 3 Objectives:

* Kick off the team project shop-ai (a serverless electronics shop with "frequently bought together" recommendations): finalize the plan, roles and way of working.
* Build the foundation: a GitHub repo with CI; an AWS account ready to deploy with code (CDK) and to deploy automatically without access keys (OIDC).
* Ship the first sample module running on AWS for the team to follow.

### Tasks to be carried out this week:

| Day | Task | Start Date | Completion Date | Reference Material |
|-----|------|------------|-----------------|-------------------|
| 2 | [fill in] | 28/09/2026 | | |
| 3 | [fill in] | 29/09/2026 | | |
| 4 | - Reviewed the team's project plan, assessing cost and scope risks: Amazon Personalize on the Free plan, the cost of an always-on Campaign, load-test scale<br>- Agreed on the plan with the team; wrote the engineering plan from build and test to deploy and operations<br>- Created the public repo `hhieuann/shop-ai`: README, git flow, testing guide, 12 ADRs, issue/PR templates, GitHub Actions workflows<br>- Configured GitHub: rulesets protecting `main`, `develop`, `release/*`; dev/staging/production environments; Dependabot, secret scanning, CodeQL<br>- Studied the modular monolith and reduced Hexagonal architecture (ADR-0009) | 30/09/2026 | 30/09/2026 | https://github.com/hhieuann/shop-ai<br>https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/managing-rulesets/about-rulesets<br>https://www.conventionalcommits.org/ |
| 5 | - Added Hoàng and Nhân to the repo; wrote the roles, tasks and 8-week roadmap for each member; configured CODEOWNERS<br>- Changed the `develop` rule: no approval required, merge when CI is green<br>- Created a GitHub Projects board (Kanban plus a Released column) for the team<br>- Asked the mentor about the Paid plan and workshop grading → decided to build our own recommendation model on Lambda + DynamoDB instead of Amazon Personalize (ADR-0016); the workshop is graded per team<br>- Installed pnpm and AWS CLI v2; signed in to the CLI with `aws login` (temporary session, no access keys)<br>- Added AWS Budgets alerts; ran CDK bootstrap in ap-southeast-1 and us-east-1<br>- Created the OIDC provider and 4 IAM roles for GitHub Actions (deploy dev, staging, prod, and a read-only diff role)<br>- Started the catalog sample module: domain, use case, DynamoDB adapter; integration tests against DynamoDB Local via Testcontainers | 01/10/2026 | 01/10/2026 | https://docs.github.com/en/issues/planning-and-tracking-with-projects<br>https://docs.aws.amazon.com/cli/latest/userguide/<br>https://docs.aws.amazon.com/cost-management/latest/userguide/budgets-managing-costs.html<br>https://docs.aws.amazon.com/cdk/v2/guide/bootstrapping.html<br>https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_providers_create_oidc.html<br>https://node.testcontainers.org/ |
| 6 | [fill in] | 02/10/2026 | | |
| 7 | - Sample module: API errors following RFC 9457, the handler for `GET /api/v1/products/{id}`, the endpoint declared in OpenAPI<br>- Created the AWS CDK app: DynamoDB table, Lambda (Node.js 24, arm64), HTTP API, CloudWatch Logs; security rules checked with cdk-nag; ran `cdk diff` against the sandbox | 03/10/2026 | 03/10/2026 | https://www.rfc-editor.org/rfc/rfc9457<br>https://docs.aws.amazon.com/cdk/v2/guide/home.html<br>https://github.com/cdklabs/cdk-nag |
| Sun | - Deployed the sample module to the `shop-sbx-an` sandbox, loaded 2 sample products, called the API: 200 for an active product, 404 for a discontinued one<br>- Aligned the sample module with Hoàng's OpenAPI v0 contract and table design (`productId` key, rule BR-04)<br>- Resolved a `ConflictException` caused by renaming a path variable in an API Gateway route; documented it in the infrastructure guide<br>- Merged the sample module into `develop` (PR #22); enabled automatic deployment to the dev environment<br>- Fixed the OIDC trust policy for GitHub's new `sub` format with immutable IDs (root cause found via CloudTrail, PR #25) → dev deployment succeeded<br>- Set the convention that the number in a branch name is the issue number, with a CI step that checks it (PR #27, awaiting merge) | 04/10/2026 | 04/10/2026 | https://github.com/hhieuann/shop-ai/pull/22<br>https://github.com/hhieuann/shop-ai/pull/25<br>https://github.com/hhieuann/shop-ai/pull/27 |

### Week 3 Achievements:

* The team has a finalized plan, clear roles (An: platform and DevOps; Hoàng: sales features; Nhân: data, recommendations, security) and an 8-week schedule with milestones v0.1.0 (19/10), v0.2.0 (02/11), v1.0.0 (23/11).
* The repo follows an industry-style process: git flow branches, Conventional Commits, every PR gated by CI (lint, type check, tests, build, secret and dependency scanning), and a task board.
* Avoided billing risk: replaced Amazon Personalize with a self-built model following the mentor's advice; budget alerts at several thresholds.
* Understood and built CI/CD without access keys: GitHub Actions exchanges an OIDC token for a short-lived IAM role, usable only from the right repo and environment.
* The sample module runs on AWS with a layered architecture (domain, application, ports, infra), backed by 16 unit tests, 3 integration tests and 14 infrastructure tests; errors follow RFC 9457.
* All infrastructure is written in CDK: a whole environment can be created or destroyed with one command; every merge into `develop` updates the dev environment automatically.
* Learned from real failures: renaming a route variable makes CloudFormation create the new route before deleting the old one; GitHub changed the OIDC `sub` format for new repos; the AWS CLI on Windows cannot read files containing Vietnamese characters.
