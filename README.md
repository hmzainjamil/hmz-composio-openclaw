<div align="center">

# 🔌⚡ HMZ Composio-OpenClaw — Tool Integration Bridge

**Built by [Hafiz Muhammad Zulqarnain](https://github.com/hmzainjamil)**  
*AI Automation Engineer | Claude Code Power User*

[![Part of](https://img.shields.io/badge/Part_of-claude--ai--system-blue?style=flat-square)](https://github.com/hmzainjamil/claude-ai-system)

</div>

---

## What This Is

The bridge between **Composio** (250+ tool integrations) and **OpenClaw** (multi-model AI gateway) — enabling any AI model in the HMZ system to use external tools via Composio's managed authentication layer.

> **This repo**: Docs-only mirror. Source contains API credentials — not published for security.

---

## Architecture

```
User Request
    ↓
OpenClaw Gateway (model selection)
    ↓
Composio Bridge (tool execution)
    ├── Gmail → send emails, create drafts
    ├── GitHub → create repos, open issues
    ├── Slack → send messages, create channels
    ├── Airtable → create records, update CRM
    ├── Google Sheets → write lead data
    ├── LinkedIn → connection requests (via browser)
    └── Apollo → contact enrichment
```

---

## Problem This Solves

Without this bridge:
- Each model needs individual API integrations
- Authentication handled separately per tool
- 250+ possible integrations = enormous setup complexity

With Composio-OpenClaw bridge:
- Any model → any tool via single unified interface
- OAuth/API auth managed by Composio
- Tools available across all 170+ models in the OpenClaw gateway

---

## Key Integrations Enabled

| Composio Tool | OpenClaw Model | HMZ Use Case |
|---|---|---|
| Gmail | Groq Llama 3 | Auto-draft lead reports |
| GitHub | DeepSeek-V3 | Create client repos |
| Airtable | Gemini Flash | Update CRM records |
| Google Sheets | GPT-4o-mini | Log lead data |
| Slack | Ollama | Internal notifications |

---

## Setup

```bash
# Install Composio
pip install composio-claude

# Add tools
composio add gmail github airtable slack

# Configure OpenClaw to use Composio toolset
export COMPOSIO_API_KEY="[YOUR_COMPOSIO_KEY]"
# Get key: composio.dev → Settings → API Keys
```

---

**Part of [claude-ai-system](https://github.com/hmzainjamil/claude-ai-system) — 27 repos, auto-updated daily**
