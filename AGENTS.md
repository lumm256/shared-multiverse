# Shared Multiverse — Agent Instructions

## Read first
Before changing code, read every document under `docs/`, especially `HANDOFF.md`, `PRODUCT.md`, `ARCHITECTURE.md`, and `MVP.md`.

## Product invariant
Shared Multiverse is **not** a generic AI video generator and **not** a conventional branching-story generator. It is a shared, persistent possibility space:

- many users explore the same `World`;
- user actions alter explicit `WorldState` under `WorldRules`;
- the Narrative Engine interprets consequences and creates `StoryNode`s;
- each user's path is a `Timeline` (Git branch analogy);
- timelines can fork from any existing node without mutating the original timeline;
- multiple timelines may reuse the same StoryNode / VideoAsset when the normalized state and rendering context are equivalent;
- video is a renderer/window into world state, not the story engine.

Working product definition:
> A shared AI multiverse where every choice can fork reality.

Chinese project name: **造梦空间**. English brand is intentionally **not finalized**. Repository/code name: `shared-multiverse`.

## Core philosophy
> 规则决定这个世界是什么，用户决定世界往哪里走，AI 决定你不知道会发生什么。

## Git mental model
- Repository → World
- Commit → StoryNode
- Commit tree → Story Graph
- Branch → Timeline
- HEAD → Timeline.headNodeId
- Parent commit → StoryNode.parentNodeId
- Fork → new Timeline from an existing node
- Artifact/blob → VideoAsset
- Commit state → WorldState

## Architecture rules
1. `StoryNode` is not owned by a user or timeline; nodes/assets may be shared.
2. `Timeline` owns a path/head and may belong to an anonymous session or future user.
3. World definitions are configuration-driven; adding a world should not require Narrative Engine rewrites.
4. LLMs must operate inside explicit rules/state. Do not let the model freely rewrite reality.
5. Check generation cache **before** charging credits.
6. Do not scatter plan checks such as `user.isPro`; use authorization/entitlements.
7. Do not hardcode a single LLM/video provider. Use provider adapters.
8. Never commit real secrets. `.env.example` is committed; `.env.local` is local only.
9. Start as a modular monolith. Do not introduce microservices for the MVP.
10. Preserve incomplete information in the UI; do not expose every internal probability/state variable.

## Current development priority
Implement **Titanic Vertical Slice / Milestone 1** before real video generation:

`enter Titanic → anonymous timeline → StoryNode → action/custom action → rule/state evaluation → consequence → next StoryNode`, repeat for at least 3 interventions.

The core loop must work without H3 Max first. After that, integrate video rendering and caching.

## Development behavior
- Prefer TypeScript and small domain modules.
- Add tests for deterministic state transitions and invariants.
- Keep interfaces/provider boundaries explicit.
- When a product decision is ambiguous, consult docs instead of inventing a new product direction.
- If changing an architectural invariant, update docs in the same commit and explain why.
