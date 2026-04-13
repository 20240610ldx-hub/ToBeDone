# ToBeDone
A multi-agent deep research workflow driven purely by natural language. Zero-code, designed to generate think-tank level reports. | 一个由中国大陆文科生纯自然语言打造的多智能体深度研究工作流，零代码产出智库级研报。事在人为，凌霄之志终有回响。

# ToBeDone : Multi-Agent Deep Research Workflow
**一个东北大学文科生用 400 元 Token 打造的、基于纯自然语言驱动的智库级深度研究工作流**

[![GitHub stars](https://img.shields.io/github/stars/20240610ldx/ToBeDone)](https://github.com/20240610ldx/ToBeDone)
[![License](https://img.shields.io/github/license/20240610ldx/ToBeDone)](LICENSE)
[![Status](https://img.shields.io/badge/Status-POC%2FExperimental-orange.svg)](#)

> *"事在人为"——既然体制无法保障未来，那就自己寻找出路。*

---

## 💡 这是什么？(What is ToBeDone)

**ToBeDone**（有待完成之事）是一套基于自然语言 Prompt Engineering 构建的多智能体深度研究工作流，运行在 [Doge](https://github.com/HELPMEEADICE/doge-code)（Claude Code 魔改版）环境中。

它的架构与 Google Gemini Deep Research 同构：**Search → Analyze → Synthesize** 三段式流水线。配合 Human-in-the-loop（人类在环）审核和迭代补充检索机制，**全部由 Markdown 配置文件驱动，无需编写一行代码**。

### 🌟 核心特性
* **严格的四角色Agent流水线**：Lead Researcher（总指挥）→ Search Specialist（检索专家）→ Content Analyzer（质检员）→ Report Synthesizer（总执笔人）。
* **数据完备度闭环（Gap Analysis）**：Analyzer 必须给出“数据已完备”或“强烈建议二次检索+具体关键词”的二选一结论，防止内容空洞。
* **顶级学术规范输出**：强制 GB/T 7714-2015 引文格式、要求至少生成 3 个 Markdown 数据表格、采用智库级话语体系（如：范式重塑、路径依赖等）。
* **生产级防错设计**：4-Key Tavily 故障转移链（单 Key 失效自动切换）、盲写追加（Blind Append）日志机制，对抗长文本 Context 溢出。

---

## 📖 起源与初心：一个文科生的“凌霄之志”

我是东北大学文法学院的一名学生。热爱历史与地理，痴迷于严谨的学术钻研。高中时，我曾成功纠正过历史教材中的一处事实性错误，那是我第一次真切感受到“求真”的力量。

然而，理想与现实的碰撞从未停止：考场失利、背景局限，让我一次次目睹世道的荒诞与底层民众的艰辛。种种际遇让我沉寂许久，但这份郁郁寡欢并未磨平我的棱角——心中的斗志仍在燃烧。

2025 年，Claude Code 源码泄露与 Gemini Deep Research 的强悍能力彻底激发了我的探索欲。我意识到：**技术不应只是少数人的特权，而应成为每一个独立思考者通往真相的阶梯。**

我在 Doge 魔改项目的基础上，结合对 LLM 原理的自学，通过 VPN 向国际先进大模型反复求证。历经无数次幻觉、上下文漂移、逻辑断层，在烧完 400 多元人民币的 Token 后，硬生生打造出了这套工作流。

今天我选择将其开源：
**不是因为它完美，而是因为它已经走到了一个文科生用自然语言 Prompt 工程能达到的极限。**

---

## 🏗️ 系统架构 (Architecture)

```text
┌─────────────────────────────────────────────────┐
│              Lead Researcher（总指挥）             │
│         规划 · 调度 · 判定 · 收尾                  │
└──────┬──────────────┬──────────────┬────────────┘
       │              │              │
       ▼              ▼              ▼
┌────────────┐ ┌────────────┐ ┌──────────────┐
│  Search     │ │  Content   │ │   Report     │
│  Specialist │ │  Analyzer  │ │  Synthesizer │
│  检索特种兵 │ │  质检过滤器 │ │   总执笔人    │
└────────────┘ └────────────┘ └──────────────┘

     Tavily API        Gap Analysis      GB/T 7714
     (4-key failover)   + 2-round cap      Academic Writing
决策                                   ,                               背后逻辑
三 Agent 物理分离,避免单体大模型在长线任务中出现“上下文窗口爆炸”与记忆混乱。
Human-in-the-loop,研究计划必须由人类确认，防止 Agent 跑偏，确保“思想框架”由人主导。
信源提纯与隔离,Analyzer 拦截原生噪点，仅将提纯后的高密度信息（每信源约 300 字）传给 Synthesizer。
盲写日志防覆写,系统级指令：强制先 Read 再 Append/Update，绝不覆写，避免进程意外丢失。

用户需求输入
    │
    ▼
┌─────────────────┐
│  Phase 1: 规划   │  Lead Researcher 生成多维《研究计划草案》
│  Human Review    │  ← 用户介入确认或提出修改建议
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│  Phase 2: 检索   │  Search Specialist 按维度广度检索
│  Distillation    │  清洗无效 HTML，保存结构化搜索结果
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│  Phase 3: 分析   │  Content Analyzer 交叉比对、去重、矛盾识别
│  Gap Analysis    │  评估数据完备度 → 完备 / 需二次检索
└────────┬────────┘
         │
    ┌────┴────┐
    │ 存在断层 │ ──→ 触发 Phase 2 补充检索（最多 2 轮）
    └────┬────┘
         │ 数据完备
         ▼
┌─────────────────┐
│  Phase 4: 撰写   │  Report Synthesizer 开启 <thinking_process> 打草稿
│  Academic Output  │  输出三段式学术段落 + 强制表格化 + GB/T 7714 引用
└────────┬────────┘
         │
         ▼
    final_report_[slug].md (智库级最终报告)

⚠️ 瓶颈、现状与未来之路

诚实地说，在耗尽 400 元 Token 后，我撞到了当前大模型的能力天花板。以下是目前的已知局限，也是待完成之事（ToBeDone）：

    上下文窗口硬墙：复杂课题极易逼近 128K/200K token 的有效推理极限。

    交互面板幻觉：受限于模型逻辑能力，本应输出的控制台交互面板偶尔会退化为纯文本。

    Agent 间记忆断层：Lead Researcher 在规划阶段形成的“直觉判断”较难完美传递给最终的撰写者。

Roadmap — 我做不到的，也许你可以：

    [ ] LangChain / CrewAI 代码化重写（突破 Prompt 局限）

    [ ] RAG + 向量数据库集成（实现持久化跨 Agent 记忆）

    [ ] 流式报告撰写（降低单次 Context 压力）

🤝 致谢与开源寄语

感谢开源社区 doge 项目(https://github.com/HELPMEEADICE/doge-code)提供的魔改基础，感谢那些被我反复“折磨”的国际大模型，更感谢我的母校东北大学——这里教会了我“知识为民”的底色。

本项目采用 MIT 协议开源。我们不只是开源一套工具，更是在共同探索：在 AI 时代，文科生如何重新定义“研究”？

如果未来模型更强了、上下文更宽了——这套基于逻辑的架构只需要极少改动就能获得巨大飞跃。如果你拥有更好的计算资源或代码能力，希望你能在此基础上继续迭代，让普通人也能拥有洞见世界本质的“数字外脑”。

欢迎 Star、提交 PR 或 Issue。 如果你也曾经历理想与现实的激烈碰撞，却始终不愿放弃——欢迎加入我们。
