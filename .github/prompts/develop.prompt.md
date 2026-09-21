---
description: "DEVELOP — End-to-end Salesforce development for a Jira story: analyze the story + wiki, clarify gaps, implement (org-synced) via the Senior Engineer, get Product Owner + Solutions Lead review, then (after your approval) branch off N2A, commit, raise a PR against N2A, and sync Jira status. Nothing destructive happens without your confirmation."
agent: "orchestrator"
argument-hint: "Jira story ID + optional wiki/PR/components + instructions (e.g. SFDC-62329 | AlertService, customerHealthHubPopover)"
---
# DEVELOP

Run the **DEVELOP** workflow for:

**$input**

(First token = Jira story ID. After `|` or on later lines = relevant wiki links, components, PR, or special instructions.)

Follow your **DEVELOP playbook** (Understand & clarify → set status → implement → PO+SL review → approval gate → branch/commit off N2A → PR against N2A → Jira sync). Stop at each confirmation gate. End with the standard footer.
