---
description: "DEV TESTER — Generate a comprehensive Dev Testing wiki page for a Jira story in the CASM standard format. Reads the story + PR/components, designs positive/negative/boundary/regression cases, has the Product Owner and Solutions Lead verify coverage, then publishes via the Confluence Writer and (only if testing is starting/finishing) syncs the Dev Testing subtask status."
agent: "orchestrator"
argument-hint: "Jira story ID + PR link OR component list + instructions (e.g. SFDC-61539 | PR 13301)"
---
# DEV TESTER

Run the **DEV-TESTER** workflow for:

**$input**

(First token = Jira story ID. After `|` = PR link(s) or component list. Extra lines = special instructions.)

Follow your **DEV-TESTER playbook** (gather story + existing page → design `TC-###` cases → PO+SL verify coverage → publish to CASM → sync Dev Testing status only if testing is starting/finishing). Never mark a case Passed without real results. If neither a PR nor components can be resolved, ask before proceeding. End with the standard footer.
