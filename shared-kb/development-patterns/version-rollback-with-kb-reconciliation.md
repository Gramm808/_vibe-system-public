# Version rollback with KB reconciliation

A version rollback should be a **full rollback** — code + KB + journals + SQL all return to the
target version, which becomes the new mainline — but it must not silently discard the *knowledge*
accumulated since that version. The reconciliation is the valuable part.

Mechanism:
1. **Snapshot** the pre-rollback state with a git tag (`pre-rollback/<timestamp>`). The tag *is*
   the snapshot — no file copy; git history is never lost.
2. **Roll back** the tree to the target as a forward commit (history intact; target becomes main).
3. **Reconcile the KB with judgment:** diff the snapshot's `_dev/kb` against the rolled-back KB.
   Classify each change as **code-dependent** (describes behavior/schema just reverted → correctly
   gone, drop it) or **non-code-dependent value-add** (a learned fact, gotcha, external dependency,
   decision rationale — true regardless of version → propose re-applying it). **A human approves**
   per item before anything is written.

## The hard rule — three handling modes

- **Code = git-mechanical** (checkout/revert + redeploy).
- **Database = rule-gated** — revert ONLY via a paired down-migration or a pre-migration backup
  restore. **Never reconcile a database by judgment/opinion.** Classify each migration reversible
  (ship a `*.down.sql`) vs irreversible (mandatory backup-before-destructive; never hard-delete —
  use soft-delete).
- **KB/prose = judgment + human approval** (the reconciliation above).

## Why it matters

The instinct to "not roll back the KB with the code" (e.g. gitignoring the KB "to exclude it from
rollback") is the wrong fix — it just desyncs knowledge and breaks multi-machine sync. The right
fix is: keep KB in git, roll it back too, then deliberately carry forward the code-independent
value-adds. And databases are categorically different from text — they get rules, not opinions.

## How to apply

- Build it as a project-scoped `rollback` skill + a shell helper that tags the snapshot, performs
  the code/KB/SQL rollback, lists crossed migrations, and prints the KB diff for reconciliation.
- Back it with a standard: release tags (`release/<env>/<date>-<seq>`), a `schema_migrations` table
  per environment, a reversibility policy, and a code-to-schema compatibility note.
- Production rollbacks require explicit approval; rehearse in a sandbox before relying on it.
