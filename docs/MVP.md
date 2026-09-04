# Titanic MVP

## 目标

验证：

> **用户是否愿意连续改变一个世界，并期待系统根据 World State 给出未知后果。**

Milestone 1 暂不接真实视频。

## Vertical Slice

```text
进入 Titanic
→ 创建/恢复匿名 Timeline
→ 初始 StoryNode
→ Narrative + Choices
→ Action / Custom Action
→ World Rules
→ State Transition
→ 新 StoryNode
→ Timeline HEAD 前进
→ 至少重复 3 次
```

## TitanicWorldState

```ts
interface TitanicWorldState {
  time: number;
  ship: {
    speed: number;
    heading: number;
    hullDamage: number;
    floodingLevel: number;
    sinking: boolean;
  };
  iceberg: {
    discovered: boolean;
    distance: number;
    collisionRisk: number;
  };
  crew: {
    bridgeAlerted: boolean;
    captainAlerted: boolean;
    wirelessAlerted: boolean;
  };
  rescue: {
    distressSignalSent: boolean;
    lifeboatsPrepared: boolean;
    lifeboatsLaunched: number;
  };
  passengers: {
    panic: number;
    casualties: number;
  };
  player: {
    credibility: number;
    location: string;
  };
}
```

初始关键值：time=1415、speed=22、heading=266、hullDamage=0、floodingLevel=0、iceberg.distance=6000、collisionRisk=65、credibility=20。

## EventTemplates

```text
night-at-sea
strange-horizon
warn-crew
bridge-decision
iceberg-sighted
collision-moment
damage-assessment
evacuation
distress-call
final-outcome
```

它们不是固定 10 节剧情，而是事件模板。

## Actions 示例

- Strange Horizon：通知驾驶台 / 警告瞭望员 / 去无线电室 / 什么也不做 / 做点别的
- Warn Crew：坚持减速 / 声称有冰山 / 要求见船长 / 放弃 / 做点别的
- Bridge Decision：减速 / 改变航向 / 保持航向 / 做点别的
- Iceberg Sighted：紧急转向 / 全速倒车 / 尝试正面碰撞 / 做点别的
- Damage Assessment：准备救生艇 / 检查损伤 / 组织乘客 / 发求救信号 / 做点别的

后果必须依赖当前 State，而不是 Action 直接等于结果。

## UI

Milestone 1 可先纯文字：

```text
23:35 · 北大西洋

夜色中，海面平静得异常。
远处似乎出现一道比夜色更黑的轮廓……

航速：22 knots
船员警觉：低

[通知驾驶台]
[警告瞭望员]
[继续观察]
[做点别的……]
```

未来升级为 Video → Narrative → State HUD → Choices。

## 深度

内部建议普通 Timeline 6–10，hard limit 约 12；用户端不显示总节点数。关键节点 3–4 个预设 Action + Custom Action。视频只用于重要节点。

## Milestones

### M1 — Text / State Vertical Slice
World、初始 State、EventTemplates、StoryNode、Timeline、anonymous session、Actions、Custom Action 基础入口、State transition、连续至少 3 次、基本测试。

### M2 — Narrative LLM
Provider Adapter、结构化输出、Rule validation、Custom Action 理解、mock/fallback、prompt versioning。

### M3 — Video
Video Provider、fal.ai/H3 Max、async job、VideoAsset、R2、generation status、reference strategy。

### M4 — Shared Timeline
StoryNode reuse、cache、Fork、share link、public/unlisted、branch discovery。

### M5 — Production
Vercel、Cloudflare R2、PostgreSQL、logging、error tracking、usage metrics。

### M6 — User / Credits
登录、anonymous claim、UsageEvent、Credit Ledger、Stripe、Entitlement。

M1 明确不做真实视频、支付、完整用户系统、社区、Creator World、微服务。
