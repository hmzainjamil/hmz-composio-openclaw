# hmz-composio-openclaw

> **Composio + OpenClaw integration — 200+ tool connections bridged to Claude Code via OpenClaw gateway**

[![composio](https://img.shields.io/badge/tools-200%2B-blue?style=flat)](.) [![gateway](https://img.shields.io/badge/gateway-OpenClaw-purple?style=flat)](.) [![status](https://img.shields.io/badge/status-production-brightgreen?style=flat)](.)

[Overview](#overview) · [Tools](#tools) · [Integration](#integration) · [Setup](#setup) · [Tips](#tips)

---

## 🧠 OVERVIEW

Composio connects 200+ external tools (GitHub, Slack, Gmail, Linear, Notion, Salesforce, and more) to Claude Code via the OpenClaw gateway. This repo contains the integration layer, connection configs, and action definitions that allow Paperclip AI and Claude agents to act in external systems.

| Component | Value |
|---|---|
| Composio tools | 200+ integrations |
| Gateway | OpenClaw (port managed by LaunchAgent) |
| Auth | OAuth/API key per service via `~/.composio/` |
| CLI | `composio` command |
| MCP | Available as MCP server for Claude Code |

---

## ⚙️ TOOL CATEGORIES

| Category | Tools | Count |
|---|---|---|
| Communication | Gmail, Slack, WhatsApp, Discord | 4 |
| Productivity | Notion, Linear, Jira, Asana, Trello | 5 |
| Development | GitHub, GitLab, Bitbucket | 3 |
| CRM/Sales | HubSpot, Salesforce, Apollo | 3 |
| Marketing | Mailchimp, Klaviyo, Brevo | 3 |
| Analytics | Google Analytics, Mixpanel | 2 |
| Finance | Stripe, QuickBooks | 2 |
| Other | 180+ more available | 180+ |

---

## 🔌 INTEGRATION ARCHITECTURE

```
Claude Code / Paperclip AI
        │
        ▼
   OpenClaw Gateway
        │
        ▼
   Composio MCP Server
        │
   ┌────┴────┐
   ▼         ▼
GitHub     Slack    Gmail    Notion    ...200+ more
```

---

## 💡 TIPS

■ **Connection (4)**
| Tip | Source |
|---|---|
| `composio whoami` to verify active connections | CLI ref |
| `composio add <tool>` to connect new service | CLI ref |
| OAuth tokens stored in `~/.composio/` — never commit this folder | Security |
| Test connection: `composio actions --app github` | Debug |

■ **Usage in Claude (3)**
| Tip | Source |
|---|---|
| Invoke via MCP: tools appear as `mcp__composio__*` | MCP ref |
| Always check auth before long automation chains | Ops rule |
| Composio actions are rate-limited per service — batch where possible | Performance |

---

## ⚠️ GOTCHAS

| Issue | Fix |
|---|---|
| OAuth expired | `composio add <tool>` to re-auth |
| Action not found | `composio actions --app <name>` to list available |
| Rate limit hit | Add delay between actions in automation scripts |
| Gateway not bridging | Restart OpenClaw: `launchctl restart ai.openclaw.gateway` |

---

*Part of [DigiMinds AI Agency Stack](https://github.com/hmzainjamil) — Composio + OpenClaw integration layer*
