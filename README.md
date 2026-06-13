# 🎓 Git Sample — Complete Git Workflow Lesson Repo

This repository is a hands-on reference for learning real-world Git workflows: multi-remote setups, branching strategies, merging (fast-forward & 3-way), conflict resolution, rebasing, stashing, cherry-picking, tagging, and GitHub CLI (`gh`) integration.

---

## 📡 Multi-Remote Setup

| Remote   | URL                                            | Typical Use          |
|----------|------------------------------------------------|----------------------|
| `origin` | `git@github.com:lqdflying/git-sample1.git`     | Your fork / primary  |
| `upstream` | `git@github.com:lqdflying/git-sample2.git`   | Original repo        |

```bash
# View remotes
git remote -v

# Fetch all remotes
git fetch origin
git fetch upstream

# Push specific branches to specific remotes
git push origin master
git push upstream develop
```

---

## 🌿 Branching Strategy (Git Flow Lite)

```
master          ─────●─────●────────────────────────●─────●─────► production
                    │     │                        │     │
develop     ────────┘     └────────────────────────┘     │
                          │                              │
feature-a   ─────●──●─────┘                              │
feature-b   ─────●──●────────────────────────────────────┘
hotfix      ─────●───────┘
release/v1.1.0 ──────────┘
```

| Branch              | Purpose                                    |
|---------------------|--------------------------------------------|
| `master`            | Production-ready code                      |
| `develop`           | Integration branch for features            |
| `feature/*`         | Individual feature development             |
| `hotfix`            | Critical production fixes                  |
| `release/*`         | Release preparation (version bumps, docs)  |

---

## 👥 Simulated Contributors

Every commit in this repo simulates a real team with different authors:

- **Alice Chen** (`alice@example.com`) — Backend, docs, CI
- **Bob Smith** (`bob@example.com`) — Features, utilities, tests
- **Carol Wu** (`carol@example.com`) — Refactoring, reviews, releases

### Simulate another contributor
```bash
GIT_AUTHOR_NAME="Alice Chen" GIT_AUTHOR_EMAIL="alice@example.com" \
GIT_COMMITTER_NAME="Alice Chen" GIT_COMMITTER_EMAIL="alice@example.com" \
git commit -m "feat: your message"
```

---

## 🔀 Pull Request Simulation

Since `gh pr create` requires GitHub auth, PRs are simulated via **merge commits with PR-style messages**:

```bash
# Simulate merging a PR
$ git merge --no-ff feature/user-auth -m "Merge pull request #1 from lqdflying/feature/user-auth

feat(auth): user authentication module"
```

### Simulated PRs

| PR  | Branch                  | Status   | Notes                          |
|-----|-------------------------|----------|--------------------------------|
| #1  | `feature/user-auth`     | ✅ Merged| Auth module (login/logout/reg) |
| #2  | `feature/payment`       | ✅ Merged| Had merge conflict, resolved   |
| #3  | `feature/notifications` | ✅ Merged| Squashed messy commits         |
| #4  | `feature/api-docs`      | ✅ Merged| Fast-forward merged later      |
| #5  | `release/v1.1.0`        | ✅ Merged| Release branch into master     |

### GitHub CLI (gh) Quick Reference

```bash
# Check gh status
gh auth status

# List PRs (requires auth)
gh pr list

# Create a PR (requires auth + push)
gh pr create --title "feat: new feature" --body "Description..."

# View PR status
gh pr view 1

# Merge a PR via CLI
gh pr merge 1 --squash
```

---

## ⚔️ Merge Conflict Resolution

A realistic conflict was created between `develop` and `feature/payment` on `app.js`.

### How to view the conflict resolution
```bash
# See the merge commit
git show 838eacb

# During conflict, Git shows:
<<<<<<< HEAD
// Analytics module
=======
// Payment gateway integration v2
>>>>>>> feature/payment

# Resolution: keep both lines
```

### Cherry-pick conflict
Cherry-picking the security patch to `master` also triggered a modify/delete conflict on `.env.example`.

```bash
# Cherry-pick a commit
git cherry-pick <hash>

# If conflict, resolve then:
git add <file>
git cherry-pick --continue
```

---

## 🔄 Interactive Rebase & Amend

The `feature/notifications` branch originally had messy commits:
```
feat(notif): add notifictions
feat(notif): email
feat(notif): sms
fix: typo
wip
```

### Cleaned up via soft reset + recommit
```bash
# Squash last 2 commits into 1
git reset --soft HEAD~2
git commit -m "feat(notif): add email and SMS notifications"

# Amend last commit
git commit --amend -m "feat(notif): add push notifications with config"
```

### Interactive rebase (full control)
```bash
# Rebase last 5 commits
git rebase -i HEAD~5

# Common commands in the editor:
# p, pick     = use commit
# r, reword   = use commit, edit message
# s, squash   = meld into previous commit
# f, fixup    = like squash, discard message
# d, drop     = remove commit
```

---

## 🍒 Cherry-Pick

```bash
# Apply a specific commit to current branch
git cherry-pick ef43506

# Useful for:
# - Hotfixing production without merging entire branch
# - Backporting fixes to older releases
```

---

## ↩️ Revert

A bad commit (`chore: add debug log`) was made and immediately reverted:

```bash
# Revert last commit (creates a new undo commit)
git revert HEAD

# Revert a specific commit
git revert <hash>
```

---

## 📦 Stash Workflow

```bash
# Save WIP with a message
git stash push -m "WIP: dashboard redesign"

# Do other work, then restore
git stash pop

# List stashes
git stash list

# Apply without removing from stash
git stash apply stash@{0}
```

---

## 🏷️ Tagging & Releases

```bash
# Lightweight tag
git tag v0.9.0 <hash>

# Annotated tag (recommended)
git tag -a v1.0.0 -m "Release v1.0.0 - MVP with auth and payment"

# Push tags to remote
git push origin --tags

# Push a specific tag
git push origin v1.1.0
```

### Simulated Tags

| Tag          | Target   | Description                          |
|--------------|----------|--------------------------------------|
| `v0.9.0`     | `fdaa65b`| Pre-release before feature merges    |
| `v1.0.0`     | `master` | MVP with auth and payment            |
| `v1.1.0`     | `master` | Full release after develop merge     |
| `v1.1.0-beta`| `develop`| Beta snapshot                        |

---

## 📝 Git Notes (Simulating PR Comments)

Git notes attach metadata to commits without changing history:

```bash
# Add a note to HEAD
git notes add -m "CI passed ✅ | 2 approvals"

# Show notes
git log --notes

# Push notes to remote
git push origin refs/notes/*
```

---

## 🌳 Git Worktree

A linked worktree was created for parallel branch work:

```bash
# Create a worktree for feature-a at ../git-sample-worktree
git worktree add ../git-sample-worktree feature-a

# List worktrees
git worktree list

# Remove worktree when done
git worktree remove ../git-sample-worktree
```

---

## 🚀 Complete Commit Graph

```bash
git log --oneline --graph --decorate --all
```

Run the command above to see the full visual history including:
- Fast-forward merges (`feature-a` → `master`)
- 3-way merges (`feature-b`, `hotfix`)
- Merge commits with conflict resolution
- Cherry-picks
- Reverts
- Release branch merges

---

## 📂 Simulated Files

```
.
├── .env.example              # Environment variables (cherry-picked to master)
├── .eslintrc                 # Linting config
├── .github-ci.yml            # CI/CD pipeline
├── .github/
│   └── pull_request_template.md
├── .gitignore                # Standard ignore rules
├── CHANGELOG.md              # Release notes
├── README.md                 # This file
├── app.js                    # Main app (has resolved merge conflict)
├── dashboard-wip.html        # From stash workflow
├── develop.txt               # Develop branch marker
├── feature-a.txt             # Feature A artifact
├── feature-b.txt             # Feature B artifact
├── hotfix.txt                # Hotfix artifact
├── src/
│   ├── auth.js               # Auth module (PR #1)
│   ├── cache.js              # Caching layer
│   ├── middleware.js         # Middleware pipeline
│   ├── notifications.js      # Notifications (PR #3 + review fixes)
│   ├── routes.js             # Route definitions
│   ├── search.js             # Search module (fast-forward)
│   └── utils/
│       ├── helper.js         # Helper utility
│       └── validator.js      # Input validator
├── test.txt                  # Placeholder test
└── version.txt               # v1.1.0 version file
```

---

## 🧪 Practice Exercises

Try these commands to explore the repo:

```bash
# 1. View all branches and their upstreams
git branch -vv

# 2. See who wrote each line in app.js
git blame app.js

# 3. Find which commit introduced a specific line
git log -S "Payment gateway" --oneline

# 4. View reflog (safety net)
git reflog

# 5. Show diff between master and develop
git diff master..develop --stat

# 6. Show merge commit details
git show --stat 838eacb

# 7. List commits by author
git log --author="Alice" --oneline

# 8. Show stash list
git stash list

# 9. Show notes
git log --format="%H %s" | head -10 | while read h s; do echo "== $s"; git notes show $h 2>/dev/null || echo "(no notes)"; done

# 10. Simulate a new feature branch
git checkout -b feature/my-feature develop
echo "my code" > myfile.txt
git add myfile.txt
git commit -m "feat: my feature"
```

---

## 📊 Ahead / Behind Remote Simulation

Several branches are intentionally out of sync with remotes to demonstrate `git branch -vv` and `git log` ahead/behind workflows:

```bash
# View all branches with ahead/behind counts
git branch -vv
```

### Ahead of Remote

| Branch | Tracking | Status | Explanation |
|--------|----------|--------|-------------|
| `feature/unpushed` | `origin/feature/unpushed` | `[ahead 3]` | 3 commits exist locally but not on `origin` |
| `feature/only-on-origin` | `upstream/feature/only-on-origin` | `[ahead 2]` | Exists on `origin` but has 2 extra commits vs `upstream` |
| `feature/only-on-upstream` | `origin/feature/only-on-upstream` | `[ahead 2]` | Exists on `upstream` but has 2 extra commits vs `origin` |
| `feature/mixed-sync` | `origin/feature/mixed-sync` | `[ahead 2]` | Part 2 pushed to `upstream` only; part 3 not pushed anywhere |

### Behind Remote

| Branch | Tracking | Status | Explanation |
|--------|----------|--------|-------------|
| `feature/behind-origin` | `origin/feature/behind-origin` | `[behind 2]` | Local reset to v1; origin has v2 and v3 |
| `feature/behind-upstream` | `upstream/feature/behind-upstream` | `[behind 2]` | Local reset to v1; upstream has v2 and v3 |

### Inspecting Ahead/Behind Manually

```bash
# Commits local has that origin doesn't
git log origin/feature/unpushed..feature/unpushed --oneline

# Commits origin has that local doesn't
git log feature/behind-origin..origin/feature/behind-origin --oneline

# Diff between local and a remote branch
git diff feature/mixed-sync origin/feature/mixed-sync --stat
```

### Syncing Strategies

```bash
# Push local ahead commits to tracked remote
git push origin feature/unpushed

# Pull remote commits when local is behind
git checkout feature/behind-origin
git pull origin feature/behind-origin

# Force push (destructive — only when you know what you're doing)
git push origin feature/behind-origin --force-with-lease

# Fetch all remotes to update local knowledge of remote state
git fetch --all
```

---

## 📖 Key Takeaways

| Scenario                  | Command Pattern                          |
|---------------------------|------------------------------------------|
| Start new feature         | `git checkout -b feature/x develop`      |
| Merge to develop          | `git checkout develop && git merge --no-ff feature/x` |
| Fast-forward merge        | `git merge feature/x --ff-only`          |
| Fix conflict              | Edit file → `git add .` → `git commit`   |
| Undo last commit (safe)   | `git revert HEAD`                        |
| Save WIP                  | `git stash push -m "msg"`                |
| Move commit to another branch | `git cherry-pick <hash>`             |
| Clean up commit history   | `git rebase -i HEAD~N`                   |
| Fix last commit message   | `git commit --amend`                     |
| Mark release              | `git tag -a v1.0.0 -m "Release"`         |
| Parallel branch work      | `git worktree add ../other feature/x`    |

---

## 🔗 Remotes Quick Reference

```bash
# Add remotes (already configured in this repo)
git remote add origin git@github.com:lqdflying/git-sample1.git
git remote add upstream git@github.com:lqdflying/git-sample2.git

# Push all branches to origin
git push origin --all

# Push develop to upstream
git push upstream develop

# Track a remote branch
git branch --set-upstream-to=origin/master master
```

---

Happy learning! 🚀
