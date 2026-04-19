# Ticket Notes - Lab 14: Jira Dashboards and Filters

> These notes document every configuration decision made during the lab - gadget by gadget, filter by filter, query by query. They are written as working notes that would help someone reproduce this dashboard from scratch or adapt it for a different project.

---

## Pre-Lab State

**Project:** Support-1 (SUP)
**Platform:** Jira Service Management (Cloud)
**Existing issues:** 12 issues from Labs 12 and 13 - onboarding and offboarding workflows - all in Done status
**Trigger:** Team lead requested a weekly performance dashboard before Friday stand-up
**Constraint:** Dashboard must be readable by someone who has not been in the queue all week

---

## Dashboard Configuration Notes

---

### Dashboard - Weekly Service Desk Review

**Name:** Weekly Service Desk Review

The name was chosen to be descriptive at a glance. In a Jira environment with multiple dashboards, "Dashboard 1" or "Support Dash" is not findable. "Weekly Service Desk Review" tells you the cadence (weekly), the team (service desk), and the purpose (review) before you open it.

**Viewers:** Private (during setup)
**Editors:** Private (during setup)

Both access settings were kept Private during the build phase. This is the correct approach. A dashboard that is shared before the gadgets are validated can mislead the team if a gadget is misconfigured and showing wrong data. The sequence is: build, validate, then share.

In a production environment, Viewers would be set to the service desk team's Jira group so every team member can see the dashboard without needing to be given individual access. Editors would be restricted to the team lead and any senior analysts responsible for maintaining the dashboard.

---

## Gadget Configuration Notes

---

### Gadget 1 - Pie Chart (Priority)

**Purpose:** Show the urgency distribution of all issues in the project

**Configuration:**

| Setting | Value | Decision reason |
|---|---|---|
| Project or Saved Filter | Support-1 | Direct project reference rather than a filter - simpler for a chart that covers all issues |
| Statistic Type | Priority | Priority is the most action-relevant grouping for a stand-up |
| Auto refresh | Every 15 minutes | Stand-ups can run during business hours when tickets are being created and resolved |

**What the data showed:**
- Total Issues: 12
- Medium: 7 (blue slice)
- Highest: 5 (red slice)

The priority split of roughly 58% Medium and 42% Highest is notable. In a healthy service desk, the Highest proportion is usually lower - perhaps 10% to 20% of the total. The 42% figure here reflects the offboarding workflow from Lab 13 where all five sub-tasks were escalated to Highest priority because of the same-day access removal requirement.

**Placement decision:** This gadget was placed top left. It is the first thing the team lead sees and the most action-relevant question - if the Highest slice is growing week over week, that is the signal to escalate staffing or triage processes.

---

### Gadget 2 - Created vs Resolved Chart

**Purpose:** Show weekly throughput - whether the team is keeping pace with incoming volume

**Configuration:**

| Setting | Value | Decision reason |
|---|---|---|
| Project or Saved Filter | Support-1 | Same scope as the priority chart for consistency |
| Period | Daily | Weekly aggregates hide the day-by-day pattern |
| Days Previously | 7 | One week of history - matches the stand-up cadence |
| Collection Operation | Count | Individual day counts, not cumulative totals |
| Display Trend of Unresolved | No | Status pie chart handles this - keeping charts separate keeps each one readable |
| Display Versions | Only major versions | Not relevant for a service desk project but set to avoid chart noise |
| Auto refresh | Every 15 minutes | Consistent with other gadgets |

**What the data showed:**

The chart showed a single spike on 19 April 2026 with 12 issues both created and resolved. All other days in the 7-day window showed zero activity. This pattern is a lab artefact - in production the pattern would be spread across the week with lower daily volumes.

The key metric the chart confirms is that the resolved line (green) matched the created line (orange) exactly. No issues were created and left open. The weekly backlog delta is zero.

In a real environment, watch for:
- A created line that consistently sits above the resolved line - backlog is growing
- A resolved line that spikes on Fridays with a corresponding dip on Mondays - team is cramming resolution before the weekend rather than working steadily
- Days with zero created and zero resolved - potential data logging gap or team capacity issue

---

### Gadget 3 - Filter Results (Open High Priority Issues)

**Purpose:** Name the specific high priority issues that need attention - not just a count but a list

**Filter creation:**

The filter was created in Jira's Filters section before the gadget was added. The JQL used:

```
project = "Support-1" AND priority in (Highest, High) AND status != Done ORDER BY created ASC
```

**JQL clause breakdown:**

| Clause | Effect |
|---|---|
| `project = "Support-1"` | Restricts to the service desk project |
| `priority in (Highest, High)` | Captures both the top two priority levels |
| `status != Done` | Only shows open items - Done issues have been handled |
| `ORDER BY created ASC` | Oldest issues first - the longest-waiting ones appear at the top |

The `ORDER BY created ASC` clause is deliberate. Showing oldest first means the team lead sees the items that have been sitting longest at the top of the list. Sorting by created DESC would show the newest items first, which could hide older unresolved issues that have been waiting the longest.

**Filter name:** Open High Priority Issues

The filter was starred after saving. Starring is necessary for the filter to appear in gadget configuration dropdowns. Unstarred filters can be found via search but starred filters appear at the top of the dropdown and are faster to select.

**Gadget configuration:**

| Setting | Value | Decision reason |
|---|---|---|
| Saved Filter | Open High Priority Issues | The filter created in the previous step |
| Number of results | 10 | Enough to see the full list without scrolling in most cases |
| Columns | Issue Type, Key, Summary, Priority | Minimum columns needed to triage without opening each ticket |

The column selection was deliberately minimal. Adding Status, Assignee, and Created would make the gadget more informative but would also make it wider and harder to read at a glance. The purpose of this gadget is to surface the names of the issues, not to be a full issue list. If the team lead needs more detail they click the issue key.

**Auto refresh:** Disabled on this gadget. The filter results gadget does not have auto refresh by default - it was left off because the filter results are less time-sensitive than the charts. The charts are updated when tickets are resolved. The filter results are read at the start of the stand-up and discussed once.

---

### Gadget 4 - Pie Chart (Status)

**Purpose:** Show queue health at a workflow level - how much is done versus in progress versus waiting

**Configuration:**

| Setting | Value | Decision reason |
|---|---|---|
| Project or Saved Filter | Support-1 | Same project scope for consistency |
| Statistic Type | Status | Workflow state distribution - different from the priority chart |
| Auto refresh | Every 15 minutes | Consistent with other gadgets |

**What the data showed:**
- Total Issues: 12
- Done: 12 (entire pie is blue)

A status chart that is entirely Done is the ideal outcome for a stand-up. It means the team closed everything they opened. In practice this would be unusual and worth noting explicitly in the stand-up summary. It validates the prioritisation and workload management decisions made during the week.

In a live service desk the status chart typically shows:
- A segment of Open issues (new, not yet picked up)
- A segment of In Progress issues (actively being worked)
- A segment of Done issues (resolved this period)
- Potentially a segment of On Hold or Pending (waiting for user response)

The ratio between these segments tells the team lead about queue flow. A large Open segment with a small In Progress segment means the team is not picking up new work fast enough. A large In Progress segment with a small Done segment means work is being picked up but not closed - possible skill gap or escalation bottleneck.

---

## Saved Filter Notes

Three filters were created and visible in the Filters list:

**Filter for SUP**
- Owner: Nnamso Mkpong
- Viewers: Private
- Purpose: General project filter scoped to the SUP project key
- Not used in any gadget - created as a reference filter

**Filter for SUP1**
- Owner: Nnamso Mkpong
- Viewers: Support-1, All roles
- Purpose: Support-1 project filter with team-wide visibility
- The Viewers setting of Support-1 All roles means any user with a role in the Support-1 project can use this filter

**Open High Priority Issues**
- Owner: Nnamso Mkpong
- Viewers: Private
- Starred: Yes (1 person)
- Purpose: The active filter used in the Filter Results gadget
- This filter should be shared (Viewers set to team group) when the dashboard is shared

---

## JQL Query Notes

---

### Query 1 - Issues Resolved This Week

```
project = "Support-1" AND status = Done AND resolved >= 2026-04-13
```

**Result:** 12 issues

**Date clause note:** The date `2026-04-13` is the Monday of the current week. This clause must be updated every Monday to match the start of the new week. One way to avoid this manual update is to use a relative date:

```
project = "Support-1" AND status = Done AND resolved >= startOfWeek()
```

The `startOfWeek()` function in Jira JQL automatically calculates the Monday of the current week. This makes the query reusable without editing the date. The hardcoded date approach was used in this lab to be explicit about the time window being queried.

**Issues returned:**
All 12 issues from Labs 12 and 13. The five offboarding sub-tasks (SUP1-21 through SUP1-25) at Highest priority, the offboarding parent issue (SUP1-15), the five onboarding sub-tasks (SUP1-9 through SUP1-13) at Medium priority, and the onboarding parent issue (SUP1-8).

---

### Query 2 - All Done Issues in the Project

```
project = "Support-1" AND status = Done
```

**Result:** 12 of 12

This query returns all resolved issues without a date filter. The result is the same as Query 1 because all 12 issues were created and resolved this week. In a mature project with months of history, this query would return a much larger number and the comparison between Query 1 and Query 2 would show what percentage of all-time project work was completed this week.

**Using this for trend analysis:**

If you save Query 2 as a filter and track the count week over week, you get the total project completion trend. Comparing the week-over-week delta of Query 2 (total Done count) gives the same weekly resolution figure as Query 1 without needing to update the date. The delta approach works when the project is closed (no new issues being created). For an active service desk where new issues arrive constantly, Query 1 with the explicit date range is more accurate.

---

## Stand-Up Presentation Notes

The five-sentence summary in the README was written with five specific elements:

1. **Volume** - 12 issues handled across two workflows (onboarding and offboarding)
2. **Priority split** - 7 Medium, 5 Highest, with explanation of why the Highest proportion was elevated
3. **Resolution rate** - 100%, all created and resolved same day
4. **Breaches** - none, offboarding completed at 16:45 same day
5. **Recommendation** - two specific focus areas for next week: confirm onboarding success and review whether Highest proportion is a pattern

The recommendation section is the part that most first-line analysts skip. A summary that ends at "everything was resolved" is accurate but not forward-looking. A team lead who has to think about what to focus on next week has not received a complete briefing. The recommendation is what turns a data report into a management briefing.

---

## Things Worth Knowing Before Running This Lab

**Create all filters before adding gadgets.** The Filter Results gadget and the Pie Chart gadget both require a project or saved filter to be specified. If you add the gadgets first and then try to create the filters, you have to go back and edit each gadget after the filter is saved. Build the filters first, then add the gadgets.

**Star your filters.** An unstarred filter is findable via the search box in gadget configuration, but starred filters appear at the top of the dropdown. One extra click every time you configure a gadget is a minor inconvenience in a lab. In production where a dashboard might be reconfigured regularly, starring saves time consistently.

**The Status pie chart only makes sense next to the Priority pie chart.** On its own, the Status chart showing 100% Done is impressive but lacks context. Next to the Priority chart showing 42% Highest, the combination tells a more complete story: the team handled a disproportionate amount of high-urgency work this week and resolved all of it. Neither chart tells that story alone.

**JQL date filters are not automatically updated.** The `resolved >= 2026-04-13` clause in Query 1 is hardcoded. Either update it every Monday or switch to `startOfWeek()` for a self-maintaining query. Document which approach you are using in the filter description field so future maintainers know whether to expect automatic or manual updates.

**Dashboard URLs are shareable.** Once the dashboard is built and shared with the team, the URL in the browser is the shareable link. Anyone with access can bookmark it. The URL does not change when gadgets are added or rearranged. Share the URL at the end of the stand-up so team members can bookmark the dashboard for self-service access during the week.

---

*Nnamso Mkpong - Lab 14 - Jira Service Management IT Service Desk Series*
