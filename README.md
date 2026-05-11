# hmz-composio-openclaw

![Composio](https://img.shields.io/badge/Composio-250%2B_Apps-blue?style=flat&labelColor=000) ![OpenClaw](https://img.shields.io/badge/OpenClaw-MCP_Gateway-purple?style=flat&labelColor=555) ![Claude](https://img.shields.io/badge/Claude_Code-tool_provider-orange?style=flat&labelColor=555) ![Status](https://img.shields.io/badge/status-production-green?style=flat&labelColor=555)

Composio + OpenClaw unified integration layer — 250+ business tool connectors routed through OpenClaw's cost-optimized model gateway. Every external app (Slack, Notion, Gmail, HubSpot, GitHub, Airtable, and 240+ more) exposed as Claude Code tools with zero-token sub-task routing.

## 🧠 WHAT THIS IS

Two systems working together:

**Composio** — managed API connector platform. Handles OAuth flows, credential storage, rate limiting, and webhook delivery for 250+ business apps. No manual API key wrangling per service.

**OpenClaw** — MCP gateway at `http://127.0.0.1:51827`. Routes Claude tool calls to the right model (Tier 0 first), manages authentication context, and enforces the global model routing mandate.

Together they give Claude Code: one unified tool interface for every business app, zero OAuth friction, and automatic cost routing.

## ⚙️ ARCHITECTURE

```
Claude Code
    ↓
OpenClaw MCP Gateway (port 51827)
    ↓
Composio Integration Hub
    ↓
250+ App Connectors
    ├── Google Workspace (Gmail, Drive, Sheets, Calendar)
    ├── Microsoft 365 (Outlook, Teams, OneDrive, Word)
    ├── Notion / Airtable / Coda
    ├── Slack / Discord
    ├── GitHub / GitLab / Linear
    ├── HubSpot / Salesforce / Apollo
    ├── Shopify / Stripe / PayPal
    └── 240+ more
```

## 💡 CONFIGURATION

```bash
# LaunchAgent — OpenClaw runs permanently
launchctl list | grep openclaw
# ai.openclaw.gateway → always-on, RunAtLoad=true

# Composio auth status
~/.claude/bin/composio-status

# Add new app connector
composio add <app-name>
composio login <app-name>

# View connected apps
composio apps list
```

**MCP config** (`~/.mcp.json`):
```json
{
  "mcpServers": {
    "composio": {
      "command": "composio",
      "args": ["mcp", "--apps", "ALL"]
    }
  }
}
```

## 🔧 CONNECTOR CATEGORIES

| Category | Apps | Use Case |
|---|---|---|
| Productivity | Gmail, Calendar, Drive, Notion | Client communication, scheduling |
| CRM & Sales | HubSpot, Apollo, Salesforce | Lead enrichment, pipeline tracking |
| Dev Tools | GitHub, Linear, Jira, GitLab | Code review, issue tracking |
| Analytics | GA4, Mixpanel, Amplitude | Traffic analysis, funnel data |
| Payments | Stripe, PayPal, QuickBooks | Invoice processing, revenue tracking |
| Social | LinkedIn, Twitter/X, Reddit | Content publishing, monitoring |

## ☠️ WHY NOT INDIVIDUAL MCP SERVERS

Every major tool has its own MCP server (Gmail MCP, Notion MCP, GitHub MCP...). Running 20+ individual MCP servers means: 20 auth configs, 20 credential stores, 20 rate limit handlers, 20 failure modes.

Composio centralizes all of that. One auth flow per app, one connector, one interface. OpenClaw adds the routing intelligence on top.

## 📁 REPO STRUCTURE

```
hmz-composio-openclaw/
├── composio-config/
│   └── connected-apps.json     ← list of authorized app connectors
├── openclaw-routing/
│   └── model-router.js         ← Tier 0/1/2 routing logic
├── mcp-schemas/
│   └── composio-tools.json     ← tool schemas exposed to Claude
└── scripts/
    ├── auth-refresh.sh         ← refresh expiring OAuth tokens
    └── health-check.sh         ← ping all connectors, log failures
```
