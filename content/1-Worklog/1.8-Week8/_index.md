---
title: "Week 8 Worklog"
date: 2026-09-14
weight: 8
chapter: false
pre: " <b> 1.8. </b> "
---

### Week 8 Objectives:

* Release `v0.2.0` (late from week 7): cut the release branch on 03/11, acceptance testing on staging 03–04/11, tag to prod on 05/11.
* Fix bugs found during acceptance, or record clearly that they move to `v1.0.0-rc.1`.
* Prepare `v1.0.0-rc.1`: WAF (Nhân), C1 and security headers (me), admin page, web events (Hoàng).

### Tasks to be carried out this week:

| Day | Task | Start Date | Completion Date | Reference Material |
|-----|------|------------|-----------------|-------------------|
| 3 | - Reviewed progress after 4 days with no changes: v0.2.0 had not been branched or tagged; proposed a new schedule to the team (tag 05/11, `v1.0.0-rc.1` moved to 12/11)<br>- Merged checklist #308 and cut `release/v0.2.0`; staging deploy green (smoke test, E2E). Checked staging: the email worker has all environment variables, the recommendations schedule is enabled, the queue and DLQ are empty, 3 alarms OK<br>- Order acceptance tests O1–O8 on staging: O1, O2, O3, O4, O6, O8 passed; O5 partly passed (the web does not show the guidance text and link when over 3 orders, #315); O7 had passed on dev; also found that `/orders` does not redirect to sign-in when signed out (#316). Checked the worker logs; cancelled all test orders to restore stock. Recorded results in the checklist (PR #318 into the release branch)<br>- Reviewed Nhân's draft security report (#310); merged #310 and 4 Dependabot PRs into `develop`, dev deploy green<br>- Ran the recommendations job manually on staging so recommendation tests have data; pulled the `PURCHASE` Lambda log for Nhân's S6 check (no personal data in logs) | 03/11/2026 | 03/11/2026 | https://docs.aws.amazon.com/lambda/latest/dg/testing-functions.html<br>https://docs.github.com/en/actions/writing-workflows/choosing-what-your-workflow-does/control-the-concurrency-of-workflows-and-jobs |

### Week 8 Achievements:

* In progress.
