# Claude SMB Marketplace

This repository is a Claude Code marketplace containing plugins and skills that automate key SMB workflows.

## Aida — AI Business Agent

Aida is an AI business agent and the entry point for the agentic business platform. She acts as a trusted business partner for SMB customers, helping them succeed online through autonomous discovery, strategy, and simulated campaign execution.

**Multi-agent pipeline:**

| Agent | Skill | Role |
|---|---|---|
| Aida (orchestrator) | `aida` | Client intake, briefing, strategy synthesis |
| Site Agent | `site-agent` | Scrapes and analyzes the client's website |
| Competitor Agent | `competitor-agent` | Identifies and researches competitors |
| Social Media Agent | `social-media-agent` | Drafts example social posts (simulation) |
| Paid Agent | `paid-agent` | Drafts example Google Ads (simulation) |
| Listings Agent | `listings-agent` | Proposes Google Business Profile updates (simulation) |
| Campaign Simulator | `campaign-simulator` | Generates realistic campaign results after execution |

**Client data structure:**

```
plugins/aida/clients/
└── [url-slug]/
    ├── meta.md                  ← business name, URL, dates
    ├── memory/
    │   ├── business_profile.md  ← written by site-agent
    │   ├── competitors.md       ← written by competitor-agent
    │   └── marketing_strategy.md ← final deliverable
    └── campaign-results/
        ├── gbp-listing.md       ← written by campaign-simulator
        ├── social-media.md
        └── google-ads.md
```

Each client is isolated by URL slug (e.g. `https://www.example.com` → `example-com`). Aida presents a client picker on startup when multiple clients exist.

---

## Local development

If you're contributing or want to test changes without pushing to GitHub, install from your local clone.

### Prerequisites

- [Claude Code CLI](https://docs.claude.com/en/docs/claude-code) — verify with `claude --version`
- Git

### Setup

Clone this repo, then open a Claude Code session in the repo folder:

```bash
cd <path to this local repo>
claude
```

Register your local clone as a marketplace and install Aida:

```
/plugin marketplace add ./.
/plugin install aida@claude-smb-marketplace
/reload-plugins
```

### Run Aida

```
/aida:aida
```

After making local changes to any skill, reload:

```
/reload-plugins
```

---

## Repository structure

```
.claude-plugin/
└── marketplace.json             ← marketplace manifest

plugins/
└── aida/
    ├── .claude-plugin/
    │   └── plugin.json          ← plugin manifest
    ├── skills/
    │   ├── aida/                ← orchestrator
    │   ├── site-agent/
    │   ├── competitor-agent/
    │   ├── social-media-agent/
    │   ├── paid-agent/
    │   ├── listings-agent/
    │   └── campaign-simulator/
    ├── references/              ← guides and templates used by agents
    └── clients/                 ← per-client memory and campaign data

catalog/
└── plugins.json                 ← plugin catalog for discovery
```
