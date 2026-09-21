---
description: "PR REVIEWER — Review a pull request against its Jira story and return structured, categorized feedback (blocking / non-blocking / nit / praise / question). Reads the story + PR diff, applies Salesforce best practices via the Solutions Lead, drafts review comments (nothing auto-posted), and syncs the PR Review subtask status."
agent: "orchestrator"
argument-hint: "Jira story ID + PR number/URL + instructions (e.g. SFDC-62329 | https://github.com/paychex/Salesforce-COE/pull/13301)"
---
# PR REVIEWER

Run the **PR-REVIEW** workflow for:

**$input**

(First token = Jira story ID. After `|` = PR number or URL. Extra lines = special instructions.)

Follow your **PR-REVIEW playbook** (story ACs → read PR diff → Solutions Lead analysis → categorized draft comments → verdict → Jira sync). Verdict is `changes_requested` if any blocking/big-no item exists. **Nothing auto-posts** — present drafts, post only after approval. End with the standard footer.
