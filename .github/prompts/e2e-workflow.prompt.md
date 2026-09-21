---
description: "Run the complete E2E development workflow: Jira ticket analysis → Salesforce code implementation → Confluence documentation → GitHub pull request. Use this when you want to implement a Jira story end-to-end."
agent: "orchestrator"
argument-hint: "Jira ticket ID (e.g. PROJ-123)"
---
# E2E Development Workflow

Run the full development workflow for Jira ticket: **$input**

Invoke the Dev Orchestrator agent to:
1. Fetch and analyze the Jira ticket `$input` (summary, description, acceptance criteria, comments)
2. Review relevant files in the codebase
3. Implement the required Salesforce code changes (LWC / Apex / Aura / Objects as needed)
4. Create or update the Confluence wiki page for this feature/fix
5. Create a GitHub branch, push the changes, and open a pull request

Show progress after each phase and ask for confirmation if requirements are ambiguous.
