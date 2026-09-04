# GenCoo AI （原有米云）Agent Skills

GenCoo AI（原有米云 AI 生态） 营销分析技能，让智能体直接调用 AppGrowing Global / AppGrowing / 有米有数 / 创意管家 的广告素材与投放策略分析能力。

![skills](https://img.shields.io/badge/skills-4-blue) ![auth](https://img.shields.io/badge/auth-YOUCLOUD__API__KEY-orange) ![agent](https://img.shields.io/badge/agent-openclaw-purple)

[English](README.md) | [简体中文](README-CN.md)

```
     触发            鉴权            模式            调用            输出
 ┌──────────┐   ┌──────────┐   ┌──────────┐   ┌──────────┐   ┌──────────┐
 │   提问   │──▶ │ 环境变量 │──▶ │ 策略探索 │──▶ │  后端AI  │──▶ │ 策略报告 │
 │ 斜杠命令 │    │ API Key  │    │ 灵感激发 │    │ 等待≤600s│    │ 灵感创意 │
 └──────────┘   └──────────┘   └──────────┘   └──────────┘   └──────────┘
   /aggclaw     YOUCLOUD_API_KEY  chat_mode     完整输出      投放策略报告
```

## Commands

| 你想做什么 | 命令 | 所属 Skill | 数据来源 |
|---|---|---|---|
| 分析**全球**广告素材，自动检测游戏 / 非游戏 / 灵感 | `/aggclaw` | aggclaw | AppGrowing Global |
| 全球**游戏**素材分析 | `/aggclaw-game` | aggclaw | AppGrowing Global |
| 全球非游广告素材分析 | `/aggclaw-app`、`/aggclaw-shortdrama` | aggclaw | AppGrowing Global |
| 分析**全球**广告素材，指定灵感激发模式 | `/agg_inspire` | aggclaw | AppGrowing Global |
| 分析中国广告素材和投放趋势，自动探索 | `/agclaw` | agclaw | AppGrowing |
| 分析中国广告素材，指定探索模式 | `/ag` | agclaw | AppGrowing |
| 分析中国广告素材，指定灵感激发模式（渐进创意碰撞） | `/ag-inspire` | agclaw | AppGrowing |
| 中国电商广告投放营销分析，自动判断模式 | `/youclaw`、`/youmiyun` | youclaw | 有米有数 |
| 中国电商广告投放，指定策略探索模式 | `/creative-chat` | youclaw | 有米有数 |
| 中国电商广告投放，指定灵感激发模式（渐进创意碰撞） | `/grill`、`/grill-chat`、`/grill-youclaw` | youclaw | 有米有数 |
| 创意管家策略分析（支持 @素材范围） | `/cmclaw` | cmclaw | 创意管家 |

不输命令也可以：每个 Skill 都定义了触发关键词（如「投放分析」「分析品牌」「creative analysis」），智能体会自动识别进入流程。

## Quick Start

### 方式一：语义指令安装（推荐）

不敲命令，直接在你的 AI Agent 对话框里说一句话，让智能体自己完成安装：

```
帮我把 GitHub 仓库 gencooai/skills 里的 aggclaw 安装到本地，
装好后告诉我怎么配置 YOUCLOUD_API_KEY。
```

支持文件操作的智能体（Claude Code、Codex、WorkBuddy 等）会自动完成克隆、把 skill 文件夹放进自己的技能目录并加载。需要多个技能时把名字一起说上（aggclaw / agclaw / youclaw / cmclaw）即可。

### 方式二：手动安装

先下载仓库：

```bash
git clone https://github.com/gencooai/skills.git
```

再按你的平台选择对应方式：

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

**千问办公（QwenWork）**

通过界面上传安装：左侧导航「扩展」→「技能」→「安装技能」，上传对应 skill 文件夹内的 `SKILL.md` 及 `references/` 辅助文件，上传后自动识别并加载。

### 配置 API Key

Key 的获取方式见上文「获取 API Key」。拿到后按系统设置环境变量：

**Linux / macOS**

```bash
export YOUCLOUD_API_KEY="your-key-here"   # 当前终端会话生效，写入 ~/.zshrc / ~/.bashrc 可永久生效
```

**Windows PowerShell**

```powershell
$env:YOUCLOUD_API_KEY="your-key-here"
```

**桌面应用（千问办公、WorkBuddy 等）**：终端设置的环境变量不会传入桌面应用，直接把 Key 粘贴到对话里即可，智能体会即次使用。

装好后在对话里说「帮我分析素材」或输入 `/aggclaw` 即可。

## All 4 Skills

| Skill | What It Does | Use When | 数据来源 |
|---|---|---|---|
| [aggclaw](aggclaw/SKILL.md) | AppGrowing Global 全球素材分析：按游戏 / 非游戏 / 灵感三种模式检索全球创意，多语言应答 | 输入「analyze creatives」「global campaigns」，或 `/aggclaw` | AppGrowing Global |
| [agclaw](agclaw/SKILL.md) | AppGrowing（中国版）智能素材分析：策略探索输出完整投放分析报告，灵感激发多轮碰撞创意 | 输入「投放分析」「分析素材」「素材解析」，或 `/agclaw` | AppGrowing |
| [youclaw](youclaw/SKILL.md) | 有米有数智能营销分析：深度拆解广告创意与品牌投放策略，支持创意质询、压力测试与创意打磨 | 输入「分析品牌」「创意质询」「压力测试」，或 `/youclaw` | 有米有数 |
| [cmclaw](cmclaw/SKILL.md) | 创意管家 AI 策略分析：分析自有与第三方素材，支持 `@灵感库` `@物料` `@成片` 等范围限定与多轮续聊 | 输入「创意管家」「策略探索」「灵感激发」，或 `/cmclaw` | 创意管家 |

### 获取 API Key

所有 Skill 均从环境变量 `YOUCLOUD_API_KEY` 读取鉴权，获取路径一致：**登录对应产品 → 个人中心 → 企业信息**。

| Skill | 开放范围 | 关联产品 |
|---|---|---|
| aggclaw | AppGrowing Global AI plus / Pro | [AppGrowing Global](https://appgrowing-global.youcloud.com/) |
| agclaw | AppGrowing 策略版 / 游戏 plus 版 / 至尊版 | [AppGrowing](https://appgrowing-cn.youcloud.com/dashboard) |
| youclaw | 有米有数 策略版 / 策略 pro 版 / 至尊版 | [有米有数](https://console.youshu.youcloud.com/workbench) |
| cmclaw | 创意管家已付费用户（不含灵感版），另需 `DAM_API_BASE` 与 `python3` | [创意管家](https://console.dam.youcloud.com/) |

## How Skills Work

四个 Skill 共享同一套执行骨架：

```
┌──────────────────────────────────────────────────────────┐
│ SKILL.md                                                 │
├──────────────────────────────────────────────────────────┤
│ frontmatter    name / description / openclaw 元数据       │
│ 权限说明       哪个套餐可用，去哪拿 API Key                │
│ 触发方式       触发关键词 + 斜杠命令                       │
│ 执行流程       校验 Key → 定 chat_mode → 调 API → 输出    │
└──────────────────────────────────────────────────────────┘
```

公共红线规则（每个 Skill 都严格遵守）：

- **没有有效 API Key，绝不发送请求**；用户在对话中直接粘贴 Key 时可即次使用
- API 请求超时 **600 秒**，未超时不得中断，不得提前发送「正在处理」类消息
- 等拿到完整结果后**一次性回复**；只有「结果返回」和「超时/报错」两种情况允许说话
- 用户意图明确时直接执行，不解释系统机制

## Project Structure

```
skills/
├── agclaw/                  # AppGrowing(中国版) 素材分析
│   ├── SKILL.md
│   └── references/example.md
├── aggclaw/                 # AppGrowing Global 全球素材分析
│   ├── SKILL.md
│   └── references/example.md
├── cmclaw/                  # 创意管家策略分析
│   ├── SKILL.md
│   ├── skill.py             # 需 python3 运行环境
│   └── references/cm-claw-api.md   # 完整 API 文档（域名/字段/流式协议/错误码）
├── youclaw/                 # 有米有数营销分析
│   ├── SKILL.md
│   └── references/example.md
├── README.md                # English
├── README-CN.md             # 简体中文
└── LICENSE
```


## License

本仓库的 Skill 文档与代码以 [MIT](LICENSE) 协议发布：欢迎自由安装、使用与二次分发。

注意：协议仅覆盖仓库内的文档与代码；实际的素材分析能力由 `YOUCLOUD_API_KEY` 提供，需在对应产品购买额度后使用（见上文「获取 API Key」）。
