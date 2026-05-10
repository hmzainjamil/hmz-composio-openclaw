# hmz-composio-openclaw

> **Composio + OpenClaw** — 250+ business tool integrations connected to the HMZ AI stack, routed through OpenClaw's cost-optimized model gateway.

Part of [claude-ai-system](https://github.com/hmzainjamil/claude-ai-system).

---

## Overview

This repo connects two critical pieces of the HMZ infrastructure:

- **Composio** — pre-built integrations for 250+ business apps (CRM, email, Slack, GitHub, Notion, calendars) with OAuth handled automatically
- **OpenClaw** — AI gateway that routes every tool call to the cheapest capable model, preserving Claude quota for final output

Together they allow Claude Code to interact with any business tool while minimizing token cost.

---

## What Composio Provides

Instead of building API integrations from scratch, Composio provides standardized tool interfaces that Claude can call directly:

```
"Search my HubSpot for leads in Australia"
→ Composio HubSpot tool executes (auth already handled)
→ Returns structured lead list
→ OpenClaw routes through cheapest model
→ Claude formats final response
```

---

## Connected Integrations

### CRM & Sales (7 tools)
| Tool | Available Actions |
|---|---|
| HubSpot | contacts, deals, pipelines, notes, tasks, emails |
| Salesforce | leads, opportunities, accounts, reports, activities |
| Apollo | lead search, enrichment, email sequences, campaigns |
| Pipedrive | deals, activities, persons, organizations, forecasting |
| Close CRM | leads, activities, calls, SMS |
| Zoho CRM | contacts, leads, deals |
| Monday CRM | boards, items, columns |

### Communication (6 tools)
| Tool | Available Actions |
|---|---|
| Gmail | send, search, threads, drafts, labels, attachments |
| Slack | messages, channels, files, search, user lookup |
| Outlook | email, calendar, contacts, tasks |
| Telegram | bot messages, channels, groups |
| WhatsApp Business | messages, templates |
| Intercom | conversations, contacts, notes |

### Productivity (6 tools)
| Tool | Available Actions |
|---|---|
| Notion | pages, databases, comments, search, templates |
| Airtable | records, tables, views, fields, bases |
| Google Sheets | read, write, append, format, pivot |
| Google Calendar | events, scheduling, availability, reminders |
| Trello | boards, cards, lists, checklists |
| ClickUp | tasks, spaces, lists, docs |

### Development (5 tools)
| Tool | Available Actions |
|---|---|
| GitHub | repos, issues, PRs, commits, releases, webhooks |
| Linear | issues, projects, cycles, roadmaps, teams |
| Jira | tickets, sprints, boards, epics, components |
| GitLab | repos, issues, MRs, pipelines |
| Bitbucket | repos, PRs, pipelines |

### Marketing (4 tools)
| Tool | Available Actions |
|---|---|
| Mailchimp | campaigns, audiences, automation |
| ActiveCampaign | contacts, campaigns, automation |
| Klaviyo | flows, campaigns, segments |
| Brevo (Sendinblue) | campaigns, contacts, SMS |

---

## OpenClaw Bridge Architecture

```
~/.openclaw/config.json          ← Gateway configuration
LaunchAgent: ai.openclaw.gateway ← Always-on daemon (KeepAlive=true)

Tool call flow:
Claude Code → OpenClaw gateway → cheapest model → Composio tool → result → Claude
```

The gateway intercepts every tool call and routes through the cost hierarchy before executing the Composio action.

---

## Setup & Authentication

```bash
# Step 1: Authenticate Composio
composio login

# Step 2: Connect tools (OAuth flow opens in browser)
composio add gmail slack notion hubspot airtable github linear

# Step 3: Verify OpenClaw is running
launchctl list | grep openclaw

# Step 4: Test a tool call
# Tell Claude: "List my 5 most recent HubSpot deals"
```

---

## Usage Examples

```
# CRM
"Find all HubSpot contacts added in the last 7 days in Australia"
"Create a HubSpot deal for [company] — $5K, closing this month"
"Update Salesforce lead [name] status to Qualified"

# Communication
"Search Gmail for emails from [client] with invoice in subject"
"Send Slack message to #agency: weekly report ready for review"
"Create Notion page: client brief for [company name]"

# Productivity
"Add event to Google Calendar: call with [client] tomorrow 2pm AEST"
"Create Airtable record in Leads table for new prospect"
"Update Google Sheet 'KPI Dashboard' with this week's numbers"

# Development
"Open GitHub issue: fix conversion tracking on landing page"
"Create Linear ticket: update Apollo email sequences for Q3"
```

---

## Cost Optimization

Every Composio tool call routes through OpenClaw:
- Research-type actions → Groq llama3-70b (free)
- Data extraction → Ollama local (free)
- Complex reasoning → DeepSeek-V3 (~$0.001/1k)
- Final output → Claude Sonnet (only when needed)

---

## Full System

[claude-ai-system](https://github.com/hmzainjamil/claude-ai-system) | [hmz-composio](https://github.com/hmzainjamil/hmz-composio) | [hmz-openclaw](https://github.com/hmzainjamil/hmz-openclaw) | [claude-ai-automations](https://github.com/hmzainjamil/claude-ai-automations)

---

*HMZ AI Agency — auto-synced daily at 6:30 AM*
