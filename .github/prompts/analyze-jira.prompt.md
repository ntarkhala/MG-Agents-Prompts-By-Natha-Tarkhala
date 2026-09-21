---
description: "Analyze a Jira ticket only — fetch story details, acceptance criteria, description, and comments. Use this when you want to understand what a ticket requires before implementing."
agent: "agent"
argument-hint: "Jira ticket ID (e.g. PROJ-123)"
tools: [atlassian/*]
---
# Analyze Jira Ticket

Fetch the full details for Jira ticket: **$input**

Using the Jira MCP tools:
1. Retrieve the full issue (summary, description, status, priority, assignee, labels)
2. Extract acceptance criteria — look for sections labelled "Acceptance Criteria", "AC", or "Definition of Done" in the description
3. Retrieve all comments in chronological order
4. List any linked issues (blockers, related, duplicates)

Present the result in a clean structured format with clearly separated sections for Description, Acceptance Criteria, and Comments.

If acceptance criteria are not explicitly stated, identify them from the description and clearly mark them as "Inferred AC".
