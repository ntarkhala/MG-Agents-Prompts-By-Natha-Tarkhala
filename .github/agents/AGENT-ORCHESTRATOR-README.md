# AI Agent Orchestrator Suite

Twelve VS Code Copilot custom agents implementing the hub-and-spoke Orchestrator-Worker pattern.

**Scope:** Workspace (`.github/agents/`) — shared with the repo. Prompts live in `.github/prompts/`.

## How It Works (grounded agents)

The **detailed procedures live in the agents/orchestrator**, not the prompts. Each specialist now has
real tool access via MCP and coordinates with the others:

- **MCP tool grants** — Jira/Confluence/Webex through the `eng-mcp-tool` server; GitHub branches, commits,
  and PRs through the `github` server (`.vscode/mcp.json`). **This workspace has no local git — all VCS
  operations (branch, commit, PR) go through the GitHub MCP, not the `git` CLI.** Agents that had only
  `[read, search, web]` now hold the tools they actually need, so they function as true subagents.
- **Lifecycle** — the **User Story Life Cycle** (status + subtask-status map) is embedded in `jira-agent`
  (the only Jira writer) and driven at each gate by the orchestrator's playbooks.
- **Base branch** — PRs are raised against **N2A** (GitHub, `paychex/Salesforce-COE`). Feature branches:
  `ntarkhala/Feature/{STORY}/{title-kebab}` off N2A.
- **Thin prompts** — `/develop`, `/pr-reviewer`, `/dev-tester` just pass `$input` to the orchestrator,
  which runs the matching playbook (DEVELOP / PR-REVIEW / DEV-TESTER).

## Entry Point

Always invoke **`Orchestrator`** from the chat agent picker. It is the only `user-invocable: true` agent in the suite. All specialists are subagents (`user-invocable: false`) that the Orchestrator delegates to.

```
@Orchestrator Review PROJ-1234 and create a dev test plan
```

## Agent Roster

| # | Agent | File | Model (closest available) | Role |
|---|---|---|---|---|
| 1 | Orchestrator | `orchestrator.agent.md` | Claude Sonnet 4.5 | Single entry point, routes & synthesizes |
| 2 | Jira Agent | `jira-agent.agent.md` | GPT-5 mini | Jira CRUD + subtask validation |
| 3 | Confluence Explorer | `confluence-explorer.agent.md` | GPT-4.1 | Read-only Confluence search |
| 4 | Confluence Writer | `confluence-writer.agent.md` | Claude Sonnet 4.5 | Creates dev test plans & docs |
| 5 | Product Owner | `product-owner.agent.md` | Claude Sonnet 4.5 | Project memory / KB |
| 6 | Senior Software Engineer | `senior-software-engineer.agent.md` | Claude Opus 4.1 | Salesforce code implementation |
| 7 | Solutions Lead | `solutions-lead.agent.md` | Claude Opus 4.1 | Architecture & code review |
| 8 | Git Commit | `git-commit.agent.md` | GPT-4.1 | Branches + Conventional Commits |
| 9 | Pull Request | `pull-request.agent.md` | Claude Sonnet 4.5 | PR creation + Jira sync |
| 10 | PR Review Response | `pr-review-response.agent.md` | Claude Opus 4.1 | Address PR feedback (drafts only) |
| 11 | Timesheet | `timesheet.agent.md` | GPT-5 / Sonnet 4 | Daily Jira + Git + Webex activity rollup |
| 12 | Dev Tester | `dev-tester.agent.md` | Claude Sonnet 4.5 | Designs dev-testing wiki (PO + SL verified) |

## Model Mapping Notes

The original spec referenced models that don't exist in Copilot (Claude 3.7 Sonnet, Claude Opus 4.6, GPT-5.5). Each agent's `model:` frontmatter uses an **array** so Copilot falls back if the primary isn't available in your subscription. Adjust to taste.

**Token / cost optimization** — models are right-sized per role: cheap structured agents (`jira-agent`,
`confluence-explorer`, `git-commit`, `timesheet`) run on GPT-mini/4.1-class; high-reasoning agents
(`senior-software-engineer`, `solutions-lead`, `pr-review-response`) run on Opus/Sonnet-class. The
orchestrator passes each subagent only the slice of context it needs. To trim runtime tokens further, you
can replace a whole-server grant (`eng-mcp-tool/*`) with an explicit list of just the tools an agent uses.

## Side-Effect Discipline

These agents have side effects and **only run on explicit user request** (the Orchestrator enforces this):

- `confluence-writer` — creates/updates wiki pages
- `dev-tester` — publishes dev-testing wiki pages (via confluence-writer, after PO + SL verification)
- `senior-software-engineer` — writes code (and retrieves live metadata from the org)
- `git-commit` — creates branches & commits
- `pull-request` — opens PRs & updates Jira
- `pr-review-response` — proposes code edits (but still requires user approval before applying)

## Routing Cheat Sheet

| User says... | Orchestrator routes to |
|---|---|
| "Review PROJ-1234" | jira-agent (read) |
| "Find docs about Account triggers" | confluence-explorer |
| "Create a dev test plan for PROJ-1234" | jira-agent → dev-tester → confluence-writer |
| "Implement PROJ-5678" | jira-agent + confluence-explorer + product-owner → senior-software-engineer |
| "Review this code" | solutions-lead |
| "Commit these changes for PROJ-1234" | git-commit |
| "Raise a PR for PROJ-1234" | pull-request |
| "Address the PR feedback on #123" | pr-review-response |
| "What did I work on this week?" | timesheet |

## Preview Before Executing (Plan / Dry-Run)

The orchestrator has two modes. To see exactly what it *would* do without touching Jira/GitHub/code,
ask for a plan — e.g. `@Orchestrator plan the develop flow for SFDC-62329` or add "dry run" /
"don't perform any action". It returns the full agent sequence, the Jira transitions, and the Git/PR
operations it would run, then stops for your approval. A normal request runs the playbook and pauses
at each confirmation gate.

## Validation

After creation, verify in VS Code:

1. Open the chat, click the agent picker — only **Orchestrator** should appear (others are subagents).
2. `@Orchestrator` should be selectable.
3. `@Orchestrator what agents are available?` → Orchestrator lists all 11 specialists.

## Workflow Prompts (`/` menu)

Four end-to-end prompt templates in `.github/prompts/` drive the whole suite:

| Prompt | File | Purpose |
|---|---|---|
| `/develop` | `develop.prompt.md` | Story → clarify gaps → implement → PO+SL review → user approval → branch/commits → PR |
| `/dev-tester` | `dev-tester.prompt.md` | Story (+PR/components) → PO+SL-verified dev-testing wiki page |
| `/pr-reviewer` | `pr-reviewer.prompt.md` | Story + PR → structured review (blocking / non-blocking / nit / praise) |
| `/timesheet-filler` | `timesheet-filler.prompt.md` | Mon–Fri timesheet from Jira + Git + Webex |

## Editing

To tune any agent:

- **Behavior** — edit the body of the corresponding `*.agent.md`.
- **Model** — change the `model:` array in frontmatter.
- **Tools** — adjust the `tools:` list (`read`, `search`, `edit`, `execute`, `web`, `todo`, `agent`).
- **Discoverability** — refine the `description:` field. Keywords in `description` are how the Orchestrator decides whether to delegate.

## Removing the Suite

Delete the twelve `*.agent.md` files in `.github/agents/`, the workflow prompts in `.github/prompts/`, and this README.

## Reference

Architecture pattern: hub-and-spoke with unidirectional communication (orchestrator → agents only), dynamic parallel execution, dependency-aware sequencing, lifecycle-driven Jira transitions, and graceful error recovery. Detailed procedures live in the orchestrator's playbooks (DEVELOP / PR-REVIEW / DEV-TESTER) and in each specialist agent — the prompts are thin entry points.
