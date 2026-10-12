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
| 2 | - Reviewed the team's work after the weekend: Nhân merged the recommendation and events business docs and the v1 data generator (uv, pytest, the Python CI job now runs) (#107, #109); Hoàng rebuilt the UI in the indigo theme (#117–#125)<br>- Read Nhân's proposed `events` and `recs` table designs; routed the 3 open questions to their owners (PURCHASE flow to Hoàng; data export and the ACTIVE pointer to Nhân and An); noted that Firehose is not available on the Free plan<br>- Analysed the lockfile breakage after merging two Dependabot PRs created from the same base (#111, #115), fixed by Hoàng in #119<br>- Learned how Dependabot works (schedule, grouped updates, one branch per PR, major vs minor versions); configured it to skip major updates for npm and uv and to group uv minor/patch updates into one PR (PR #127); the bot closed the 3 major PRs and opened 2 new ones (#128, #129), merged one after the other and watched both dev deploys: Lambda functions updated, smoke tests 6/6 | 12/10/2026 | 12/10/2026 | https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/bp-general-nosql-design.html<br>https://pnpm.io/git#merge-conflicts<br>https://docs.github.com/en/code-security/dependabot/working-with-dependabot/dependabot-options-reference |

### Week 5 Achievements:

* In progress.
