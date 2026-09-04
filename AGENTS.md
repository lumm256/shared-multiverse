# AGENTS.md — Shared Multiverse 开发约定

项目中文工作名：**造梦空间**；英文工作名/仓库名：**Shared Multiverse**。

本项目不是普通 AI 视频生成器，也不是预先写死节点的互动短剧，而是：

> **基于 Git 分支理念的多人共享 AI 交互视频多元宇宙。**

核心理念：

> **规则决定这个世界是什么，用户决定世界往哪里走，AI 决定你不知道会发生什么。**

## 开发前必须理解

- `World` ≈ Git Repository：定义规则、变量、角色、地点和 EventTemplate。
- `StoryNode` ≈ Git Commit：某条世界线上实际发生的一次状态变化。
- `Timeline` ≈ Git Branch：用户正在探索的一条世界线。
- `headNodeId` ≈ HEAD。
- Fork 从已有 StoryNode 创建新 Timeline，绝不能改写原 Timeline 历史。
- StoryNode 和 VideoAsset 不天然属于某个用户，可被多个 Timeline 复用。
- Narrative Engine 是 Game Master；视频模型只是 Renderer。
- LLM 必须受 World Rules 与 World State 约束。
- Cache Check 必须发生在未来 Credits 扣费之前。

核心链路：

```text
User Action
→ World Rules / Validation
→ World State
→ Narrative Engine
→ Consequence
→ StoryNode
→ optional Video Rendering
→ Timeline HEAD
```

## 当前唯一开发重点

实现 **Titanic Milestone 1**，先不要接真实视频：

```text
进入 Titanic
→ 匿名 Timeline
→ 初始 StoryNode
→ Action / Custom Action
→ State Transition
→ 新 StoryNode
→ Timeline HEAD 前进
→ 至少连续 3 次
```

## 技术原则

- TypeScript + Next.js / React。
- PostgreSQL。
- MVP 保持单体，不提前拆微服务。
- LLM / Video Model 使用 Provider Adapter 解耦。
- 视频生成按异步任务建模。
- 不要散落 `plan === "pro"` 判断，未来使用 Authorization / Entitlement。
- 模型、分辨率、时长使用 GenerationProfile，不硬编码进 World。
- 禁止提交真实 `.env` / `.env.local`。

修改 World / StoryNode / Timeline / WorldState / Cache 的语义前，必须先阅读 `docs/ARCHITECTURE.md`，并记录重大设计变化。
