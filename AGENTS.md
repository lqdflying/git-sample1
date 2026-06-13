# AGENTS.md — Git Sample Lesson Repository

> This file provides agent-facing context for the `git-sample` repository.
> Instructions here take precedence over any generic conventions.

---

## Purpose

This repository is a **hands-on Git workflow demonstration and teaching tool**.
It simulates real-world Git scenarios: multi-remote setups, branching with
varying remote presence, merging (fast-forward & 3-way), conflict resolution,
rebasing, cherry-picking, stashing, tagging, and ahead/behind remote states.

**Do not treat this as production code.** The source files are intentionally
minimal stubs (e.g. `function login() {}`) meant only to generate commit diffs.

---

## Remotes

Two GitHub remotes are configured and actively pushed:

| Remote   | URL                                         | Role                  |
|----------|---------------------------------------------|----------------------|
| `origin` | `git@github.com:lqdflying/git-sample1.git`  | Primary / your fork   |
| `upstream` | `git@github.com:lqdflying/git-sample2.git`| Upstream / original   |

### Push Policy
- `master` is kept in sync on **both** remotes.
- Other branches may be selectively pushed to demonstrate remote presence patterns.

```bash
# Push all local branches to a remote
git push origin --all

# Push tags
git push origin --tags
```

---

## Branch Layout (Current — 6 Branches)

The repo demonstrates **6 branches** covering remote presence and sync states:

| Branch | Local | `origin` | `upstream` | Lesson |
|--------|-------|----------|------------|--------|
| `master` | ✅ | ✅ | ✅ | Multi-remote sync |
| `feature/single-remote` | ✅ | ✅ | ❌ | Single-remote branch |
| `feature/local-only` | ✅ | ❌ | ❌ | Local-only branch |
| `feature/remote-only` | ❌ | ✅ | ❌ | Remote-only branch (no local) |
| `feature/local-ahead-origin` | ✅ | ✅ `[behind 2]` | ✅ (tally) | Local ahead of origin |
| `feature/remote-ahead-origin` | ✅ | ✅ `[ahead 2]` | ✅ (tally) | Remote ahead of local |

### Checking remote presence and sync
```bash
git branch -a          # All branches (local + remote)
git branch -vv         # Local branches with tracking + ahead/behind
git branch -r          # Remote branches only
```

### Checkout a remote-only branch locally
```bash
git checkout -b feature/remote-only origin/feature/remote-only
```

### Pull when remote is ahead
```bash
git checkout feature/remote-ahead-origin
git pull origin feature/remote-ahead-origin
```

### Push when local is ahead
```bash
git push origin feature/local-ahead-origin
```

---

## Simulated Contributors

Commits use `GIT_AUTHOR_*` / `GIT_COMMITTER_*` overrides to simulate a team:

| Name       | Email                 | Role in Simulations              |
|------------|-----------------------|----------------------------------|
| Alice Chen | `alice@example.com`   | Backend, docs, CI, auth, i18n    |
| Bob Smith  | `bob@example.com`     | Features, utilities, tests       |
| Carol Wu   | `carol@example.com`   | Refactoring, reviews, releases   |
| Demo User  | `demo@example.com`    | Initial scaffold commits         |

Use the same pattern when creating new simulated commits:

```bash
GIT_AUTHOR_NAME="Alice Chen" GIT_AUTHOR_EMAIL="alice@example.com" \
GIT_COMMITTER_NAME="Alice Chen" GIT_COMMITTER_EMAIL="alice@example.com" \
git commit -m "type(scope): description"
```

---

## Tags

All releases are annotated tags (`git tag -a`) on `master`:

| Tag           | Target    | Description                        |
|---------------|-----------|------------------------------------|
| `v0.9.0`      | `fdaa65b` | Pre-release before feature merges  |
| `v1.0.0`      | `master`  | MVP with auth and payment          |
| `v1.1.0`      | `master`  | First develop → master release     |
| `v1.1.0-beta` | old `develop` | Beta snapshot                |
| `v1.2.0`      | `master`  | i18n, audit log, rate limiting     |

When creating a new release tag, use `-a` and push to both remotes.

---

## Git Notes

Some merge commits have `git notes` attached to simulate PR metadata
(e.g. `CI passed ✅ | 2 approvals | merged by Carol`).

```bash
# Add a note to a commit
git notes add -m "CI passed ✅ | N approvals" <hash>

# Push notes
git push origin refs/notes/*
```

---

## Agent Guidelines

1. **Preserve the simulation nature** — do not refactor code for correctness.
   The `src/*.js` files are intentionally minimal stubs.

2. **Always simulate realistic authorship** — override `GIT_AUTHOR_*` when
   committing new simulated work.

3. **Keep to 6 branches** — if adding new scenarios, replace an existing
   feature branch rather than proliferating branches.

4. **Keep the README updated** — if new workflows are added, document them
   in `README.md`.

5. **Push appropriately** —
   - `master` → both remotes
   - `feature/single-remote` → `origin` only
   - `feature/local-only` → do not push (demonstrates local-only)
   - `feature/remote-only` → push then delete local (demonstrates remote-only)
   - `feature/local-ahead-origin` → push v3 to upstream, v1 to origin
   - `feature/remote-ahead-origin` → push v1 to upstream, v3 to origin, reset local to v1

6. **Always update AGENTS.md whenever the branch diagram changes** —
   If you create, delete, rename, or restructure branches, update the
   "Branch Layout" section in this file and the branch table in `README.md`
   in the same commit (or immediately after).

---

## Common Commands for This Repo

```bash
# Full graph
git log --oneline --graph --decorate --all

# Branches with upstream tracking
git branch -vv

# Reflog (safety net)
git reflog

# Blame on a simulated file
git blame src/auth.js

# Show notes
git log --notes --oneline

# See commits a local branch has that origin doesn't
git log origin/feature/single-remote..feature/single-remote --oneline
```

---

## External References

- **GitHub repos**: `lqdflying/git-sample1`, `lqdflying/git-sample2`
- **GitHub CLI (`gh`)**: Used for PR simulation where possible; auth required
  for live `gh pr create`.
