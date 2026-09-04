# Shared Multiverse / 造梦空间

> **Every choice can fork reality. 每一个选择，都可能让现实产生新的分支。**

一个基于 Git 分支理念的多人共享 AI 交互视频多元宇宙。

当前处于 MVP 阶段，第一个 World 为 **Titanic**。第一目标不是生成漂亮视频，而是验证：

```text
World
→ StoryNode
→ User Action
→ World State Change
→ Narrative Engine
→ New StoryNode
→ Timeline
```

## Git 概念映射

| Git | Shared Multiverse |
|---|---|
| Repository | World |
| Commit | StoryNode |
| Branch | Timeline |
| HEAD | Timeline.headNode |
| Parent Commit | parentNode |
| Fork | Fork Timeline |
| Commit State | World State |
| Artifact | VideoAsset |

## 文档阅读顺序

1. `AGENTS.md`
2. `docs/PRODUCT.md`
3. `docs/ARCHITECTURE.md`
4. `docs/MVP.md`
5. `docs/COMPETITORS.md`
6. `docs/MONETIZATION.md`
7. `docs/HANDOFF.md`

## 部署方向

- Vercel：Next.js / SSR / API / Preview Deployment
- Cloudflare：DNS / R2 / CDN
- PostgreSQL：Neon、Supabase 或同级托管服务
- AI：Narrative LLM Provider + Video Provider

真实 API Key 只放 `.env.local` 或部署平台 Secret，禁止提交 Git。
