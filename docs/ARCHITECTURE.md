# 架构设计

## 总体原则

MVP 使用单体架构：

```text
Browser
  ↓
Next.js / React
  ↓
Backend API
  ↓
Narrative / Domain Layer
  ├── PostgreSQL
  └── Generation Queue → Video Provider → VideoAsset
```

部署：Vercel 承载 Next.js；Cloudflare 提供 DNS、R2、CDN；数据库使用托管 PostgreSQL。

## 三层模型

### World Definition
定义世界是什么：Rules、Variables、Characters、Locations、EventTemplates、Generation defaults。

### Narrative State
定义当前世界线发生了什么：WorldState、StoryNode、Timeline、actions、character state、history。

### Rendering
把状态变成用户能看到的 Narrative / scene prompt / references / video。

## 核心领域模型

### World

```ts
interface World {
  id: string;
  meta: WorldMeta;
  rules: WorldRules;
  variables: WorldVariable[];
  characters: Character[];
  locations: Location[];
  eventTemplates: EventTemplate[];
  generationProfileId: string;
}
```

World 保存 EventTemplate，不保存运行时 StoryNode。

### StoryNode

EventTemplate 表示“发生什么类型的事情”；StoryNode 表示“这条世界线上具体发生的这一次事情”。

```ts
interface StoryNode {
  id: string;
  worldId: string;
  parentNodeId?: string;
  eventTemplateId: string;
  action?: RuntimeAction;
  stateBefore: WorldState;
  stateAfter: WorldState;
  stateHash: string;
  narrative: Narrative;
  videoAssetId?: string;
  createdAt: string;
}
```

StoryNode 不应天然包含 `userId`。

### Timeline

```ts
interface Timeline {
  id: string;
  worldId: string;
  ownerId?: string;
  anonymousSessionId?: string;
  parentTimelineId?: string;
  forkedFromNodeId?: string;
  headNodeId: string;
  depth: number;
  visibility: "private" | "unlisted" | "public";
  status: "active" | "ended";
  createdAt: string;
  updatedAt: string;
}
```

Timeline 才是用户拥有/控制的世界线。

### VideoAsset

VideoAsset 表示“某个世界状态的渲染结果”，不是“某用户的视频”。满足相同渲染条件时可跨 Timeline 复用。

## Git 映射

```text
Repository → World
Commit → StoryNode
Commit Tree → Story Graph
Branch → Timeline
HEAD → Timeline.headNodeId
Parent Commit → parentNodeId
Fork Branch → Fork Timeline
Commit State → World State
Artifact → VideoAsset
```

## Narrative Engine

职责：

1. 理解 Action。
2. 校验 World Rules。
3. 判断可行性。
4. 更新 WorldState / Character State。
5. 选择或生成下一个 Event。
6. 生成短 Narrative。
7. 生成 Rendering Context。
8. 判断 Ending。

输入：

```text
World Rules
+ Current World State
+ Relevant Timeline History
+ Current EventTemplate
+ User Action
```

输出应结构化为 Validated Action、State Changes、Consequence、Next Event、Narrative、Rendering Context。

Custom Action 不是“用户说什么就发生什么”。低可行性行为可以失败，并改变 credibility、panic 等状态。

## Provider 解耦

视频接口概念：

```ts
interface VideoProvider {
  generate(input: VideoGenerationInput): Promise<VideoGenerationJob>;
  getStatus(jobId: string): Promise<VideoGenerationStatus>;
}
```

当前候选 fal.ai / H3 Max，但 Domain 不依赖供应商。

连续场景可使用 Character References + Previous Video + Prompt；换场景优先 Character References + Location References + World State + Prompt。不要递归输入全部历史视频。

## GenerationProfile

模型、分辨率、时长、成本不要硬编码进 World：

```ts
interface GenerationProfile {
  id: string;
  provider: string;
  model: string;
  resolution: string;
  durationSeconds: number;
  estimatedCreditCost: number;
}
```

## Cache

概念缓存键：

```text
hash(
  worldId
  + eventTemplateId
  + normalizedWorldState
  + visualContext
  + generationProfileId
  + generationVersion
)
```

Cache 不只是成本优化，未来也是“该宇宙已被探索”的产品信号。

## Anonymous → User

MVP 可匿名游玩。浏览器只保存 timelineId/worldId/currentNodeId/depth 等轻量指针，真实状态在服务器。

未来注册：

```text
anonymousSessionId → claim timelines → ownerId
```

## Billing-ready

当前不接支付，但预留 UsageEvent、Credit Ledger、Entitlement。

```ts
interface UsageEvent {
  id: string;
  userId?: string;
  anonymousSessionId?: string;
  type: "story_generation" | "video_generation";
  worldId: string;
  timelineId: string;
  nodeId?: string;
  cacheHit: boolean;
  providerCost?: number;
  creditsCharged: number;
  createdAt: string;
}
```

禁止到处写 `user.plan === "pro"`，未来通过 `authorizeAction()` / `can(actor, entitlement)`。

## 数据表方向

MVP：

```text
worlds
story_nodes
timelines
video_assets
generation_jobs
anonymous_sessions
```

未来：

```text
users
usage_events
credit_ledger
subscriptions
entitlements
```
