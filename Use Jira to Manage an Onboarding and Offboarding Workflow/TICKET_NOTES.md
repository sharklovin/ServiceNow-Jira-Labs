# Ticket Notes - Lab 13: Onboarding and Offboarding Workflow

> These notes cover both workflows run in this lab - the Chiamaka Bello onboarding and the James Okafor offboarding. Each section documents what was done, what decisions were made, and what is worth knowing if you are reproducing this lab or adapting these workflows for a different HR scenario. Written as a real handover document - specific enough that someone else can pick this up without asking questions.

---

## Pre-Lab State

**Project:** Support-1 (SUP)
**Platform:** Jira Service Management (Cloud)
**Existing issues:** None relevant to these workflows
**Trigger:** Two simultaneous HR requests received on 17 April 2026
**Constraint:** Onboarding must be complete before Monday April 14. Offboarding must be complete by end of business today, April 17.

---

## Ticket 1 - Onboarding: SUP1-8

**Title:** New Starter - Chiamaka Bello - Legal Department - Starts Monday April 14.
**Type:** Service request
**Reporter:** Nnamso Mkpong
**Due date:** Apr 11, 2026
**Final status:** Done

---

### Step Notes - Onboarding

---

**Creating the parent issue**

The title format used - Name, Department, Start Date - is deliberate. When you have multiple onboarding tickets in flight at the same time, a title like "New Starter Onboarding" is useless in the queue. A title that includes the person's name and start date tells you immediately who it is and when it needs to be done by before you even open it. This naming convention should be standardised across the team.

Description set to:
> HR request for onboarding. Provision full access before start date.

Short and accurate. The description does not need to list every task because the sub-tasks do that. The description should state the business reason and the primary constraint. The business reason is HR-initiated provisioning. The primary constraint is the start date.

---

**Creating the five sub-tasks**

Each sub-task was created using the Create subtask button from within the parent issue. This is important. If you create a new standalone issue and then try to link it as a child, it does not appear in the Subtasks section with the progress bar. It appears as a linked issue, which is a different thing. The Create subtask button is what creates the parent-child hierarchy.

Sub-tasks in order:

1. SUP1-9 - Create Active Directory account
   - This is always first. You cannot configure M365, add to security groups, or do anything else until the AD account exists. The UPN is what everything else is tied to.

2. SUP1-10 - Assign Microsoft 365 licence and configure email
   - Depends on SUP1-9 being done. The M365 account is provisioned using the same UPN as the AD account (c.bello@company.com). The email address becomes the primary contact for the welcome email.

3. SUP1-11 - Add to Legal shared drive security group
   - The AD account must exist (SUP1-9) before group membership can be assigned. For Legal department users, the security groups are LegalUsers and AllStaff.

4. SUP1-12 - Build and configure laptop (WIN11-CB-01)
   - The device reference WIN11-CB-01 identifies the specific asset. Including the device name in the sub-task title means the asset is traceable from the ticket. If someone asks which machine Chiamaka was given, the sub-task title answers that question without opening any other system.

5. SUP1-13 - Send welcome email with first-day instructions
   - This is always last. You cannot send a welcome email with login credentials until the AD account (SUP1-9) and M365 email (SUP1-10) are confirmed working. Sending it before those steps are done means the new starter gets instructions they cannot act on.

---

**Setting the due date**

Due date set to Apr 11, 2026. Start date is Monday April 14. The three-day buffer is not generous - it is a minimum. If something goes wrong during provisioning on Friday afternoon (laptop build fails, M365 licence is not available, group membership is blocked by a policy), there needs to be time to resolve it before 9am Monday. A due date of April 13 - the Sunday before - is not a due date. It is a hope.

The due date goes on the parent issue, not the individual sub-tasks. The workflow deadline is one date. If you put a due date on each sub-task you create five separate SLA clocks to manage. One parent due date with a progress bar showing how far through the checklist you are is simpler and more accurate.

---

**Moving sub-tasks to Work in Progress**

Each sub-task was opened individually and moved from Open to Work in Progress before any work was done. This is the correct sequence: assign the task, move it to Work in Progress, do the work, add the completion note, move it to Done. Jumping from Open to Done without using Work in Progress loses the audit trail of when work started.

Observed: The parent issue progress bar does not move when sub-tasks are moved to Work in Progress. It only moves when sub-tasks are moved to Done. This is correct behaviour. 0% Done does not mean no work has started - it means no task has been completed yet. The status badges on the sub-tasks (WORK IN PROGRESS) show the current state of the work.

---

**Completion notes on sub-tasks**

Completion note on SUP1-9:
> AD account created. UPN: c.bello@company.com. Groups: LegalUsers, AllStaff

The note was added as an internal note - yellow background, lock icon - not as a reply to the customer. Customer-visible comments on sub-tasks would be visible to Chiamaka if she has portal access. Internal notes are for the support team only.

The note records three things: the action taken (account created), the identifier that matters (UPN), and the configuration applied (groups). These are the three things a technician needs if something does not work on Monday. The UPN tells them where to look in AD. The groups tell them what access should be working. Without this note, troubleshooting on Monday morning starts from zero.

The same pattern was applied to each sub-task - completion note before closing, recorded as an internal note, specific enough to be useful on its own.

---

**Closing the parent issue**

When all five sub-tasks reached Done, the progress bar on SUP1-8 showed 100% Done. The parent issue was then transitioned to Done. The ticket now shows: Done status, due date Apr 11 2026, five sub-tasks all Done and assigned to Nnamso Mkpong, description confirming the HR request context.

SUP1-8 is a complete auditable record of the Chiamaka Bello onboarding. Everything a colleague, manager, or auditor needs to confirm the provisioning was done correctly and on time is in that one ticket.

---

## Ticket 2 - Offboarding: SUP1-15

**Title:** Leaver - James Okafor - Operations - Last Day Today
**Type:** Service request
**Reporter:** Nnamso Mkpong
**Due date:** Apr 17, 2026
**Final status:** Done

---

### Step Notes - Offboarding

---

**Creating the parent issue**

Same title format as the onboarding: Name, Department, Time context. "Last Day Today" in the title is more useful than a specific date because anyone looking at this ticket on the day it was created understands the urgency immediately. A date would require mental arithmetic. "Last Day Today" does not.

Description set to:
> HR request for urgent offboarding. Remove all access immediately.

The word "urgent" in the description reinforces the priority before the ticket is even opened. Anyone triaging the queue who hovers over the description preview sees the urgency signal before they click into the ticket.

---

**Creating the five sub-tasks**

Same process as the onboarding - Create subtask from within the parent issue for each removal step.

Sub-tasks in order:

1. SUP1-21 - Disable Active Directory account
   - This is always first for the same reason it is last in onboarding: the AD account is the root of all other access. Disabling it does not immediately revoke M365 or shared drive access but it prevents new logins from that moment. This is the fastest control to apply.

2. SUP1-22 - Revoke Microsoft 365 licence and forward email to manager
   - Revoking the licence removes access to email, Teams, SharePoint, and all M365 services. Forwarding the email to the manager ensures business continuity - incoming emails are not lost. The manager's email address should be documented in the sub-task completion note.

3. SUP1-23 - Remove from all security groups and shared drives
   - This is the broadest task in the offboarding. Every group membership that was added during onboarding and any subsequent access requests need to be removed. The completion note should list the groups removed, matching the onboarding note that listed the groups added.

4. SUP1-24 - Retrieve laptop and wipe device
   - The physical asset must be collected and wiped. A remote wipe can be initiated through M365 or Intune if the device does not come back to the office, but physical collection is preferred. The device reference should be recorded in the completion note.

5. SUP1-25 - Confirm no active sessions (VPN, remote desktop)
   - This is the final check and it is the one most commonly skipped. An account can be disabled in AD while a VPN session is still active because the session token does not expire immediately. Checking active sessions in the VPN management console and remote desktop logs before closing the ticket is the last line of defence.

---

**Priority escalation to Highest**

All five offboarding sub-tasks were set to Highest priority. The onboarding sub-tasks were Medium. The reasoning:

A new starter who cannot log in on Monday is inconvenienced. That is a problem but it is not a security risk. The account does not exist yet, so there is no exposure.

A leaver who retains active access after their employment ends is a security risk. That is not an inconvenience - it is a potential data breach. The priority setting reflects the risk level, not just the urgency of the deadline.

Note: Changing priority on the parent issue does not cascade to existing sub-tasks. Each sub-task was opened individually and the priority was changed in the Details panel. This is a Jira behaviour that is easy to miss.

---

**Compliance note on the parent issue**

Compliance note added to the description of SUP1-15:
> All access confirmed removed at 16:45. Device collected. No active sessions. HR notified.

This note is on the parent issue rather than a sub-task because it is the summary sign-off for the whole workflow. It answers four questions that matter in a security or compliance review:

- When was access removed? - 16:45
- What about the physical asset? - Device collected
- Were there any residual sessions? - No active sessions confirmed
- Has HR been informed? - HR notified

The note is in the description field rather than the activity section because the description field is the first thing visible when you open the ticket. A compliance reviewer who opens SUP1-15 sees the confirmation immediately, before scrolling to any comments.

---

**Closing the parent issue**

All five sub-tasks at Highest priority were moved to Done. Progress bar showed 100% Done. Status of SUP1-15 was transitioned to Done. Due date confirmed as Apr 17, 2026 - same day as the work was completed.

The complete offboarding record:
- Parent issue: Done
- Due date: Apr 17, 2026 (today, met in full)
- Sub-tasks: All five Done, all at Highest priority, all assigned to Nnamso Mkpong
- Compliance note: Recorded in description with timestamp, device status, session confirmation, HR notification
- Completion time: 16:45 - within business hours

---

## Comparative Notes - Onboarding vs Offboarding

| Element | Onboarding (SUP1-8) | Offboarding (SUP1-15) |
|---|---|---|
| Sub-task order logic | Build up - account first, access last, comms last | Tear down - disable first, confirm last |
| Due date buffer | 3 days before start date | Same day as last day |
| Priority | Medium | Highest |
| Completion note location | Per sub-task | Parent issue description |
| Note content | Configuration details (UPN, groups) | Compliance confirmation (time, device, sessions, HR) |
| Risk of getting it wrong | User cannot work on day one | Residual access - security risk |

---

## Things Worth Knowing Before You Run This Lab

**Create subtask from inside the parent, not as a standalone issue.** The progress bar and the Subtasks section only work for issues created as child tasks of the parent. An issue linked with "is a subtask of" does not appear in the same place and does not contribute to the progress percentage.

**Work in Progress does not move the progress bar.** Only Done does. This is correct. The bar shows completion, not activity. A sub-task that is Work in Progress is in the middle of something. It has not been completed. The bar moves when the work is done.

**Priority on existing sub-tasks must be changed individually.** Changing the parent priority does not cascade. This is a known Jira behaviour. For offboarding workflows where urgency is high, set the priority during sub-task creation rather than changing it afterwards.

**Internal notes vs customer comments.** Completion notes and compliance notes should always be internal notes - lock icon, yellow background. Customer-visible comments go to the portal and are visible to the requester. In an offboarding scenario, you do not want access removal details visible to the person whose access is being removed.

**The compliance note in the description field is more prominent than one in the activity section.** When an auditor opens a ticket, they see the description first. If the compliance note is buried in a comment thread, it takes longer to find. Putting the summary sign-off in the description makes the most important information the first thing anyone reads.

---

*Nnamso Mkpong - Lab 13 - Jira Service Management IT Service Desk Series*
