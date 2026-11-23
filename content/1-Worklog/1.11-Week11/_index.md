---
title: "Week 11 Worklog"
date: 2026-09-14
weight: 11
chapter: false
pre: " <b> 1.11. </b> "
---

### Week 11 Objectives:

* Release `v1.0.0` to prod: record acceptance results, merge `release/v1.0.0` into `main`, tag, deploy prod, check the WAF.
* Finish the FCAJ report before the ~29/11 deadline: chapter 5 intro and 5.1, merge 5.4 (Hoàng) and 5.5 (Nhân), Proposal, Events, Blog 3, Self-evaluation, Feedback.
* Review and merge the 2-tab fix (#515) for `v1.0.1`.

### Tasks to be carried out this week:

| Day | Task | Start Date | Completion Date | Reference Material |
|-----|------|------------|-----------------|--------------------|
| 2 | - Worked at the office <br> - Reviewed team progress: PRs, issues, release, report <br> - Reminded Hoàng (5.4), Nhân (5.5 screenshots) of the 24/11 deadline <br> - **`v1.0.0` acceptance on staging:** <br>&emsp; + N2 admin cancels an order, checked stock in DynamoDB <br>&emsp; + N3 forgot password, checked the session-revoke trigger logs <br>&emsp; + N6 sign-in with an old session on the device <br> - Recorded acceptance results, added the SES sandbox limitation (#523) <br> - Looked at Hoàng's fix commit for #515 | 23/11/2026 | 23/11/2026 | [TransactWriteItems](https://docs.aws.amazon.com/amazondynamodb/latest/APIReference/API_TransactWriteItems.html) <br> [AdminUserGlobalSignOut](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_AdminUserGlobalSignOut.html) <br> [Amplify token refresh](https://docs.amplify.aws/react/build-a-backend/auth/connect-your-frontend/manage-user-sessions/) |

### Task details:

#### Monday (23/11/2026)

* **Worked at the office.**
* **Team progress:** `release/v1.0.0` is on staging, not tagged yet; #515 (Hoàng) has a fix commit for the guest cart, CI green; 4 Dependabot PRs (#519 red) wait until after the tag. Report: 5.2, 5.3, 5.6, 5.7 are live; still empty: chapter 5 intro, 5.1, Proposal, Events, Self-evaluation, Feedback, Blog 3; 5.4 is not in the repo yet, 5.5 lacks screenshots. Reminded Hoàng and Nhân of the 24/11 draft deadline.
* **N2, admin cancels a `CONFIRMED` order:** placed order `01M8369G…` (2 items, 17,080,000₫) as a customer, it became `CONFIRMED` within 2 seconds. The admin (with MFA) cancelled it in `/admin`: the confirm dialog showed the right order ID, item count and total; the order became `CANCELLED` and the customer sees "Đã huỷ". Stock read in DynamoDB: 10 → 9 → 10 and 30 → 29 → 30. The first stock read came too late (the order was already placed), so it was cross-checked against the `products.json` seed and the list of orders containing both items.
* **N3, forgot password:** reset with the emailed code, signed in with the new password. Staging Post confirmation trigger logs at 10:24:28 and 10:32:59: "Đã đăng xuất mọi thiết bị sau khi đặt lại mật khẩu", no errors. Lesson: deleting the `accessToken` cookie does not force a token refresh because Amplify treats it as signed out; to see `Refresh Token has been revoked` in the browser, wait for the old access token to expire (1 hour). The "other device" must be a different browser, since Chrome incognito windows share cookies.
* **N6:** all 3 cases passed: F5 on `/orders` does not go to `/login` (#500); signed in as A, signing in as B with a wrong password shows the error and signs A out, the right password gets into B (#507).
* **#523 into `release/v1.0.0`:** recorded N1–N8 (all 8 passed), added "SES is still in sandbox in all 3 environments" to Known limitations, ticked the re-cut steps. Waiting for Hoàng or Nhân to review.
* **#515:** fix commit `8401a92` matches the feedback: a guest tab only reloads and keeps the guest cart; new test added. To be approved after the `v1.0.0` tag.

### Week 11 Achievements:

* `v1.0.0` acceptance on staging passed all 8 cases; results and the SES sandbox limitation are recorded in the checklist (#523, awaiting review).
* Remaining this week: tag and deploy `v1.0.0` to prod, merge #515, the empty report sections before ~29/11.
