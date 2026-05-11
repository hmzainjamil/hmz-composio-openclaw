# hmz-composio-openclaw

> **Composio × OpenClaw integration — 200+ external tool connections bridged into Claude Code agents and Paperclip AI automation via OpenClaw gateway**

[![composio](https://img.shields.io/badge/tools-200%2B_integrations-blue?style=flat)](.) [![gateway](https://img.shields.io/badge/gateway-OpenClaw-purple?style=flat)](.) [![auth](https://img.shields.io/badge/auth-oauth_per_service-green?style=flat)](.) [![status](https://img.shields.io/badge/status-production-brightgreen?style=flat)](.)

[Overview](#overview) · [Architecture](#architecture) · [Tool Catalog](#tool-catalog) · [Setup](#setup) · [CLI](#cli) · [Tips](#tips) · [Gotchas](#gotchas)

---

## 🧠 OVERVIEW

The integration layer that connects Composio's 200+ external tool connections to Claude Code and Paperclip AI through the OpenClaw gateway. When an agent needs to send a Slack message, create a GitHub issue, update a Notion page, or enrich a lead in Apollo — it calls Composio through the bridge. All OAuth tokens are managed by Composio — no auth code in agent scripts.

| Component | Value |
|---|---|
| Composio CLI | `composio` (installed globally) |
| Config | `~/.composio/` (OAuth tokens — never commit) |
| OpenClaw bridge | `~/.claude/bin/openclaw-bridge` |
| MCP tools | Appear as `mcp__composio__*` in Claude sessions |
| Paperclip integration | All external actions routed via Composio |
| Auth method | OAuth (per service) or API key |
| Token storage | `~/.composio/` — encrypted |

---

## ⚙️ ARCHITECTURE

```
Claude Code / Paperclip AI
        │
        ▼
   openclaw-bridge  ←→  ~/.claude/bin/openclaw-bridge
        │
        ▼
   Composio SDK (Python / CLI)
        │
        ├─► GitHub (issues, PRs, commits)
        ├─► Slack (messages, channels, users)
        ├─► Gmail (read, send, draft)
        ├─► Notion (pages, databases)
        ├─► Apollo (contacts, sequences)
        ├─► HubSpot (contacts, deals, sequences)
        ├─► LinkedIn (profile, posts)
        ├─► Stripe (invoices, customers)
        └─► ... 190+ more
```

| Integration Layer | Component | Auth |
|---|---|---|
| Bridge | openclaw-bridge script | N/A |
| Composio SDK | Python package + CLI | Session token |
| Per-service | OAuth/API key per app | ~/.composio/ |
| MCP layer | mcp__composio__* tools | Inherited |

---

## 📦 TOOL CATALOG

### Communication
| Service | Key Actions | Use Case in DigiMinds |
|---|---|---|
| Slack | send_message, create_channel, invite_user | Team alerts, client notifications |
| Gmail | read_thread, send_email, create_draft | Cold outreach, client comms |
| Discord | send_message, create_webhook | Community notifications |

### Productivity
| Service | Key Actions | Use Case in DigiMinds |
|---|---|---|
| Notion | create_page, update_database, add_block | Client wikis, SOP storage |
| Linear | create_issue, update_status, assign | Engineering task tracking |
| Jira | create_issue, transition_issue | Client project management |
| Asana | create_task, update_project | Agency project boards |

### Development
| Service | Key Actions | Use Case in DigiMinds |
|---|---|---|
| GitHub | create_issue, create_pr, push_file | Code, automation repos |
| GitLab | create_mr, create_issue | Client code repos |

### CRM / Sales
| Service | Key Actions | Use Case in DigiMinds |
|---|---|---|
| Apollo | search_contacts, enrich_contact, add_to_sequence | Lead gen, outreach |
| HubSpot | create_contact, update_deal, enroll_sequence | CRM management |
| Salesforce | create_lead, update_opportunity | Enterprise clients |

### Marketing
| Service | Key Actions | Use Case in DigiMinds |
|---|---|---|
| Mailchimp | add_subscriber, send_campaign | Email newsletters |
| Klaviyo | create_profile, add_to_list | E-commerce email |

### Finance
| Service | Key Actions | Use Case in DigiMinds |
|---|---|---|
| Stripe | create_invoice, list_customers, charge | Client billing |
| QuickBooks | create_invoice, record_payment | Bookkeeping |

---

## 🔧 SETUP

```bash
# Install Composio
pip install composio-core composio-openai

# Login
composio login

# Connect a service
composio add github
composio add slack
composio add gmail

# Verify connections
composio whoami

# List connected apps
composio apps --connected

# Test a connection
composio actions --app slack
```

---

## 💻 CLI REFERENCE

```bash
# List all available apps (200+)
composio apps

# List actions for a specific app
composio actions --app github

# Execute an action directly
composio execute --action GITHUB_CREATE_ISSUE   --params '{"repo":"hmzainjamil/claude-ai-system","title":"Test issue","body":"Testing Composio"}'

# View active connections
composio connections

# Disconnect a service
composio remove github

# Re-authenticate expired token
composio add github --reauth

# Composio in Python script
python3 << 'EOF'
from composio_openai import ComposioToolSet, App
toolset = ComposioToolSet()
tools = toolset.get_tools(apps=[App.GITHUB, App.SLACK])
print(f"Loaded {len(tools)} tools")
EOF
```

---

## 🔌 OPENCLAW BRIDGE USAGE

```bash
# Send Slack message via bridge
~/.claude/bin/openclaw-bridge   --tool composio   --action SLACK_SEND_MESSAGE   --params '{"channel":"#general","text":"Lead engine completed: 12 hot leads"}'

# Create GitHub issue via bridge
~/.claude/bin/openclaw-bridge   --tool composio   --action GITHUB_CREATE_ISSUE   --params '{"repo":"hmzainjamil/claude-ai-system","title":"Auto-sync fix needed"}'
```

---

## 💡 TIPS

■ **Connection Management (5)**
| Tip | Source |
|---|---|
| `~/.composio/` contains OAuth tokens — NEVER commit this directory | Security rule |
| OAuth tokens expire (usually 60-90 days) — `composio add <app> --reauth` | Maintenance |
| Test each connection before long automation chains | Pre-flight check |
| `composio whoami` shows all active authenticated services at once | Debug |
| Service disconnects silently — always wrap API calls with error handling | Reliability |

■ **Usage in Agents (5)**
| Tip | Source |
|---|---|
| All Composio tools available as MCP (`mcp__composio__*`) in Claude sessions | MCP ref |
| Batch multiple actions in one agent turn — fewer round-trips | Performance |
| Each service has rate limits — add 1s sleep between batch operations | Rate limits |
| Composio logs all actions — use `/var/log/composio` for audit trail | Audit |
| Prefer Composio over raw API calls — handles retry, auth, versioning | Best practice |

■ **Integration with Paperclip (4)**
| Tip | Source |
|---|---|
| All Paperclip external actions route through openclaw-bridge → Composio | Architecture |
| Lead enrichment uses Apollo via Composio — not direct API calls | Design |
| Slack alerts from CEO loop use Composio SLACK_SEND_MESSAGE | CEO loop |
| GitHub portfolio sync uses GitHub API directly (not Composio) for speed | Exception |

■ **Debugging (4)**
| Tip | Source |
|---|---|
| 401 error → token expired → `composio add <app>` | Auth debug |
| Action not found → `composio actions --app <name>` to see available | Discovery |
| Rate limit → add `time.sleep(2)` between calls | Rate limit fix |
| Bridge not connecting → restart OpenClaw: `launchctl restart ai.openclaw.gateway` | Gateway debug |

---

## ☠️ TOOLS REPLACED

| Composio × OpenClaw | Replaced |
|---|---|
| Unified OAuth for 200+ services | Building OAuth per service (2-4h per integration) |
| `composio add <app>` connection | Custom auth flows, token refresh code |
| Standardized action API | Different API shapes for every service |
| OpenClaw bridge persistence | Re-initializing connections per script run |
| MCP-native tool access | Writing custom MCP servers per integration |
| Composio action logging | No audit trail on external agent actions |
| Token refresh handling | OAuth refresh failures crashing automations |
| Rate limit handling | 429 errors crashing production workflows |

---

## ⚠️ GOTCHAS

| Issue | Fix |
|---|---|
| Auth expired mid-workflow | `composio add <service>` to re-auth; add expiry alerts |
| Action not found | `composio actions --app <name>` — service may have renamed it |
| Rate limit (429) | Add `time.sleep(1-2)` between calls; check service limits |
| Gateway not bridging | `launchctl restart ai.openclaw.gateway` |
| MCP tools not showing | Restart Claude Code session to reload MCP |
| ~/.composio/ accidentally committed | Rotate all tokens immediately + add to .gitignore |
| Composio version mismatch | `pip install --upgrade composio-core` |
| Action params wrong shape | `composio actions --app <name> --action <action>` for schema |

---

## 🚀 QUICK REFERENCE

```bash
# Full setup check
composio whoami && composio apps --connected

# Add new service
composio add <service-name>

# Test action
composio execute --action SLACK_SEND_MESSAGE --params '{"channel":"#test","text":"test"}'

# View all 200+ apps
composio apps | grep -i <keyword>

# Bridge status
launchctl list | grep openclaw

# Logs
tail -f ~/.openclaw/logs/bridge.log
```

---

*Part of [DigiMinds AI Agency Stack](https://github.com/hmzainjamil) — Composio × OpenClaw integration layer | 200+ tool connections for autonomous agency operations*
