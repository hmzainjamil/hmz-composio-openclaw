# hmz-composio-openclaw

> **Composio + OpenClaw integration** — connecting 250+ business tool integrations to the HMZ AI Claude Code environment.

Part of [claude-ai-system](https://github.com/hmzainjamil/claude-ai-system).

---

## Overview

Composio provides pre-built integrations for 250+ apps (Salesforce, HubSpot, Slack, Notion, GitHub, Gmail, etc.) exposed as tools Claude can use directly. OpenClaw is the AI gateway managing model routing and tool orchestration.

---

## What This Unlocks

| Without Composio | With Composio + OpenClaw |
|---|---|
| Build each integration from scratch | 250+ tools instantly available |
| Manage auth flows manually | OAuth handled automatically |
| Custom API wrappers per service | Standardized tool interface |
| One model at a time | Multi-model routing via OpenClaw |

---

## Connected Integrations

### CRM & Sales
- HubSpot — contacts, deals, pipelines
- Salesforce — leads, opportunities, reports
- Apollo — lead search, enrichment, sequences
- Pipedrive — deals, activities

### Communication
- Gmail — send, search, threads, drafts
- Slack — messages, channels, search
- Outlook — email, calendar
- Telegram — bot messages

### Productivity
- Notion — pages, databases, comments
- Airtable — records, tables, views
- Google Sheets — read, write, append
- Google Calendar — events, scheduling

### Development
- GitHub — repos, issues, PRs, commits
- Linear — issues, projects, cycles
- Jira — tickets, sprints, boards

---

## OpenClaw Bridge

```
~/.openclaw/                     ← OpenClaw config
LaunchAgent: ai.openclaw.gateway ← Always-on bridge (KeepAlive=true)
Port: managed by LaunchAgent
```

OpenClaw routes model calls:
```
Claude Code prompt
→ OpenClaw gateway
→ Route to cheapest/fastest available model
→ Return response
```

---

## Setup

```bash
# Composio auth
composio login
composio add gmail slack notion hubspot

# OpenClaw bridge (already running via LaunchAgent)
launchctl list | grep openclaw
```

---

## Usage in Claude Code

```
"Search my HubSpot for leads in Australia"    → Composio HubSpot tool
"Send Slack message to #agency channel"       → Composio Slack tool
"Create Notion page for new client brief"     → Composio Notion tool
"Find email thread with [client]"             → Composio Gmail tool
```

---

## Full System

[claude-ai-system](https://github.com/hmzainjamil/claude-ai-system) | [hmz-composio](https://github.com/hmzainjamil/hmz-composio) | [hmz-openclaw](https://github.com/hmzainjamil/hmz-openclaw)

---

*HMZ AI Agency — auto-synced daily*
