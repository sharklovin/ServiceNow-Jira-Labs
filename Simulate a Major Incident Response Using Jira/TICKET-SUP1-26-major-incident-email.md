# TICKET NOTE - SUP1-26
## MAJOR INCIDENT - Email Service Down - All 300 Users Affected - 09:15

---

**Ticket Key:** SUP1-26
**Type:** Incident
**Priority:** Critical (P1)
**Status:** Completed
**Reporter:** Nnamso Mkpong
**Assignee:** Nnamso Mkpong (Incident Coordinator)
**Project:** Support-1
**Created:** 23 April 2026 - 09:15
**Resolved:** 23 April 2026 - 10:15
**Total Downtime:** 45 minutes

---

## Impact Assessment

| Field | Detail |
|---|---|
| Users Affected | 300 - all users in the organisation |
| Services Affected | Microsoft 365 email (Exchange Online) |
| Business Impact | No user could send or receive email |
| Secondary Impact | Pending communications, delayed workflows, client-facing disruption |
| Incident Category | Identity and Access - Conditional Access Policy Fault |
| Root Cause Type | Administrative change without validation or staged rollout |

---

## Response Team

| Role | Name | Action |
|---|---|---|
| Incident Coordinator | Nnamso Mkpong | Jira ticket management, customer communication, PIR |
| Infrastructure Lead | Maria | Azure AD audit, policy revert |
| Microsoft Escalation | Support ticket raised | Standby - not required after root cause identified |

---

## Chronological Ticket Log

### 09:15 - Incident Raised

Ticket SUP1-26 created as Priority: Critical. Title: MAJOR INCIDENT - Email Service Down - All 300 Users Affected - 09:15. SLA clock started. Issue unassigned on creation pending coordinator confirmation.

---

### 09:20 - Initial Situation Report (Internal Note)

> Email service offline for all users since approximately 09:15. Microsoft 365 status page shows no outage. Initial user reports indicate inability to send or receive emails. Issue may be DNS or MX record related.
>
> Response team assembled:
> - Infrastructure Lead: Maria
> - Microsoft Escalation: Support ticket raised
> - Service Desk Coordinator: Nnamso Mkpong



---

### 09:22 - First Customer Communication (Customer Visible)

> We are aware of an issue affecting email service for all users. Our team is investigating urgently. Next update will be provided within 30 minutes.

**Communication type:** Customer visible reply

---

### 09:30 - Timeline Update: DNS Ruled Out (Internal Note)

> 09:30 - DNS records checked. MX record intact. Exchange Online admin centre shows service healthy. Suspect tenant authentication issue. Checking Azure AD sign-in logs.

**Diagnostic steps completed at this stage:**
- Microsoft 365 status page: No outage
- MX record verification: Intact
- DNS resolution check: Healthy
- Exchange Online admin centre: Service reported healthy
- Next step: Azure AD sign-in log review



---

### 09:45 - Timeline Update: Root Cause Identified (Internal Note)

> 09:45 - Azure AD logs indicate a bulk conditional access policy change applied at 09:10 by an admin account. Policy is blocking Exchange Online access for all users. Reverting the policy now.

**Root cause confirmed:** Conditional access policy change at 09:10 blocked all Exchange Online access tenant-wide.
**Action in progress:** Infrastructure Lead Maria reverting the policy change.



---

### 10:00 - Timeline Update: Service Restored (Internal Note)

> 10:00 - Conditional access policy reverted. Email service restored. User confirmation in progress.



---

### 10:05 - Second Customer Communication (Customer Visible)

> Email service has been restored as of 10:00. If you are still experiencing issues:
> - Restart Outlook
> - Log out and back into Microsoft 365
>
> Please raise a new ticket if the issue persists.

**Communication type:** Customer visible reply


---

### 10:15 - Incident Resolved (Internal Note + Status Change)

**Status changed to:** Completed

> Root cause: Misconfigured conditional access policy applied at 09:10.
> Impact: Blocked Exchange Online access for all users.
> Fix: Policy reverted at 09:58.
> Service restored: 10:00.
> Total downtime: 45 minutes.


---

## SLA Performance Record

| SLA Metric | Target | Result | Status |
|---|---|---|---|
| Time to First Response | Within 6 hours | Met at 09:22 - 7 minutes | Passed |
| Time to Resolution | Within 36 hours | Met at 10:15 - 60 minutes | Passed |
| Time to Close After Resolution | Within 60 hours | Met | Passed |
| First Customer Communication | Within 30 minutes (self-committed) | Delivered at 09:22 | Met |

---

## Comment Audit Trail

| Time | Type | Summary |
|---|---|---|
| 09:20 | Internal note | Initial sitrep - team assembled |
| 09:22 | Customer reply | First user communication |
| 09:30 | Internal note | DNS and MX ruled out |
| 09:45 | Internal note | Root cause identified in Azure AD |
| 10:00 | Internal note | Service restored |
| 10:05 | Customer reply | Restoration confirmed to users |
| 10:15 | Internal note | Formal resolution and closure note |

---

## Resolution Statement

Root cause: A conditional access policy was applied to the tenant at 09:10 by an admin account without validation or staged rollout. The policy was misconfigured to block Exchange Online access for all users. The issue was identified through Azure AD sign-in log analysis at 09:45. The policy was reverted at 09:58 and email service was restored at 10:00. Total downtime was 45 minutes.

---

## Linked Records

| Key | Title | Relationship |
|---|---|---|
| PIR - April 2026 | PIR - Email Major Incident - April 2026 | Post-incident review linked |

---

## Closure Note

Incident resolved and SLA met. Post-incident review opened and linked. Three action items raised with named owners and due dates. No secondary tickets received following the customer restoration update. Incident closed.

**Coordinator sign-off:** Nnamso Mkpong - 23 April 2026 - 10:15
