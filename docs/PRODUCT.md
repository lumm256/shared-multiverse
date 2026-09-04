# 产品定义

## 产品定位

中文工作名：**造梦空间**。英文品牌尚未最终确定，仓库工作名为 **Shared Multiverse**。

一句话定义：

> **基于 Git 理念的多人共享交互视频式 AI 多元宇宙。**

内部英文定义：

> A shared AI multiverse where every choice can fork reality.

产品核心不是“生成视频”，而是：

> **改变一个世界，并看到另一种现实出现。**

## 核心体验

```text
进入 World
→ 观看/阅读 StoryNode
→ 关键事件
→ 预设 Action 或 Custom Action
→ Narrative Engine
→ World Rules + World State 推演
→ 新 StoryNode
→ 必要时生成 VideoAsset
→ Timeline 前进
→ 继续 / Fork / 分享
```

## 产品哲学

> **规则决定世界是什么，用户决定世界往哪里走，AI 决定用户不知道会发生什么。**

所谓“无限感”来自：

> **有限规则 × 大量状态组合 × 用户自由 × AI 对后果的解释与不可预测性。**

不要在 UI 暴露总节点数，例如不要显示 `4/10`，而应保持：

```text
Past → Current → ???
```

## Shared 是核心

普通 AI Story 是每个用户拥有一份孤立故事；本项目是所有用户共同探索同一个 World，并共同扩展 Story Graph，而每个人拥有自己的 Timeline。

```text
                         TITANIC WORLD
                              N1
                              │
                              N2
                         ┌────┼────┐
                         N3   N4   N5
                        / \        /                        N6 N7      N8 N9
```

用户可以沿已有节点探索，也可以从任意允许的节点 Fork 出新 Timeline。

## World State，而不是固定 Choice Tree

禁止退化成：

```text
Choice A → Node A
Choice B → Node B
```

正确模型：

```text
User Action
→ World Rules
→ World State
→ Narrative Engine
→ Consequence
→ StoryNode
```

例如“要求 Titanic 减速”可能造成：

```text
ship.speed: 22 → 18
iceberg.collisionRisk: 65 → 42
crew.bridgeAlerted: false → true
player.credibility: 20 → 35
```

后续故事从新状态继续运行。

## 视频的角色

```text
Narrative Engine = Game Master
Video Model      = Renderer
```

视频用于沉浸、关键时刻和分享，不承担世界逻辑。

推荐节点 UI：

```text
Video
→ 短 Narrative
→ 有限 State HUD
→ Choices / Custom Action
```

不要展示全部内部变量和概率。

## 初始 Worlds

候选：

1. Titanic
2. Zombie Apocalypse
3. Trolley Problem

当前只实现 Titanic。

长期目标是配置驱动新增 World，未来可以发展 AI World Factory。

## MVP 最关键验证

不是视频质量，而是：

> **用户会不会在同一个世界里连续干预 2–3 次以上？**

参考指标：第一次选择率 >60%，第二次选择率 >40%，Average Timeline Depth ≥3 值得继续验证。
