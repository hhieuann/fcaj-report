---
title: "Blogs Posted"
date: 2026-09-14
weight: 3
chapter: false
pre: " <b> 3. </b> "
---

Posts on the **AWS Study Group** Facebook group (at least 3 posts). The posts are written in Vietnamese.

### Blog 1: GitHub Actions to AWS with OIDC

**Title:** 🔐 GitHub Actions vào AWS bằng OIDC (GitHub Actions to AWS with OIDC: granting access without storing long-lived access keys)

**Content:** Why a CI/CD workflow needs an identity on AWS, the risk of long-lived access keys stored in GitHub Secrets, and how OIDC lets the workflow get temporary credentials: GitHub issues a token, AWS STS checks the trust policy (repository, branch, environment) and returns the role's credentials. Explains `aud` and `sub`, setting up the provider and role, and the common error when `sub` does not match.

**Link:** https://www.facebook.com/groups/awsstudygroupfcj/permalink/2325503738214585/

### Blog 2: A DynamoDB PITR restore drill

**Title:** 🛟 Bật PITR rồi, bạn đã thử khôi phục bảng DynamoDB chưa? (PITR is on, but have you tried restoring a DynamoDB table?)

**Content:** RPO and RTO; Point-in-Time Recovery always restores to a new table; real numbers from the 09/11/2026 drill on staging (23 = 23 items, a ~9 KB table still took 3–10 minutes, the latest restorable time was 4 min 59 s behind). Three lessons: the `table-exists` waiter only waits about 8 minutes; the new table does not carry over TTL, PITR, deletion protection or tags; pointing the application at the new table runs into IAM and IaC ownership. States clearly what was not drilled (copying data back, full RTO).

**Link:** https://www.facebook.com/groups/awsstudygroupfcj/posts/2334688377296121/

### Blog 3

In preparation.
