# Product — Shared Multiverse / 造梦空间

## 1. Positioning
An interactive AI-video multiverse where users enter a known world, intervene at important moments, and create/fork timelines. The long-term product combines interactive cinema, text adventure, world simulation, branching timelines, UGC and community exploration.

Core loop:

`Enter World → experience current StoryNode → choose / custom action → Narrative Engine evaluates rules + state → update WorldState → create/reuse StoryNode → render/reuse video → continue / fork / ending / share`

The product should feel like exploring possibility space, not operating a video-generation form.

## 2. Product philosophy
> 规则决定这个世界是什么，用户决定世界往哪里走，AI 决定你不知道会发生什么。

The feeling of infinity comes from finite rules × many state combinations × user freedom × AI interpretation. Do not promise literally infinite generated assets. Hide total graph size from users; the future should feel unknown (`???`).

## 3. Shared Multiverse differentiation
All users explore the same World and collectively grow a Story Graph. Each user owns/follows a Timeline through that graph.

Example:

```text
                         TITANIC WORLD
                              N1
                              │
                              N2
                         ┌────┼────┐
                         ↓    ↓    ↓
                        N3   N4   N5
                       / \        / \
                      N6  N7      N8 N9
```

A may follow N1→N2→N3→N6. B may follow N1→N2→N4. C can open A's shared timeline and fork from N3 to N7. C never mutates A's timeline.

## 4. Cache as product mechanic
Caching is not only infrastructure optimization. It represents whether a universe has already been explored.

- Existing explored branch → instant reuse, normally free.
- Unexplored branch/state → generation required, may consume credits.

Potential UX:

```text
Hard Turn       12,381 timelines   FREE / instant
Full Reverse     4,281 timelines   FREE / instant
Ram Iceberg      UNEXPLORED        Explore · 5 Credits
```

Product language candidate:
> Discover existing universes for free. Create an unexplored universe with credits.

## 5. World strategy
Initial worlds:
1. Titanic — first implementation / historical what-if.
2. Trolley Problem — moral dilemma.
3. Zombie Apocalypse — survival simulation.

Good worlds combine mass recognition, desire to decide, dramatic consequences, shareability, and low comprehension barrier.

Long-term world categories: historical what-if, moral dilemmas, survival/apocalypse, life what-if, and carefully handled recognizable fictional/cinematic structures where IP rights allow.

## 6. Naming
Chinese project name: **造梦空间** remains suitable as a Chinese project/brand candidate because it conveys creation and alternate spaces, but it no longer precisely describes the mechanism.

Do not mechanically translate it to `Dream Space` as the global brand. English naming should center on concepts such as reality, possibility, timeline, fork, branch, universe, divergence. `Worldline`, `Forkverse`, `ManyVerse`, etc. had collision/availability concerns during preliminary research, so brand/domain selection remains open.

Repository/code name: `shared-multiverse`.

## 7. MVP success signals
Primary validation question: do users intervene repeatedly in the same world rather than generate once and leave?

Early heuristics:
- first choice rate > 60%
- second choice rate > 40%
- average timeline depth ≥ 3 is promising
- repeated 2–3+ interventions is more important than raw video generations

## 8. UX
A StoryNode is a complete experience unit:
- video (optional in early MVP)
- short narrative text
- selected state/status HUD
- 3–4 predefined choices
- always offer `做点别的……` / Custom Action when appropriate

Do not expose all internal variables or exact probabilities; otherwise the experience becomes spreadsheet optimization rather than uncertain world exploration.
