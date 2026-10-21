---
title: "Week 6 Worklog"
date: 2026-09-14
weight: 6
chapter: false
pre: " <b> 1.6. </b> "
---

### Week 6 Objectives:

* Finish the `v0.1.0` release: acceptance testing on staging, tagging and deploying to prod.
* Start Sprint 2: infrastructure for the purchase event flow, the HMAC key, the recommendations API.

### Tasks to be carried out this week:

| Day | Task | Start Date | Completion Date | Reference Material |
|-----|------|------------|-----------------|-------------------|
| 2 | - Cut `release/v0.1.0` from `develop` (3 days later than planned; sent the team new dates: acceptance 19–21/10, tag 22/10)<br>- First staging deploy succeeded: `shop-stg-api` and `shop-stg-web` stacks including alarms and dashboard; 104 demo products; smoke tests read the URL from the outputs<br>- Created the E2E user in the staging user pool and set the `staging` environment variables and secret on GitHub; fixed a password mix-up caused by pasting several commands at once with a single command that generates a random password<br>- Re-ran the staging deploy: smoke 6/6, API E2E 4/4 files passed; sent the staging URL to the team for acceptance testing<br>- Reviewed and merged 3 Dependabot PRs (17 npm minor updates, 2 Python tooling updates) and Nhân's events route contract PR into `develop` after the release was cut; checked the Python lockfile still matched; both dev deploys green, smoke 6/6, E2E 4/4 | 19/10/2026 | 19/10/2026 | https://docs.aws.amazon.com/cognito/latest/developerguide/how-to-create-user-accounts.html<br>https://cli.github.com/manual/gh_secret_set |
| 3 | - Self-study on AWS Skill Builder: started **AWS Cloud Quest: Cloud Practitioner**, a role-playing game course where each assignment solves a problem in a virtual city with AWS services, with hands-on labs in a real AWS environment provided by Skill Builder; follows the completed Cloud Practitioner Essentials course | 20/10/2026 | 20/10/2026 | https://aws.amazon.com/training/digital/aws-cloud-quest/ |
| 4 | - Reviewed team progress on the acceptance deadline; asked the team for results and for Sprint 2 status<br>- Verified 5 GitHub safeguards for Nhân's OIDC security review issue (#180): 4 in place, found the production environment still lets admins bypass approval<br>- Reviewed Nhân's synthetic dataset: ran the generator and offline evaluation, found the default catalog's product IDs do not match the shop (0/104), re-ran with the real catalog (102/102): association rules still beat the baseline but by a smaller margin than published<br>- Reviewed and approved Nhân's training data bucket PR (#179): ran tests, synthesized dev and prod through cdk-nag; the dev deploy created the bucket, smoke and E2E green | 21/10/2026 | 21/10/2026 | https://docs.aws.amazon.com/AmazonS3/latest/userguide/bucket-key.html<br>https://docs.github.com/en/actions/managing-workflow-runs-and-deployments/managing-deployments/managing-environments-for-deployment |

### Week 6 Achievements:

* In progress.
