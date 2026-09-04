# 商业化设计

## 核心原则

> **不要卖“看视频”，卖“创造新的世界线”。**

已有 StoryNode / VideoAsset 的复用成本低；真正产生成本的是新的故事推演、新视频、Custom Action 和高质量连续性生成。

## 收费方向

- 浏览公开 World：免费
- 查看分享 Timeline：免费
- 播放缓存视频：免费
- 进入已有 StoryNode：免费
- Fork 已有 Timeline：免费或低门槛
- 新文本 StoryNode：低成本/免费额度
- 新视频：主要 Credits 消费点
- Custom Action + 新生成：高价值消费
- HD/更长/更高连续性：Premium
- Private Timeline：订阅权益
- Create World：未来 Creator 权益

## Credits First

早期优先 Credits，而不是只做订阅。具体数字必须根据真实模型成本重新计算。

核心产品语言：

> **Discover an existing universe — FREE**  
> **Create an unexplored universe — CREDITS**

Cache Check 必须先于扣费：

```text
Resolve State
→ Cache Check
├── HIT → reuse → 不收生成 Credits
└── MISS → authorize → credits → generate
```

## Subscription Later

Free Explorer：少量生成额度、公开世界、基础画质、有限 Custom Action。

Dreamer Pro：月度 Credits、额外 Credits 折扣、HD、更多 Custom Action、Private Timeline、更深 Timeline、Priority Generation。

不要承诺 Unlimited AI Video。

## Creator Economy

长期方向：AI World Factory、角色 reference、Private/Commercial/Paid World、Creator revenue share、平台抽成。

## Entitlement

不要散落 plan 判断，使用 `can(actor, "hd_video")` 等统一能力判断。

未来重点指标：单用户 AI 成本、World 成本、平均 Timeline Depth、视频数、Video Cache Hit Rate、StoryNode Reuse Rate、毛利、Custom Action 使用率。
