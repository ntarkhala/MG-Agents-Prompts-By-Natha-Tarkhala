---
description: "Create or update a Confluence wiki page for a feature, bug fix, or component. Use this when you need to document changes in Confluence after implementing code."
agent: "agent"
argument-hint: "Jira ticket ID and brief description (e.g. PROJ-123 AccountBanner LWC)"
tools: [atlassian/*]
---
# Create / Update Confluence Page

Create or update a Confluence wiki page for: **$input**

Steps:
1. Search Confluence for any existing page related to this ticket/feature using `confluence_search`
2. If a page exists — read it with `confluence_get_page`, then update it with new information using `confluence_update_page`
3. If no page exists — create a new page using `confluence_create_page` using the appropriate template:
   - **Feature/Story**: Use the Feature template (Overview, Jira Ticket, Usage, Properties, Events, Dependencies, Change History)
   - **Bug Fix**: Use the Bug Fix template (Problem, Root Cause, Solution, Files Changed, Testing)

Always include:
- The Jira ticket ID and a link to the ticket
- Files that were created or modified
- How a QA engineer can test the change

Return the URL of the created or updated page.
