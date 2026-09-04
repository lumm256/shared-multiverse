# Architecture

## 1. High-level model
Three layers:

1. **World Definition** — rules, variables, characters, locations, event templates, generation configuration.
2. **Narrative Runtime** — WorldState, actions, StoryNodes, timelines, history, consequences.
3. **Rendering** — narrative/state + references + generation profile → video provider → VideoAsset.

Metaphor:
- User = Player
- Narrative Engine = Game Master
- H3 Max / video model = Renderer

## 2. Runtime architecture

```text
Browser
  ↓
Next.js / React
  ↓
Backend API (modular monolith)
  ↓
┌───────────────────────────┐
│ Narrative Engine          │
│ Timeline / World services │
│ Cache / Authorization     │
└──────────┬────────────────┘
           ↓
       PostgreSQL
           │
           ├── async generation job
           ↓
     Video Provider Adapter
           ↓
     fal.ai / H3 Max
           ↓
 Cloudflare R2 / CDN
```

Use async/queued video generation even if provider latency is currently low.

## 3. Core domain

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

interface StoryNode {
  id: string;
  worldId: string;
  parentNodeId?: string;
  eventTemplateId: string;
  action: RuntimeAction;
  stateBefore: WorldState;
  stateAfter: WorldState;
  stateHash: string;
  narrative: Narrative;
  videoAssetId?: string;
  createdAt: string;
}
```

Important: StoryNode should not be tightly owned by Timeline. Multiple timelines may share a node or VideoAsset.

## 4. EventTemplate vs StoryNode
`EventTemplate` = what type of event can happen in a world.
`StoryNode` = the concrete occurrence on a runtime worldline.

Do not store a fixed final story graph inside World configuration. Runtime consequences create/reuse StoryNodes.

## 5. Narrative Engine responsibilities
1. Understand predefined/custom user action.
2. Validate feasibility against WorldRules.
3. Apply deterministic/structured state changes where possible.
4. Update character/world state.
5. Select/create next EventTemplate occurrence.
6. Generate concise narrative text.
7. Produce rendering context/prompt.
8. Detect ending conditions.

Custom actions are not unrestricted reality rewriting. Example: a low-credibility passenger trying to seize Titanic's controls may be restrained, lose credibility, and increase panic rather than directly change heading.

## 6. Video rendering and continuity
Do not recursively feed the entire video history to the video model.

Rendering context should be assembled from:
- Character Bible/reference assets
- location references
- current WorldState
- relevant Timeline history summary
- current event/action
- optional previous video for continuous action

For continuous same-scene action: character refs + previous video + prompt.
For scene change: character refs + location refs + state/story context.

## 7. Video cache
Cache is based on rendered state/context, not user or timeline.

Conceptual key:

```text
sha256(
  worldId
  + eventTemplateId
  + normalizedWorldState
  + normalizedVisualContext
  + generationProfile/version
)
```

Flow:

```text
Resolve State
   ↓
Check Cache
  /      \
HIT      MISS
 ↓         ↓
reuse    authorize cost
           ↓
        generate
           ↓
       VideoAsset
```

**Cache check must happen before credit deduction.**

## 8. Anonymous → user migration
MVP may start without login.

Browser stores only a pointer/metadata such as timelineId. Server stores actual Timeline/StoryNode state. Never store videos in localStorage.

Future signup claims anonymous timelines:
`anonymousSessionId → ownerId`.

## 9. Billing-ready abstractions
Reserve:
- User
- AccountBalance
- UsageEvent
- Entitlement
- GenerationProfile

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

Authorization should look conceptually like `authorizeAction({ actor, action, estimatedCost })`, not `if (user.isPro)`.

## 10. Provider adapters
Do not bind domain logic directly to OpenAI/Qwen/fal/H3 Max.

Suggested boundaries:
- `NarrativeProvider`
- `VideoProvider`
- `ObjectStorageProvider`
- later `BillingProvider`

## 11. Deployment
Recommended MVP:
- Vercel: Next.js application, API, previews
- PostgreSQL: Neon/Supabase/equivalent managed PG
- Cloudflare: DNS + R2 object storage/CDN
- fal.ai / H3 Max: video generation
- configurable LLM provider: narrative generation

Avoid premature microservices and unnecessary split execution between Vercel and Workers.
