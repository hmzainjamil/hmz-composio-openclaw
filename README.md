# hmz-composio-openclaw

> **Composio + OpenClaw** — 250+ business tool integrations connected to the HMZ AI stack via OpenClaw's model-routing gateway.

Part of [claude-ai-system](https://github.com/hmzainjamil/claude-ai-system).

---

## What This Provides

Composio gives Claude access to 250+ pre-built integrations (CRM, email, Slack, GitHub, Notion, etc.) with OAuth handled automatically. OpenClaw routes each tool call to the cheapest capable model.

---

## Connected Integrations

### CRM & Sales
| Tool | Actions Available |
|---|---|
| HubSpot | contacts, deals, pipelines, notes |
| Salesforce | leads, opportunities, accounts, reports |
| Apollo | lead search, enrichment, email sequences |
| Pipedrive | deals, activities, forecasting |

### Communication
| Tool | Actions Available |
|---|---|
| Gmail | send, search, threads, drafts, labels |
| Slack | messages, channels, search, files |
| Outlook | email, calendar, contacts |
| Telegram | bot messages, channels |

### Productivity
| Tool | Actions Available |
|---|---|
| Notion | pages, databases, comments, search |
| Airtable | records, tables, views, fields |
| Google Sheets | read, write, append, format |
| Google Calendar | events, scheduling, availability |

### Development
| Tool | Actions Available |
|---|---|
| GitHub | repos, issues, PRs, commits, releases |
| Linear | issues, projects, cycles, roadmaps |
| Jira | tickets, sprints, boards, epics |

---

## OpenClaw Bridge

```
Always-on: LaunchAgent ai.openclaw.gateway (KeepAlive=true)

Tool call flow:
Claude Code → OpenClaw gateway → cheapest model → Composio tool → result
```

---

## Setup

```bash
# Authenticate Composio
composio login

# Connect tools
composio add gmail slack notion hubspot airtable github

# Verify OpenClaw running
launchctl list | grep openclaw
```

---

## Usage in Claude Code

```
"Find all HubSpot contacts in Australia added this month"
"Send Slack message to #agency with weekly report"
"Create Notion page: client brief for [company]"
"Search Gmail for emails from [client] this week"
"Open a GitHub issue for this bug"
```

---

## Full System

[claude-ai-system](https://github.com/hmzainjamil/claude-ai-system) | [hmz-composio](https://github.com/hmzainjamil/hmz-composio) | [hmz-openclaw](https://github.com/hmzainjamil/hmz-openclaw)

*HMZ AI Agency — auto-synced daily*
