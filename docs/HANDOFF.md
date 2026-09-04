# ChatGPT → Codex CLI 交接

更新时间：2026-09-04

GitHub 仓库：`lumm256/shared-multiverse`（Private）。

用户在 macOS 12 上已经成功安装 Codex CLI，后续代码开发转移到本地 CLI。本仓库文档是项目长期上下文，不依赖原 ChatGPT 会话。

## 已确定的决策

- 中文工作名：造梦空间。
- 英文工作名：Shared Multiverse，最终品牌尚未锁定。
- 不是 AI Video Generator，也不是纯 branching story。
- 核心：Shared World + World State + Story Graph + Timeline/Fork + AI Video Rendering。
- 所有玩家共同探索同一个 World；每个人拥有 Timeline；Story Graph 共享。
- StoryNode ≈ Commit；Timeline ≈ Branch；World ≈ Repository。
- Narrative Engine = Game Master；Video Model = Renderer。
- LLM 受 Rules/State 约束。
- StoryNode / VideoAsset 可跨 Timeline 复用。
- Cache Check 在 Credits 扣费之前。
- 商业化核心：已有宇宙尽量免费，创造未探索宇宙消耗 Credits。
- MVP 使用 Vercel + Cloudflare R2 + PostgreSQL，保持单体。
- 首批 World 候选 Titanic / Zombie Apocalypse / Trolley Problem；当前只实现 Titanic。

## 竞品研究后的战略修正

已经确认 Branches、ManyVerse、AI-FMV、NEXZONES、Yoroll、Reverie、Branch 等方向存在。

因此必须坚持：

> **World Simulation + Shared Story Graph + Timeline/Fork + AI Video Rendering + Community Exploration**

`Shared` 是核心关键词。

## Codex 下一步

先阅读 `AGENTS.md` 和全部 `docs/*.md`，检查仓库实际代码后实现 Milestone 1：

```text
用户进入 Titanic
→ 匿名 Timeline
→ 初始 StoryNode
→ Action
→ State Transition
→ 新 StoryNode
→ Timeline HEAD 前进
→ 连续至少 3 次
```

至少测试：

1. 初始 State 正确。
2. Action 能改变 State。
3. 不同 State 有不同后果。
4. Timeline depth/head 正确推进。
5. 原 StoryNode 不被修改。
6. Custom Action 入口存在，可先 rule-based/mock。
7. Domain 不依赖 UI。

## 当前不要做

H3 Max 真调用、Stripe、完整登录、Creator Marketplace、社区 Feed、复杂后台、微服务、Redis Cluster、Kafka、多 World。

## 推荐目录

```text
src/
├── domain/
│   ├── world/
│   ├── world-state/
│   ├── story-node/
│   ├── timeline/
│   ├── narrative/
│   ├── generation/
│   └── billing/
└── worlds/
    └── titanic/
        ├── world.ts
        ├── state.ts
        ├── events.ts
        ├── actions.ts
        └── rules.ts
```

允许 Codex 根据实际 Next.js 项目结构小幅调整。

## Secret 交接

真实凭证不进入文档和 Git。

仓库提交 `.env.example`；用户在本地 `.env.local` 填写 LLM、fal.ai、数据库、R2、未来 Stripe 等 Key。

如果缺变量，Codex 应更新 `.env.example`，而不是要求提交 Secret。

## 推荐 Codex 首条 Prompt

```text
请先阅读仓库根目录 AGENTS.md 以及 docs/ 下的全部文档，把它们视为当前项目的事实来源和架构约束。

在修改代码前先检查仓库现状，然后实现 docs/MVP.md 中的 Milestone 1：Titanic 的纯文本 / World State vertical slice。

当前不要接真实视频生成，也不要实现支付。

核心目标是跑通：
Action → World Rules → World State → Consequence → StoryNode → Timeline HEAD。

请保持领域逻辑与 UI、LLM Provider、Video Provider 解耦。完成后运行测试和应用，修复错误，并总结修改文件、测试结果以及不得不做的架构决定。
```

长期建议：ChatGPT 负责产品/竞品/商业/架构讨论；Codex CLI 负责编码、测试、migration、refactor、commit；重要结论持续写回 Git 文档。
