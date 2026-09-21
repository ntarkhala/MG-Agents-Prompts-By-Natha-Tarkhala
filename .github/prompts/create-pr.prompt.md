---
description: "Create a GitHub pull request for completed code changes. Use this when code is ready and you need to open a PR. Provide the Jira ticket ID and a summary of changes."
agent: "agent"
argument-hint: "Jira ticket ID and summary (e.g. PROJ-123 Added AccountBanner LWC)"
tools: [github/*, execute]
---
# Create GitHub Pull Request

Create a pull request for: **$input**

Steps:
1. Extract the Jira ticket ID and description from the input
2. Create a branch named `feature/TICKET-ID-short-description` (or `bugfix/` for bugs) from `main` or `develop`
3. Push any staged/pending file changes to the new branch
4. Create a pull request with:
   - **Title**: `[TICKET-ID] Short description of the change`
   - **Body**: Use the standard PR template including:
     - Summary of changes
     - Link to Jira ticket
     - List of files changed
     - Acceptance criteria coverage table
     - Salesforce deployment notes
     - Testing checklist
5. Return the full PR URL

> **Note**: This uses the `GITHUB_REPO_TOKEN` (your work GitHub account that has repo access). Ensure this environment variable is set before running.
