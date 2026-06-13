# AGENTS.md — Git Sample Lesson Repository

> This file provides agent-facing context for the `git-sample` repository.
> Instructions here take precedence over any generic conventions.

---

## Purpose

This repository is a **hands-on Git workflow demonstration and teaching tool**.
It simulates a real multi-contributor project with complex branching, merging,
conflict resolution, rebasing, cherry-picking, stashing, tagging, and multi-remote
configurations. It is designed as a reference repo for learning Git commands
and understanding what real-world Git history looks like.

**Do not treat this as production code.** The source files are intentionally
minimal stubs (e.g. `function login() {}`) meant only to generate commit diffs.

---

## Remotes

Two GitHub remotes are configured and actively pushed:

| Remote   | URL                                         | Role                  |
|----------|---------------------------------------------|-----------------------|
| `origin` | `git@github.com:lqdflying/git-sample1.git`  | Primary / your fork   |
| `upstream` | `git@github.com:lqdflying/git-sample2.git`| Upstream / original   |

Both remotes contain identical branch and tag state.

### Push Policy
When adding new simulated content, push **all branches and tags to both remotes**:

```bash
git push origin --all && git push origin --tags
git push upstream --all && git push upstream --tags
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

## Branching Strategy (Git Flow Lite)

| Branch Prefix     | Purpose                                   | Merge Target    |
|-------------------|-------------------------------------------|-----------------|
| `master`          | Production releases                       | —               |
| `develop`         | Integration branch for features           | —               |
| `feature/*`       | New features / experiments                | `develop`       |
| `hotfix`          | Critical production fixes                 | `master`        |
| `security-patch-*`| Security patches (hotfix variant)         | `master` + `develop` |
| `release/v*.*.*`  | Release preparation (version bump, docs)  | `master`        |

### Merge Conventions
- Use `--no-ff` for merges into `master` and `develop` to preserve branch topology.
- Use `--ff-only` only when explicitly demonstrating fast-forward merges.
- Use `--squash` when demonstrating squash-merge workflows.
- Merge commit messages should mimic GitHub PR merge messages:
  ```
  Merge pull request #N from lqdflying/branch-name

  feat(scope): description
  ```

---

## Tags

All releases are annotated tags (`git tag -a`) on `master` or `develop`:

| Tag           | Target    | Description                        |
|---------------|-----------|------------------------------------|
| `v0.9.0`      | `fdaa65b` | Pre-release before feature merges  |
| `v1.0.0`      | `master`  | MVP with auth and payment          |
| `v1.1.0`      | `master`  | First develop → master release     |
| `v1.1.0-beta` | `develop` | Beta snapshot                      |
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

## Worktree

A secondary worktree exists at `../git-sample-worktree` tracking `feature-a`.

```bash
# List worktrees
git worktree list

# Add a new worktree (if needed for demonstrations)
git worktree add ../git-sample-worktree-<branch> <branch>
```

---

## Agent Guidelines

1. **Preserve the simulation nature** — do not refactor code for correctness.
   The `src/*.js` files are intentionally minimal stubs.

2. **Always simulate realistic authorship** — override `GIT_AUTHOR_*` when
   committing new simulated work.

3. **Maintain merge commit style** — PR-style merge messages with `#N` numbering.

4. **Keep the README updated** — if new workflows are added, document them
   in `README.md`.

5. **Push both remotes** — never leave commits unpushed on GitHub.

6. **Branch cleanup** — do not delete feature branches locally; they serve as
   teaching artifacts visible on GitHub.

---

## Common Commands for This Repo

```bash
# Full graph
git log --oneline --graph --decorate --all

# Branches with upstream tracking
git branch -vv

# Reflog (safety net)
git reflog

# Diff between master and develop
git diff master..develop --stat

# Blame on a simulated file
git blame src/auth.js

# Show notes
git log --notes --oneline
```

---

## External References

- **GitHub repos**: `lqdflying/git-sample1`, `lqdflying/git-sample2`
- **GitHub CLI (`gh`)**: Used for PR simulation where possible; auth required
  for live `gh pr create`.
