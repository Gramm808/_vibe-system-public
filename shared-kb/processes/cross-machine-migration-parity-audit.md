# Cross-machine migration parity audit

When rebuilding/migrating onto a new machine, work can be stranded where you don't expect — and
the gap is silent. Two concrete traps that surface during a primary-dev-machine migration (old
machine → new dedicated box):

1. **Unpushed local branch.** An entire feature (plus follow-on work) can live only on the old
   machine's local `feat/*` branch — **never pushed**. The new machine clones `main` (possibly
   weeks stale) and silently lacks all of it. The feature *looks* present in the design docs but
   isn't on the new machine.
2. **Repo with no remote.** A private repo may have **no git remote at all**, so it can't reach the
   new machine by pulling. It has to be given a (private) remote first.

## Why it matters

"It's in git" ≠ "it's everywhere." Unpushed branches and per-machine clones diverge invisibly, and
a no-remote repo doesn't travel. During a migration this can quietly leave the new primary machine
missing a whole feature — discovered only by accident.

## How to apply

Before assuming parity during ANY machine migration, audit and compare across machines **first**:
- `git branch -vv` on the old machine — look for unmerged/unpushed `feat/*` branches holding real work.
- `git ls-remote --heads origin` — confirm what's actually on the remote vs local-only.
- Compare `git log --oneline -1` (HEAD) per repo on each machine.
- For each repo, confirm it **has a remote** (`git remote -v`); a no-remote private repo needs a
  private remote (e.g. a private GitHub repo) before it can sync to the new machine.
- Resolve stranded work (push/merge to the mainline the new machine tracks) before declaring the
  migration's foundation phase complete.
