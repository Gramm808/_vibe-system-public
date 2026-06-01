# Shared Knowledge Base (Public)

A **public, sanitized** knowledge base: reusable development patterns, process checklists, and
infrastructure notes that are safe to share openly.

## What this is

- A curated, **sanitized** subset of a larger private knowledge base.
- Published through a deliberate **two-stage sanitation gate**: each item is generalized and
  stripped of any private identifiers (hostnames, IP addresses, usernames, file paths, tokens,
  internal project names), independently reviewed a second time, and only then committed here.
- **Published output, not a working repo.** The source of truth lives in a separate private
  repository; this repo only ever receives already-sanitized, public-safe material.

## What you will NOT find here

- No private infrastructure details (hostnames, IP addresses, credentials, tokens, file paths).
- No internal project names, journals, or working notes.
- No git history carried over from the private source — this repository starts from a clean,
  fresh root commit.

## Layout

- `shared-kb/development-patterns/` — reusable patterns and conventions.
- `shared-kb/processes/` — operational procedures and checklists.
- `shared-kb/infrastructure/` — sanitized infrastructure notes.

## Contributing

Items are authored and curated upstream in the private source, then promoted here once they pass
the sanitation gate. Please open an issue rather than committing directly.
