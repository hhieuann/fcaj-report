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
| 2 | - Cut `release/v0.1.0` from `develop` (3 days later than planned; sent the team new dates: acceptance 19–21/10, tag 22/10)<br>- First staging deploy succeeded: `shop-stg-api` and `shop-stg-web` stacks including alarms and dashboard; 104 demo products; smoke tests read the URL from the outputs<br>- Created the E2E user in the staging user pool and set the `staging` environment variables and secret on GitHub; fixed a password mix-up caused by pasting several commands at once with a single command that generates a random password<br>- Re-ran the staging deploy: smoke 6/6, API E2E 4/4 files passed; sent the staging URL to the team for acceptance testing | 19/10/2026 | 19/10/2026 | https://docs.aws.amazon.com/cognito/latest/developerguide/how-to-create-user-accounts.html<br>https://cli.github.com/manual/gh_secret_set |

### Week 6 Achievements:

* In progress.
