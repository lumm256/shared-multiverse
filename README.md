# Shared Multiverse

> A shared AI multiverse where every choice can fork reality.

Chinese project name: **造梦空间**. English brand is not finalized.

Shared Multiverse is an interactive world platform where many users explore the same World, actions alter explicit WorldState, timelines fork like Git branches, and AI video renders important moments of those worldlines.

## Start here
For Codex/AI agents: read [`AGENTS.md`](./AGENTS.md), then all documents in [`docs/`](./docs/).

For development, the first target is the **Titanic text/state vertical slice**. Do not start by building a generic video generator.

## Documents
- `docs/PRODUCT.md` — positioning and product invariants
- `docs/ARCHITECTURE.md` — domain/technical architecture
- `docs/MVP.md` — Titanic milestones
- `docs/COMPETITORS.md` — competitive landscape/differentiation
- `docs/MONETIZATION.md` — credits/subscription direction
- `docs/HANDOFF.md` — ChatGPT → Codex CLI handoff/status

## Secrets

```bash
cp .env.example .env.local
```

Put real credentials in `.env.local` or deployment-platform secrets. Never commit real `.env` values, even to a private repository.
