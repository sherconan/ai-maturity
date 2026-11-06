# AI Collaboration Maturity Assessment (ai-maturity)

[![Stars](https://img.shields.io/github/stars/sherconan/ai-maturity?style=flat)](https://github.com/sherconan/ai-maturity/stargazers)
[![License: MIT](https://img.shields.io/github/license/sherconan/ai-maturity?style=flat)](LICENSE)
[![Release](https://img.shields.io/github/v/release/sherconan/ai-maturity?style=flat)](https://github.com/sherconan/ai-maturity/releases/latest)

**It doesn't grade your AI. It grades how well you work with it.**

**它不评 AI 好不好用，它评你把 AI 用得怎么样。**

A local Agent Skill: it reads your full session history, scores six dimensions, and writes an HTML report with a rank, a title, and what to fix next. Data never leaves your machine.

本地的 Agent Skill：读取完整会话记录，六个维度打分，写出带段位、称号和下一步的 HTML 报告。数据不出本机。

**English** · [中文](#ai-协作成熟度评估ai-maturity)

<p align="center"><img src="assets/sample-report.png" alt="Sample HTML report: score 89, rank 智能体指挥 Max, covered personal title, six-dimension radar" width="720"></p>

<p align="center"><sub>Sample of the HTML report after the score is revealed (Chinese-first). The dark button still hides the personal title. A fresh file covers the score too. The page loads nothing from the network.</sub></p>

## Try it in 30 seconds

Claude Code is the host that runs the skill. You need macOS or Windows, Git, and Python 3. The scripts use the standard library only; the agent runs them, so there is nothing to install with pip. You also need session history already on the machine. Linux detection is still wrong ([#2](https://github.com/sherconan/ai-maturity/issues/2)).

```bash
mkdir -p ~/.claude/skills
git clone https://github.com/sherconan/ai-maturity.git ~/.claude/skills/ai-maturity
```

On Windows those commands work in Git Bash. In PowerShell, clone into `$env:USERPROFILE\.claude\skills\ai-maturity`.

Open Claude Code and send the shortest phrase the skill is written to recognize:

```text
成熟度评估
```

The skill description also names `AI协作评估`, `段位测评`, `测评我的使用`, `我的段位`, and `给我打个分`. In English, ask for the assessment by the name the skill publishes:

```text
Assess my AI collaboration maturity
```

It asks three questions. If the client cannot ask, it uses the defaults and tells you.

1. **Range.** `全量` — full history. This is the default.
2. **Industry.** `通用`, unless you work in securities/finance (`证券金融`, the default) or law (`法律`).
3. **Tone.** `完整版` (default), or `温和版` if a manager might read it.

The renderer writes `~/Desktop/AI协作成熟度评估报告.html` and prints that path. Open the file and click once to reveal the score and rank, then again for the personal title. Fewer than 15 real human sessions produce a cold-start guide and no rank. No session files means the collector reports that it found nothing, and there is no score ([#5](https://github.com/sherconan/ai-maturity/issues/5)).

## Who this is for

**For you** if you use Claude Code on macOS or Windows and want a private read on your own habits. Sessions from Codex (ChatGPT Work) and the other clients in the [support matrix](references/VALIDATION.md) can be included when they live on the same machine.

**Not for you** if you want a model leaderboard, or a site that stores chats. The HTML report is Chinese-first ([#3](https://github.com/sherconan/ai-maturity/issues/3)). Linux support is still open ([#2](https://github.com/sherconan/ai-maturity/issues/2)). 豆包工作 (DoubaoWork) and KroWork keep no readable local transcript. Domate has no checked sample yet.

## Why

Every benchmark out there measures the AI — leaderboards, evals, capability scores. Nobody measures **the human**. Yet the real gap is rarely "is the model good enough"; it's "am I using it well":

- Do you define what "done" means when you hand over a task?
- Is your AI an autonomous coworker, or a button you press step by step?
- Has your repetitive work become one-command automation?

The answers are already sitting in your own session logs. This skill reads them, scores them, and tells you what to fix next.

## Three design principles

1. **Data never leaves your machine.** All analysis runs locally; the report is a self-contained HTML with zero external links. You decide who sees it.
2. **Evidence before scores.** Every point cites your real sessions as evidence; counting is done by a deterministic script, and the model is not allowed to invent numbers.
3. **Reverse-engineering welcome.** This is a behavioral assessment, not an exam — the only way to game it is to actually build the habits: write acceptance criteria, feed real context, automate, parallelize. To cheat, you must genuinely get better.

## Ranks

| Rank | Score | In one line |
|---|---|---|
| ❄️ Cold Start | <15 real sessions | Not rated yet — go use it first |
| Chatbox Player → Pro → Max | 0–39 | Still in Q&A mode |
| Prompt Engineer → Pro → Max | 40–59 | Speaks well, doesn't delegate yet |
| Tool Caller → Pro → Max | 60–74 | Function calling awakened |
| Agent Commander → Pro → Max | 75–89 | Agentic-era player |
| Multi-Agent Orchestrator | ≥90 + 2 hard achievements | Runs an AI workshop in parallel |
| Human-AI Symbiosis | ≥95 + 3/3 | Rare rank · the summit |

Three **hard achievements** (a high score alone won't promote you — real proof required): ⚙️ Unattended — automation actually running · ⚡ Parallel Life — concurrent work / sub-agent orchestration · 📦 Everything-as-a-Skill — repetitive work turned into skills.

In the HTML file the score and the rank start covered. Click once to reveal them (the report uses the Chinese names from this table), and again for the personal title.

## Six dimensions

Did you say it clearly · Do you let go · Did you give it the materials · Do you check the work · Do you keep what repeats · Do you actually use it.

The report prints the Chinese names: 话说清了吗 · 敢不敢放手 · 材料给够了吗 · 收货严不严 · 会不会攒家底 · 用得勤不勤. Scoring includes gains and penalties, an industry overlay (securities & finance / legal / general), and a cold-start guard. Rules: [references/rubric.md](references/rubric.md).

## Supported clients

The skill reads local data on macOS and Windows. Clients with a local parser: Claude Code, WorkBuddy, QoderWork, TRAE Work, ChatGPT Work (Codex), KimiWork, OpenClaw, Gemini, Hermes. 豆包工作 and KroWork keep no local plaintext transcript. Domate is an unchecked candidate. Verified, experimental, and unchecked cells are in [references/VALIDATION.md](references/VALIDATION.md). Linux is treated as macOS today ([#2](https://github.com/sherconan/ai-maturity/issues/2)).

Several clients on one machine can be merged (all-clients mode, `--tool all`). Cross-client parallel work is the strongest orchestration evidence.

## Architecture & self-evolution

```
deterministic stats  →  model judgment  →  fixed-template rendering
collect_stats.py        SKILL.md + rubric   render_report.py
```

Stats never come from the model (no fabrication), judgment never comes from scripts (that's the one thing models are for), rendering is never improvised (every report looks equally good). The skill also ships a self-evolution protocol — the data layer auto-patches, the scoring layer proposes-and-awaits-approval. See [EVOLUTION.md](EVOLUTION.md).

## License

MIT

---

# AI 协作成熟度评估（ai-maturity）

**它不评 AI 好不好用，它评你把 AI 用得怎么样。**

一个 Agent Skill：读取你本机的完整 AI 会话记录（不是只有几天的短期记忆），六个维度评估你与 AI 协作的成熟度，产出一份带段位、称号和改进路径的交互式 HTML 报告。

> 你的段位可能是「提示词工程师 Pro」，也可能是「智能体指挥 Max」——在 HTML 报告里点击揭晓。

## 30 秒试用

跑这个 skill 的宿主是 Claude Code。系统要 macOS 或 Windows，本机有 Git 和 Python 3。脚本只用标准库，由 Agent 自己跑，不用 pip 装任何东西。机器上得已经有会话记录。Linux 目前会被识别成 macOS（[#2](https://github.com/sherconan/ai-maturity/issues/2)）。

```bash
mkdir -p ~/.claude/skills
git clone https://github.com/sherconan/ai-maturity.git ~/.claude/skills/ai-maturity
```

Windows 上，Git Bash 可以直接跑上面两行。PowerShell 则克隆到 `$env:USERPROFILE\.claude\skills\ai-maturity`。

打开 Claude Code，发送 skill 里写明的最短一句：

```text
成熟度评估
```

描述里同样写了这些触发句：`AI协作评估`、`段位测评`、`测评我的使用`、`我的段位`、`给我打个分`。用英文问也可以，说技能自己的英文名：

```text
Assess my AI collaboration maturity
```

接下来三问。客户端不能提问时，用下面的默认值并告诉你。

1. **范围。** `全量`。这是默认。
2. **行业。** 不是证券金融、也不是法律，就选 `通用`。默认是 `证券金融`，别让它替你选错。
3. **口味。** `完整版`（默认）。准备给同事或领导看，选 `温和版`。

渲染脚本把报告写到 `~/Desktop/AI协作成熟度评估报告.html`，并打印这个路径。打开后先点击揭晓分数和段位，再点击揭晓专属称号。真人会话不足 15 条只出冷启动引导，不定级。一条会话文件都没有时，采集脚本会说明什么都没找到，不会出分数（[#5](https://github.com/sherconan/ai-maturity/issues/5)）。

## 适合谁

**适合你**：你在 macOS 或 Windows 上用 Claude Code，想私下看看自己的协作习惯。同一台机器上如果还有 Codex（ChatGPT Work）或[适配矩阵](references/VALIDATION.md)里的其他客户端，可以一并读进来。

**不适合你**：你要的是模型榜，或一个帮你存聊天记录的网站。HTML 报告目前是中文（[#3](https://github.com/sherconan/ai-maturity/issues/3)）。Linux 支持还没做（[#2](https://github.com/sherconan/ai-maturity/issues/2)）。豆包工作、KroWork 本地没有可读的明文会话。Domate 还没有验证样本。

## 为什么做这个

市面上所有测评都在测 AI（跑分、榜单、能力评估），没有人测**人**。但企业和个人真正的问题从来不是"模型行不行"，而是"我用得行不行"：

- 你布置任务时说清"什么算做完"了吗？
- 你是把它当独立干活的助手，还是当一个要逐步按的按钮？
- 反复做的事，变成一键自动化了吗？

这些问题的答案全部藏在你自己的会话记录里。这个 skill 把它们读出来、打成分、给出下一步。

## 三个设计原则

1. **数据不出本机。** 全部分析在你的电脑上完成，不上传任何内容。报告是一个自包含 HTML（零外链），给谁看由你决定。
2. **证据先于分数。** 每个维度的每一分都要引用你的真实会话作为证据；统计由固定脚本完成，模型不许自己编数字。
3. **欢迎反向工程。** 这是行为测评，不是考试——想刷分的唯一办法是真的改掉坏习惯：写验收标准、接数据源、做自动化、学会并行。为了作弊，你必须真的变强。

## 段位体系

| 段位 | 分数 | 一句话 |
|---|---|---|
| ❄️ 冷启动 | 真实会话 <15 | 数据不足暂不定级，先用起来 |
| 聊天框玩家 → Pro → Max | 0–39 | 还在和 AI 一问一答 |
| 提示词工程师 → Pro → Max | 40–59 | 会说话了，还没放权 |
| 工具调用者 → Pro → Max | 60–74 | Function Calling 已觉醒 |
| 智能体指挥 → Pro → Max | 75–89 | Agentic 时代玩家 |
| 多智能体编排者 | ≥90 + 硬成就≥2 | 同时指挥一个 AI 车间 |
| 人机合一 | ≥95 + 硬成就 3/3 | 稀有段位 · 顶点 |

三枚**硬成就**（光分高上不去，必须有实证）：⚙️ 无人值守（真实运行的自动化）· ⚡ 多线程人生（并行/子代理编排）· 📦 万物皆可 skill（重复工作的固化率）。

## 六个维度

话说清了吗 · 敢不敢放手 · 材料给够了吗 · 收货严不严 · 会不会攒家底 · 用得勤不勤——白话在前，术语细则见 [references/rubric.md](references/rubric.md)。评分含得分项/扣分项明细、行业覆盖层（证券金融 / 法律 / 通用）与新手保护。

## 支持的客户端

自动探测本机数据（Mac / Windows）。能读本地会话的有：Claude Code、WorkBuddy、QoderWork、TRAE Work、ChatGPT Work (Codex)、KimiWork、OpenClaw、Gemini、Hermes。豆包工作、KroWork 本机没有可读的明文会话。Domate 仍是未验证的候选路径。哪些是实测通过、实验性、还是未验证，见 [references/VALIDATION.md](references/VALIDATION.md)。Linux 目前会被识别成 macOS（[#2](https://github.com/sherconan/ai-maturity/issues/2)）。

多个客户端可以合并评估（全家桶，`--tool all`）。跨端并行是最硬的编排证据。

## 自进化

本 skill 内置自进化协议（参考 [Anthropic 官方博客的自进化 Agent Skills 实践](https://claude.com/blog/how-warp-builds-self-improving-agents-on-claude)）：每次运行捕获反馈信号，数据层自动修补、评分层提案待批，随使用复利变准。详见 [EVOLUTION.md](EVOLUTION.md)。

## 架构

```
脚本统计（确定性）→ 模型判读（定性）→ 模板渲染（固定）
collect_stats.py      SKILL.md + rubric      render_report.py
```

统计不靠模型（防编造），判读不靠脚本（那是模型唯一擅长的），渲染不靠现写（每张报告一样好看）。

## License

MIT

# Contributing

Four open issues are welcome as they stand. A large change should start as a comment on the issue; a small fix can go straight to a PR. Workflow, CI, and the privacy rule: [CONTRIBUTING.md](CONTRIBUTING.md).

- [#2](https://github.com/sherconan/ai-maturity/issues/2) — Linux support. Detection currently treats Linux as macOS.
- [#3](https://github.com/sherconan/ai-maturity/issues/3) — An English HTML report. The renderer is Chinese-only today.
- [#4](https://github.com/sherconan/ai-maturity/issues/4) — Unit tests with synthetic session fixtures. Labeled **good first issue**.
- [#5](https://github.com/sherconan/ai-maturity/issues/5) — A non-zero exit and a clear message when no sessions are found.

No real session content, names, or company-identifiable information in an issue or PR.

Share card, 1280×640: [assets/og-card.png](assets/og-card.png).

# 欢迎贡献

上面四个 issue 都欢迎做，不必改写 issue 本身。改动大就先在 issue 里留言，小修可以直接提 PR。流程、CI 和隐私红线见 [CONTRIBUTING.md](CONTRIBUTING.md)。

- [#2](https://github.com/sherconan/ai-maturity/issues/2) — Linux 支持。现在会把 Linux 识别成 macOS。
- [#3](https://github.com/sherconan/ai-maturity/issues/3) — 英文版 HTML 报告。渲染器目前只有中文。
- [#4](https://github.com/sherconan/ai-maturity/issues/4) — 用合成会话补单元测试，带 **good first issue** 标签。
- [#5](https://github.com/sherconan/ai-maturity/issues/5) — 找不到会话时返回非 0，并给出看得懂的提示。

Issue 和 PR 里不要放真实会话、姓名，或任何能识别公司的信息。

分享图（1280×640）：[assets/og-card.png](assets/og-card.png)。
