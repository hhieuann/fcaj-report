---
title: "Week 5 Worklog"
date: 2026-09-14
weight: 5
chapter: false
pre: " <b> 1.5. </b> "
---

### Week 5 Objectives:

* Ship the project's first release `v0.1.0`: cut `release/v0.1.0` (16/10), have the team run acceptance tests on staging, tag and deploy to prod (19/10).
* Coordinate the team: review PRs, route design questions to the right owner, run the walkthrough of the sample module.

### Tasks to be carried out this week:

| Day | Task | Start Date | Completion Date | Reference Material |
|-----|------|------------|-----------------|-------------------|
| 2 | - Reviewed the team's work (#107, #109, #117–#125) <br> - Read the `events`, `recs` designs, routed 3 open questions <br> - Analysed a broken lockfile (#111, #115, #119) <br> - Configured Dependabot (PR #127, #128, #129) <br> - **Monitoring with CloudWatch:** <br>&emsp; + SNS alarm topic (PR #132, #134, #138) <br>&emsp; + 4 alarms, drill on dev (PR #136) <br>&emsp; + Runbooks and dashboard (PR #140, #142) <br> - Raised Lambda concurrency from 10 to 1,000 | 12/10/2026 | 12/10/2026 | [NoSQL design](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/bp-general-nosql-design.html) <br> [pnpm merge conflicts](https://pnpm.io/git#merge-conflicts) <br> [Dependabot options](https://docs.github.com/en/code-security/dependabot/working-with-dependabot/dependabot-options-reference) <br> [CloudWatch](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/WhatIsCloudWatch.html) <br> [SNS email](https://docs.aws.amazon.com/sns/latest/dg/sns-email-notifications.html) <br> [Alarm that sends email](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/AlarmThatSendsEmail.html) <br> [CloudWatch pricing](https://aws.amazon.com/cloudwatch/pricing/) <br> [SNS access policy](https://docs.aws.amazon.com/sns/latest/dg/sns-access-policy-use-cases.html) <br> [Logs Insights](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/AnalyzingLogData.html) <br> [Quota increase](https://docs.aws.amazon.com/servicequotas/latest/userguide/request-quota-increase.html) <br> [CloudWatch dashboards](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/CloudWatch_Dashboards.html) |
| 3 | - Read Hoàng's progress update <br> - Checked the dev environment's E2E configuration on GitHub <br> - Updated the v0.1.0 release checklist (PR #160) <br> - E2E tooling decision: Vitest + fetch instead of Newman | 13/10/2026 | 13/10/2026 | [GitHub releases](https://docs.github.com/en/repositories/releasing-projects-on-github/managing-releases-in-a-repository) <br> [npm audit](https://docs.npmjs.com/cli/commands/npm-audit) <br> [InitiateAuth](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_InitiateAuth.html) |
| 4 | - Nhân confirmed acceptance steps C1–C7 <br> - Agreed a nightly Scan of the `events` table <br> - Proposed a single version pointer for the `recs` table | 14/10/2026 | 14/10/2026 | [DynamoDB TTL](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/TTL.html) <br> [DynamoDB Scan](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Scan.html) |
| 5 | - Reviewed Hoàng's two E2E PRs (#162, #164) <br> - Checked the dev deploy log: 4 E2E files passed <br> - Preconditions for cutting `release/v0.1.0`: all met | 15/10/2026 | 15/10/2026 | [Vitest fileParallelism](https://vitest.dev/config/#fileparallelism) |
| 6 | - Self-study: Cloud Practitioner Essentials, Module 12 <br> - Cloud Adoption Framework, the 7 Rs, Snow Family | 16/10/2026 | 16/10/2026 | [Cloud Practitioner Essentials](https://skillbuilder.aws/learn/94T2BEN85A/aws-cloud-practitioner-essentials/) |
| 7 | - Self-study: Cloud Practitioner Essentials, Module 13 <br> - The 6 pillars of the Well-Architected Framework <br> - Prepared the online team meeting | 17/10/2026 | 17/10/2026 | [Cloud Practitioner Essentials](https://skillbuilder.aws/learn/94T2BEN85A/aws-cloud-practitioner-essentials/) <br> [Well-Architected Framework](https://docs.aws.amazon.com/wellarchitected/latest/framework/welcome.html) |

### Task details:

#### Monday (12/10/2026)

* **Reviewed the team's work after the weekend:**
  * Nhân merged the recommendation and events business docs and the v1 data generator (uv, pytest, the Python CI job now runs) (#107, #109).
  * Hoàng rebuilt the UI in the indigo theme (#117–#125).
* **Nhân's `events` and `recs` table designs:** read the proposal and routed the 3 open questions to their owners (PURCHASE flow to Hoàng; data export and the ACTIVE pointer to Nhân and An); noted that Firehose is not available on the Free plan.
* **Broken lockfile:** analysed the breakage after merging two Dependabot PRs created from the same base (#111, #115); fixed by Hoàng in #119.
* **Dependabot (PR #127):**
  * Learned how it works: schedule, grouped updates, one branch per PR, major vs minor versions.
  * Configured it to skip major updates for npm and uv and to group uv minor/patch updates into one PR.
  * The bot closed the 3 major PRs and opened 2 new ones (#128, #129); merged one after the other and watched both dev deploys: Lambda functions updated, smoke tests 6/6.
* **Learned Amazon CloudWatch:** logs, metrics, alarms, dashboards; looked at real Lambda and API Gateway metrics on dev.
* **Practice: monitoring step 1 (PR #132):** an SNS topic for alarms, HTTPS only, email subscription passed in from a GitHub secret; tested on the sandbox and received the email.
* **Handled two real issues (PR #134):**
  * A malformed email secret made the dev deploy roll back.
  * The email leaked into public CI logs through the resource name CDK derives from it.
  * Fixed by validating the email at synth time and using a fixed subscription ID.
* **Practice: monitoring step 2, CloudWatch alarms (PR #136):** API 5xx and p95 latency, pre sign-up Lambda errors, account-level throttles on prod; notify SNS on alarm and on recovery. Designed around pricing: exactly the 10 free alarm metrics across 3 environments.
* **Lambda limit:** found that the account can only run 10 Lambda functions concurrently (shared by all environments), a risk for the week-6 load test. Requested a Service Quotas increase from 10 to 1,000, approved the same day.
* **Alarm drill on dev with `set-alarm-state`:**
  * The first run showed CloudWatch could not publish to SNS: `enforceSSL` creates a Deny-only topic policy that replaces the default one.
  * Granted `cloudwatch.amazonaws.com` scoped by `aws:SourceAccount` and `aws:SourceArn` (PR #138).
  * The second run delivered both the ALARM and OK emails. Deleted the CI logs that exposed the email.
* **Monitoring step 4: runbooks for the 4 alarms (PR #140):** dry-ran the `api-5xx` runbook on dev with CloudWatch Logs Insights over the API Gateway access logs, and updated it to match the new Console layout.
* **Monitoring step 3: a CloudWatch overview dashboard per environment (PR #142):** alarm status, API requests and errors, p50/p95 latency against the 300 ms target, errors and duration per Lambda, account-wide Lambda concurrency.

#### Tuesday (13/10/2026)

* **Hoàng's update:** Newman E2E collection v1 ready before 15/10; the new product list page (#154, #156, #158) goes into v0.1.0; two merge commits got the wrong titles.
* **Dev environment E2E configuration on GitHub:** checked the variables and secret; told Hoàng the exact variable names the workflow's E2E step passes in.
* **Updated the v0.1.0 release checklist (PR #160):**
  * Scope now includes the new UI and monitoring.
  * Acceptance steps C1, C2, C7 follow the new UI; release note fix.
  * Confirming alert emails and an alarm drill on staging and prod.
* **E2E tooling decision:**
  * Hoàng reported Newman 6.2.3 pulls in High/Critical vulnerabilities through its dependencies.
  * Installed it in isolation and ran `npm audit` to confirm: 11 High, 1 Critical, one without a fix.
  * Compared 3 options and chose Vitest + fetch for E2E, with no repo-wide security exceptions.

#### Wednesday (14/10/2026)

* **Acceptance and event data:** Nhân confirmed the 7 acceptance steps C1–C7 for staging; agreed how the nightly job reads event data: a nightly Scan of the `events` table (Firehose is not available on the Free plan).
* **Version pointer for the `recs` table:**
  * Explained to Nhân how rollback differs between a pointer per anchor and a single pointer for the whole table.
  * Proposed the single pointer, keeping old versions with a TTL for instant rollback; new anchors get the best-seller baseline.

#### Thursday (15/10/2026)

* **Reviewed Hoàng's two E2E PRs (#162 ADR-0018 and docs, #164 code):**
  * API E2E with Vitest + fetch, Cognito sign-in through `InitiateAuth`, avoiding Vite's `BASE_URL` trap.
  * Skipping the suite when variables are missing, sequential runs without retries, cleaning the cart afterwards.
* **Dev deploy log:** the E2E step really ran, 4 test files passed (sign-in, catalog, cart, negative cases) right after the smoke tests.
* **Preconditions for cutting `release/v0.1.0`:** went through them, all met.

#### Friday (16/10/2026)

* **Self-study on AWS Skill Builder:** AWS Cloud Practitioner Essentials, Module 12 - Migrating to the AWS Cloud: the AWS Cloud Adoption Framework (6 perspectives), migration strategies (the 7 Rs), the AWS Snow Family, migration services.

#### Saturday (17/10/2026)

* **Self-study on AWS Skill Builder:** AWS Cloud Practitioner Essentials, Module 13 - Well-Architected Solutions: the 6 pillars of the AWS Well-Architected Framework (operational excellence, security, reliability, performance efficiency, cost optimization, sustainability), related back to the project's monitoring and cost work.
* **Prepared the online team meeting:**
  * Overall status and the Sprint 2 plan.
  * 5 decisions to make: new v0.1.0 release dates, purchase event flow, open threat-model questions, sandboxes for teammates, recommendation model thresholds.

### Week 5 Achievements:

* The project has CloudWatch monitoring: an SNS alarm topic, alarms using exactly the 10 free alarm metrics, runbooks for the 4 alarms, a dashboard per environment; the dev drill delivered both the ALARM and OK emails.
* The account's Lambda concurrency limit was raised from 10 to 1,000 before the week-6 load test.
* Dependabot is configured to skip major updates for npm and uv and to group uv minor/patch updates into one PR.
* The team chose Vitest + fetch instead of Newman for E2E; E2E now really runs after the smoke tests on dev, 4 files passed.
* All preconditions for cutting `release/v0.1.0` are met; the release checklist was updated for the new UI and monitoring.
* Self-studied AWS Cloud Practitioner Essentials on Skill Builder, modules 12–13.
