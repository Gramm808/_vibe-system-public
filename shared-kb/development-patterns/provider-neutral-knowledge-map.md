# Provider-neutral Knowledge Map

Durable agent knowledge should **not** live in a single provider's native memory (e.g. a coding
agent's per-project memory directory). That memory is (a) invisible to other agents working the
same repo, and (b) machine-local — it doesn't sync across machines. In a multi-provider,
multi-machine setup it fails on both counts.

Instead, keep knowledge in a **git-tracked, provider-neutral `AGENTS.md`** as two tiers:

- **Tier 1 — always loaded (small):** hard/safety/always-applies rules + the router itself.
- **Tier 2 — loaded on demand:** a **Knowledge Map** table mapping a *trigger* (a topic that may
  come up) → a *file to read* before acting on that topic.

This generalizes how skills already work — a skill's `description` is the always-visible trigger,
its body is read only when relevant — and applies the same idea to knowledge notes. It mirrors the
human intuition "I think I have a note on this; let me go read it" rather than front-loading
everything.

## Why it matters

Solves three problems at once:
1. **Provider lock-in / divergence** — every agent reads `AGENTS.md`; nothing is provider-only.
2. **Front-loading cost** — most facts are irrelevant to most sessions; eager-loading wastes
   context and adds noise. The router is tiny; bodies load on demand.
3. **Sync + truth** — git keeps "the code + what we knew about it" coherent and on every machine.

## How to apply

- Add a `## Knowledge Map (conditional reading)` section to `AGENTS.md`. Rows: `| trigger | file | why |`.
- Targets are existing KB notes / skills — the map is a **router, not content**. Keep it small.
- **Never put safety/always-applies rules behind a trigger** — a trigger is a soft cue the model
  *may* follow; "never hard-delete prod" must be Tier-1 text.
- Wire the capture loop: when a durable, triggerable fact emerges, the KB-update step writes the
  note **and** registers its trigger row (don't capture a note without a route — it's invisible).
- A provider's private memory may stay as a non-authoritative convenience cache only.
