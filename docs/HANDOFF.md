# ChatGPT → Codex CLI Handoff

## Status at handoff
Date: 2026-09-04
Repository: `lumm256/shared-multiverse` (private, created by user)
Local environment: macOS 12; Codex CLI installed and working. User showed Codex CLI v0.153.2 using `gpt-5.6-sol high` at the time of handoff.

No production code has been implemented yet in this handoff package. The work so far is product/architecture design and competitor analysis.

## Why this handoff exists
ChatGPT's connected GitHub interface could not see/write the newly created private repository in this setup, while Codex CLI can work directly against the user's local clone. The repository should become the durable source of truth so progress does not depend on chat history.

## Decisions already made — do not reopen without a reason
1. Product is a **Shared Multiverse**, not a generic AI video generator.
2. First implemented World is **Titanic**.
3. World definition uses EventTemplates; runtime uses StoryNodes.
4. WorldState is explicit and drives consequences.
5. StoryNode/VideoAsset can be shared; Timeline is the branch/path.
6. Timeline fork never mutates the source timeline.
7. Narrative Engine is the Game Master; video model is the Renderer.
8. MVP starts without real video so the state/action loop can be validated cheaply.
9. Use a modular monolith (Next.js/TypeScript + PostgreSQL), not microservices.
10. Vercel is the default app deployment; Cloudflare R2 for media/object storage.
11. Credits are the primary future usage abstraction; subscription layers on later.
12. Cache check happens before credit charge.
13. Anonymous timelines must be claimable by a future user account.
14. Real secrets never enter Git.
15. English brand/domain are not finalized. `shared-multiverse` is a code/repo name.

## Recommended first coding task
Read all docs, inspect current repository, then implement M1 from `docs/MVP.md`.

Suggested initial structure (adapt if existing scaffold requires it):

```text
src/
  domain/
    world/
    world-state/
    story-node/
    timeline/
    narrative/
    generation/
    billing/
  worlds/
    titanic/
```

Suggested first concrete modules:
- world/domain types
- timeline/domain types
- StoryNode types
- WorldState generic types
- NarrativeEngine/NarrativeProvider boundary
- TitanicWorldState
- Titanic initial state
- Titanic EventTemplates/actions/rules
- state transition service
- tests for state transitions

Then create the simplest UI/API needed to play 3 turns.

## What “done” means for M1
A fresh anonymous visitor can:
1. enter Titanic;
2. receive initial narrative/state summary;
3. choose an action;
4. server evaluates rules and updates state;
5. receive a new StoryNode;
6. repeat at least three times;
7. refresh/resume the Timeline;
8. tests demonstrate state transitions and timeline invariants.

No real video is required for M1.

## Environment/secrets
Copy `.env.example` to `.env.local` locally. User will fill credentials there. Never ask the user to paste secrets into source code or commit them.

Expected future categories include database, LLM provider, fal/video provider, Cloudflare R2, auth and Stripe. Only require keys when the milestone actually uses them.

## Suggested Codex kickoff prompt

```text
Read AGENTS.md and every document under docs/ before making changes.
Treat those files as the project's current source of truth.
Inspect the repository and existing code first.
Then implement Milestone 1 from docs/MVP.md: the Titanic text/state vertical slice.
Do not integrate real video generation yet.
Keep the architecture provider-agnostic and billing-ready as described in docs/ARCHITECTURE.md.
Run tests and the app locally, fix failures, and summarize the implementation and any architectural decisions you had to make.
```

## Future collaboration
After Codex commits/pushes milestones, the user can return to ChatGPT for product/architecture/competitor review. Update these docs whenever decisions change so both environments stay synchronized.
