> [!WARNING]
> This content is AI-generated. Verify before relying on it.

# Git Worktree

`git worktree` lets you have multiple working trees attached to the same Git repository. This allows you to check out and work on multiple branches simultaneously in separate folders, without cloning the repository again or interrupting your current work with `git stash`.

---

## Why Use Worktrees?

- **Urgent Hotfixes**: Fix an urgent production bug on `main` in a separate folder without stashing or switching branches in your current feature branch.
- **Side-by-Side Review**: Run, build, and test two different branches simultaneously (e.g. comparing local changes against `main`).
- **Heavy Builds**: Avoid rebuilding dependencies (e.g. `node_modules` or vendor assets) each time you switch branches.

---

## Common Worktree Commands

| Task | Command |
| --- | --- |
| List all active worktrees | `git worktree list` |
| Add a worktree for an existing branch | `git worktree add <path> <branch>` |
| Create a new branch in a new worktree | `git worktree add -b <new-branch> <path>` |
| Remove a worktree directory | `git worktree remove <path>` |
| Clean up metadata for deleted worktrees | `git worktree prune` |
| Lock a worktree to prevent pruning | `git worktree lock <path>` |
| Unlock a worktree | `git worktree unlock <path>` |

---

## Step-by-Step Workflow Example

### 1. Create a Worktree for a Hotfix

Suppose you are working on `feature-dashboard` inside `/home/user/til` and need to quickly fix a bug on `main`:

```bash
# Creates a folder named 'til-hotfix' one directory up, tracking the 'main' branch
git worktree add ../til-hotfix main
```

Git creates the directory `../til-hotfix` with a clean checkout of `main`.

### 2. Check Active Worktrees

```bash
git worktree list
```

Output:
```text
/home/user/til         a1b2c3d [feature-dashboard]
/home/user/til-hotfix  e4f5a6b [main]
```

### 3. Work and Commit in the New Directory

```bash
cd ../til-hotfix
# Edit files, test, and commit
git commit -am "Fix urgent regression"
git push origin main
```

### 4. Remove the Worktree When Done

Return to your original working directory and delete the hotfix worktree:

```bash
cd /home/user/til
git worktree remove ../til-hotfix
```

> [!NOTE]
> Git will prevent you from checking out the same branch in more than one worktree at the same time to prevent conflicts.

> [!TIP]
> If you manually deleted a worktree directory using `rm -rf`, run `git worktree prune` to clear its reference from Git's internal metadata.
