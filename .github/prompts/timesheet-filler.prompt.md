---
description: "TIMESHEET FILLER — Generate the current work-week (Mon–Fri) timesheet by aggregating Jira activity, Git logs, and Webex meeting schedules into concise per-day entries. Read-only."
agent: "orchestrator"
argument-hint: "Optional: week or date range (defaults to current Mon–Fri) and/or Jira project filter"
---
# TIMESHEET FILLER — Weekly Mon–Fri Timesheet

**Input:** `$input` (optional — defaults to the current work week, Monday–Friday)

Build a per-day timesheet by aggregating all activity sources. Delegate through the Orchestrator.

## Data Gathering (parallel reads)
1. `jira-agent` — story transitions, comments (including Gearset comments), status changes, and time logged across the week; map each to its day and story ID.
2. `timesheet` — commits authored, branches, PRs created, and PR reviews via the `github` MCP (no local git); correlate to story IDs via commit messages and branch names.
3. **Webex** — the week's meeting schedule/attendance (via the Webex tools); use real meeting names to replace the generic `MEETINGS` placeholder where available.
4. `timesheet` — synthesize everything into the standard format.

## Output Format
```
TIMESHEET SUMMARY: [Mon date] - [Fri date]

MONDAY (MM/DD/YYYY)
{Meetings} + PROJ-1234 (short activity) + PROJ-5678 (short activity)
... (Tuesday–Friday)
```
- Each activity: 5–7 words max, separated by ` + `.
- Story-linked work prefixed with its ID; non-story work as bare descriptors (e.g. `Sprint Planning`).
- Days with no tracked work → `{Meetings} + No tracked work` (never skip a day).

## Rules
- Read-only. Never modify Jira/Git/Webex state.
- Do not fabricate activity — report only what the sources show.
- Exclude weekends unless explicitly requested.
- End with an **Agents invoked** footer.
