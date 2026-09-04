# MVP — Titanic Vertical Slice

## Goal
Prove the core interactive world loop before spending time/cost on video generation.

Milestone 1 must allow a user to enter Titanic, create/resume an anonymous Timeline, make at least 3 interventions, and receive state-dependent consequences/StoryNodes.

## Initial Titanic WorldState

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

Initial values:

```json
{
  "time": 1415,
  "ship": {"speed":22,"heading":266,"hullDamage":0,"floodingLevel":0,"sinking":false},
  "iceberg": {"discovered":false,"distance":6000,"collisionRisk":65},
  "crew": {"bridgeAlerted":false,"captainAlerted":false,"wirelessAlerted":false},
  "rescue": {"distressSignalSent":false,"lifeboatsPrepared":false,"lifeboatsLaunched":0},
  "passengers": {"panic":0,"casualties":0},
  "player": {"credibility":20,"location":"upper-deck"}
}
```

## EventTemplates v1
- `night-at-sea`
- `strange-horizon`
- `warn-crew`
- `bridge-decision`
- `iceberg-sighted`
- `collision-moment`
- `damage-assessment`
- `evacuation`
- `distress-call`
- `final-outcome`

These are templates, not a mandatory ten-node path. Timelines may skip/reorder templates and later the Narrative Engine may insert events.

## Choice examples
- Strange Horizon: warn bridge / warn lookout / contact wireless / do nothing
- Warn Crew: insist on slowing / claim iceberg / request captain / give up
- Bridge Decision: slow / change course / maintain
- Iceberg Sighted: emergency turn / full astern / head-on response
- Damage: prepare lifeboats / inspect damage
- Evacuation: organize passengers / launch boats
- Distress: CQD / SOS / contact nearby ships
- Always consider Custom Action: `做点别的……`

## State-dependent consequence example
Player credibility is 20 and asks the bridge to slow down. A plausible result may be:

```diff
crew.bridgeAlerted false → true
ship.speed 22 → 20
iceberg.collisionRisk 65 → 52
player.credibility 20 → 35
```

The action enters the world; rules/state determine the consequence. Menu choice must not equal a hardcoded final outcome.

## Milestones
### M1 — Text/state vertical slice
- project scaffold
- domain types
- Titanic config/state/rules/events
- anonymous session/timeline
- 3+ consecutive actions
- deterministic state transition tests
- NarrativeProvider interface; can begin with deterministic/mock narrative output
- basic UI: narrative + state summary + choices

### M2 — LLM Narrative Engine
- structured output schema
- custom action interpretation
- feasibility/rule checks
- provider adapter
- guard against model inventing unsupported state keys

### M3 — Video
- VideoProvider adapter
- fal.ai/H3 Max integration
- async generation status
- VideoAsset persistence
- selected key nodes render 5-second video

### M4 — Cache + Fork/Share
- state/render cache key
- cache reuse
- fork timeline from historical node
- unlisted/public share links
- alternate branches from same node

### M5 — Deployment/observability
- Vercel
- PostgreSQL
- Cloudflare R2
- logs/error tracking
- usage/cost metrics

### M6 — Accounts/credits/payment
- anonymous timeline claim
- account balance
- usage ledger
- entitlement system
- Stripe/payment provider

## Hidden MVP limits
Do not show total node count to users. Internally, target approximately depth 6–10 with hard cap around 12 until the system is proven. Use 3–4 predefined choices plus custom action. Video should be reserved for important nodes; text can bridge transitions.
