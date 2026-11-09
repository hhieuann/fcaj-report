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
| 2 | - Merged #393 (Hoàng's status in the rc.1 checklist) and #395: k6 script `smoke.js` runs nightly against staging through CloudFront, `tiers.js` measures 50 and 200 virtual users directly against API Gateway, with a place-then-cancel order path when E2E credentials are set. Trial runs: smoke 105 requests, 0 failures, p95 162 ms; tiers with 5 virtual users p95 220 ms<br>- Wrote the runbooks `pitr-restore.md` and `cost-spike.md` (#401). Corrected the plan's "restore to a new table and point back to it": Lambda has no IAM access to the new table, so the needed items must be copied back to the old table<br>- **PITR drill on staging (P3):** restored the `orders` table (23 items) to 5 minutes before the run; the new table became ACTIVE after 3–10 minutes, 23 = 23 items, GSI `byStatus` ACTIVE; the new table has no TTL, PITR or deletion protection, as expected. Deleted the temporary table. Learned that even a 9 KB table takes several minutes, and the `table-exists` waiter only waits about 8 minutes<br>- Checked Hoàng's pre-check of admin AD1–AD3 on dev: deploy `9f9adf0` green, the 2 E2E 401/403 tests passed, the dev `admin` group only contains Hoàng's account. Recorded as a pre-check because the author tested his own feature; the official acceptance is still on staging<br>- Updated the rc.1 checklist (#403): dev deploy green (E2E 46/46), nightly ZAP and Schemathesis green, no High Dependabot or CodeQL alerts, the release ruleset blocks branch deletion, costs 01–09/11 dev $0.022, staging $0.020, prod $0.001; MFA #183 moved to after `v1.0.0`. Sent the team the acceptance schedule for 10–11/11<br>- Received Hoàng's Lighthouse P1 numbers on dev: mobile 99/98/99, largest median LCP 2.11 s, largest CLS 0.043, JS 131–147 KB, all 4 targets met | 09/11/2026 | 09/11/2026 | https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/PointInTimeRecovery.html<br>https://grafana.com/docs/k6/latest/using-k6/scenarios/<br>https://docs.aws.amazon.com/cost-management/latest/userguide/budgets-managing-costs.html |

### Week 9 Achievements:

* In progress.
