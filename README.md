# GencooAI Agent Skills

AI marketing-analytics skills for the YouCloud ecosystem — let your agent directly call the ad-creative and campaign-strategy analysis capabilities of AppGrowing Global, AppGrowing, youmiyoushu, and CreativeHub.

![skills](https://img.shields.io/badge/skills-4-blue) ![auth](https://img.shields.io/badge/auth-YOUCLOUD__API__KEY-orange) ![agent](https://img.shields.io/badge/agent-openclaw-purple)

[English](README.md) | [简体中文](README-CN.md)

```
   TRIGGER         VERIFY          MODE           CALL          DELIVER
 ┌───────────┐   ┌───────────┐   ┌───────────┐   ┌───────────┐   ┌───────────┐
 │    Ask    │──▶│ YOUCLOUD  │──▶│Exploration│──▶│  Backend  │──▶│ Strategy  │
 │ slash cmd │   │  API Key  │   │    or     │   │    AI     │   │  report   │
 │           │   │           │   │Inspiration│   │ wait ≤600s│   │  or ideas │
 └───────────┘   └───────────┘   └───────────┘   └───────────┘   └───────────┘
   /aggclaw       required       chat_mode       one reply      full result
```

## Commands

| What you want to do | Command | Skill | Data source |
|---|---|---|---|
| Analyze **global** ad creatives, auto-detect Game / Non-game / Inspiration | `/aggclaw` | aggclaw | AppGrowing Global |
| Global **game** creative analysis | `/aggclaw-game` | aggclaw | AppGrowing Global |
| Global non-game ad creative analysis | `/aggclaw-app`, `/aggclaw-shortdrama` | aggclaw | AppGrowing Global |
| Analyze **global** ad creatives in Inspiration Mode | `/agg_inspire` | aggclaw | AppGrowing Global |
| Analyze China ad creatives and campaign trends, auto mode | `/agclaw` | agclaw | AppGrowing |
| China creatives in Agentic Exploration mode | `/ag` | agclaw | AppGrowing |
| China creatives in Inspiration Mode | `/ag-inspire` | agclaw | AppGrowing |
| China e-commerce ad & marketing analysis, auto mode | `/youclaw`, `/youmiyun` | youclaw | youmiyoushu |
| China e-commerce analysis in Agentic Exploration mode | `/creative-chat` | youclaw | youmiyoushu |
| China e-commerce analysis in Inspiration Mode | `/grill`, `/grill-chat`, `/grill-youclaw` | youclaw | youmiyoushu |
| CreativeHub strategy analysis (supports @ scope tags) | `/cmclaw` | cmclaw | CreativeHub |

## Quick Start

### Option 1: Install by natural-language instruction (recommended)

Skip the commands — just tell your AI agent:

```
Install the aggclaw skill from the GitHub repo gencooai/skills to this machine,
then tell me how to configure YOUCLOUD_API_KEY.
```

Agents with file access (Claude Code, Codex, WorkBuddy, etc.) will clone the repo, drop the skill folder into their skills directory, and load it. Name multiple skills at once (aggclaw / agclaw / youclaw / cmclaw).

### Option 2: Manual install

Download the repo first:

```bash
git clone https://github.com/gencooai/skills.git
```

Then pick your platform:

**Claude Code**

```bash
cp -r skills/aggclaw ~/.claude/skills/
```

**Codex CLI**

```bash
cp -r skills/aggclaw ~/.codex/skills/
```

**WorkBuddy**

```bash
cp -r skills/aggclaw ~/.workbuddy/skills/
```

**QwenWork (千问办公)**

Upload via the UI: left sidebar **Extensions (扩展) → Skills (技能) → Install Skill (安装技能)**, then upload the `SKILL.md` and the files under `references/` from the skill folder. They are recognized and loaded automatically.

### Configure your API Key

See "Get an API Key" below for how to obtain one. Then set the environment variable for your OS:

**Linux / macOS**

```bash
export YOUCLOUD_API_KEY="your-key-here"   # current terminal session only; add to ~/.zshrc / ~/.bashrc to persist
```

**Windows PowerShell**

```powershell
$env:YOUCLOUD_API_KEY="your-key-here"
```

**Desktop apps (QwenWork, WorkBuddy, etc.)**: environment variables set in a terminal don't reach desktop apps — just paste your key into the conversation and the agent will use it for that request.

Once installed, type `/aggclaw` (or any command above) in the conversation to get started.

## All 4 Skills

| Skill | What It Does | Invoke With | Data source |
|---|---|---|---|
| [aggclaw](aggclaw/SKILL.md) | AppGrowing Global creative analysis: retrieves global creatives across Game / Non-game / Inspiration modes, replies in multiple languages | `/aggclaw` | AppGrowing Global |
| [agclaw](agclaw/SKILL.md) | AppGrowing (China) creative analysis: Agentic Exploration delivers a full campaign analysis report; Inspiration Mode iterates on ideas over multiple turns | `/agclaw` | AppGrowing |
| [youclaw](youclaw/SKILL.md) | youmiyoushu marketing analysis: deep-dives into ad creatives and brand campaign strategies; Inspiration Mode stress-tests and refines creative ideas | `/youclaw` | youmiyoushu |
| [cmclaw](cmclaw/SKILL.md) | CreativeHub AI strategy analysis: analyzes in-house and third-party creatives; scope the search with `@灵感库` `@物料` `@成片` tags; supports multi-turn follow-ups | `/cmclaw` | CreativeHub |

### Get an API Key

All skills authenticate via the `YOUCLOUD_API_KEY` environment variable. The key is obtained the same way for every product: **log in to the product → Profile (个人中心) → Enterprise Info (企业信息)**.

| Skill | Eligibility | Product |
|---|---|---|
| aggclaw | AppGrowing Global AI plus / Pro | [AppGrowing Global](https://appgrowing-global.youcloud.com/) |
| agclaw | AppGrowing Strategy Plan / Game Plus / Ultimate | [AppGrowing](https://appgrowing-cn.youcloud.com/dashboard) |
| youclaw | youmiyoushu Strategy Plan / Strategy Pro / Ultimate | [youmiyoushu](https://console.youshu.youcloud.com/workbench) |
| cmclaw | CreativeHub paid users (excluding the Inspire plan); also requires `DAM_API_BASE` and `python3` | [CreativeHub](https://console.dam.youcloud.com/) |

## How Skills Work

All four skills share the same execution skeleton:

```
┌──────────────────────────────────────────────────────────┐
│ SKILL.md                                                 │
├──────────────────────────────────────────────────────────┤
│ frontmatter    name / description / openclaw metadata    │
│ Eligibility    which plans can use it, where to get key  │
│ Triggers       slash commands                            │
│ Workflow       verify key → pick chat_mode → call API    │
│                → deliver the complete result             │
└──────────────────────────────────────────────────────────┘
```

Red-line rules every skill strictly follows:

- **Never send a request without a valid API Key**; a key pasted directly into the conversation may be used for that request
- API requests time out at **600 seconds** — agents must not abort early or send "working on it…" interim messages before the timeout
- The agent replies **once, with the complete result**; only two situations allow a message: the result arrived, or the request timed out / failed
- When the user's intent is clear, execute immediately — no explanations of how the system works

## Project Structure

```
skills/
├── agclaw/                  # AppGrowing (China) creative analysis
│   ├── SKILL.md
│   └── references/example.md
├── aggclaw/                 # AppGrowing Global creative analysis
│   ├── SKILL.md
│   └── references/example.md
├── cmclaw/                  # CreativeHub strategy analysis
│   ├── SKILL.md
│   ├── skill.py             # requires python3
│   └── references/cm-claw-api.md   # full API docs (domains / fields / streaming protocol / error codes)
├── youclaw/                 # youmiyoushu marketing analysis
│   ├── SKILL.md
│   └── references/example.md
├── README.md                # English (this file)
├── README-CN.md             # 简体中文
└── LICENSE
```

## License

The skill docs and code in this repo are released under the [MIT License](LICENSE): free to install, use, and redistribute.

Note: the license covers only the docs and code in this repo. The actual creative-analysis capability is provided by `YOUCLOUD_API_KEY` and requires a paid quota on the corresponding product (see "Get an API Key" above).
