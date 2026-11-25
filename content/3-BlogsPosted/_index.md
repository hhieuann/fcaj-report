---
title: "Blogs Posted"
date: 2026-09-14
weight: 3
chapter: false
pre: " <b> 3. </b> "
---

The three shared blog posts of the shop-ai team, posted on the **AWS Study Group** Facebook group (by the program rules each team has 3 shared posts). The posts are written in Vietnamese.

### Blog 1: GitHub Actions to AWS with OIDC

**Author:** Nguyễn Hiếu An

**Title:** 🔐 GitHub Actions vào AWS bằng OIDC (GitHub Actions to AWS with OIDC: granting access without storing long-lived access keys)

**Content:** Why a CI/CD workflow needs an identity on AWS, the risk of long-lived access keys stored in GitHub Secrets, and how OIDC lets the workflow get temporary credentials: GitHub issues a token, AWS STS checks the trust policy (repository, branch, environment) and returns the role's credentials. Explains `aud` and `sub`, setting up the provider and role, and the common error when `sub` does not match.

**Link:** https://www.facebook.com/groups/awsstudygroupfcj/permalink/2325503738214585/

### Blog 2: A DynamoDB PITR restore drill

**Author:** Nguyễn Hiếu An

**Title:** 🛟 Bật PITR rồi, bạn đã thử khôi phục bảng DynamoDB chưa? (PITR is on, but have you tried restoring a DynamoDB table?)

**Content:** RPO and RTO; Point-in-Time Recovery always restores to a new table; real numbers from the 09/11/2026 drill on staging (23 = 23 items, a ~9 KB table still took 3–10 minutes, the latest restorable time was 4 min 59 s behind). Three lessons: the `table-exists` waiter only waits about 8 minutes; the new table does not carry over TTL, PITR, deletion protection or tags; pointing the application at the new table runs into IAM and IaC ownership. States clearly what was not drilled (copying data back, full RTO).

**Link:** https://www.facebook.com/groups/awsstudygroupfcj/posts/2334688377296121/

### Blog 3: Amazon Cognito and Lambda triggers for user accounts

**Author:** Nguyễn Minh Hoàng (posted 18/11/2026)

**Title:** 🔑 Đổi mật khẩu rồi mà điện thoại kia vẫn đăng nhập? Kinh nghiệm dùng Amazon Cognito và Lambda trigger (Changed the password but the other phone is still signed in? Lessons from Amazon Cognito and Lambda triggers)

**Content:** A Cognito User Pool handles the hard parts of accounts (password storage, verification codes, lockout after repeated failures, JWTs), while shop-specific rules need Lambda triggers. The Pre sign-up trigger that checks duplicate usernames timed out on cold starts: Cognito waits at most 5 seconds for a trigger, so the team raised memory to 512 MB and set the timeout to exactly 5 seconds; the alarm had to separate real errors from rule-based rejections. Triggers do not guard every door (calling Cognito's API directly bypasses them). Forgot password: user-existence protection, no "check the code only" call, the default email sender's ~50 emails/day. Cognito resets the password but does not sign out other devices, so the team added a Post confirmation trigger calling global sign-out; the catches are that the trigger runs after the password is already changed, and old access tokens keep working until they expire.

**Link:** https://www.facebook.com/groups/awsstudygroupfcj/posts/2342420569856235/
