> [!WARNING]
> This content is AI-generated. Verify before relying on it.

# Useful Git Commands

A cheat sheet of commonly used Git commands organized by workflow.

---

## Configuration & Identity

### View User Name and Email

Check the user identity currently configured for commits:

| Scope | Command |
| --- | --- |
| Current Repository | `git config user.name`<br>`git config user.email` |
| Global (System User) | `git config --global user.name`<br>`git config --global user.email` |
| View All Configurations | `git config --list --show-origin` |

### Set User Name and Email

```bash
# Set globally for all repositories
git config --global user.name "Your Name"
git config --global user.email "you@example.com"

# Set locally for the current repository only
git config user.name "Your Name"
git config user.email "you@example.com"
```

### View Remote URLs

```bash
# View remote repository names and fetch/push URLs
git remote -v
```

---

## Inspecting History & Logs

### `git log --oneline -5`

Shows the last 5 commits formatted on a single line each (abbreviated hash + commit message):

```bash
git log --oneline -5
```

### Show Commits by a Specific Author

Filter the commit history to only show commits authored by a specific person (matches name or email substring):

```bash
# Filter by author name or email
git log --author="vachan"

# Combine with oneline and limit to 5 commits
git log --author="vachan" --oneline -5
```

### Visual Commit Graph

```bash
# Visual ASCII graph of branching and merging
git log --oneline --graph --decorate -10
```

---

## Viewing Differences (Diffs)

### `git diff` vs `git diff --staged`

| Command | Compares | What it Shows |
| --- | --- | --- |
| `git diff` | Working directory vs. Staging area (Index) | Unstaged changes you haven't run `git add` on yet |
| `git diff --staged`<br>*(or `git diff --cached`)* | Staging area (Index) vs. Last commit (`HEAD`) | Changes already staged with `git add` that will be included in the next commit |

```bash
# View unstaged changes
git diff

# View unstaged changes for a specific file
git diff path/to/file.ext

# View staged changes waiting to be committed
git diff --staged
```

> [!TIP]
> Always run `git diff --staged` before `git commit` to do a final self-review of what is about to be recorded.

---

## Branch Management

### Deleting a Branch

| Target | Command | Notes |
| --- | --- | --- |
| Local branch (Safe) | `git branch -d <branch-name>` | Prevents deletion if branch contains unmerged commits |
| Local branch (Force) | `git branch -D <branch-name>` | Forces deletion regardless of merge status |
| Remote branch | `git push origin --delete <branch-name>` | Deletes branch from remote server (GitHub/GitLab) |

```bash
# Delete a local merged feature branch
git branch -d feature-login

# Force-delete an abandoned local branch
git branch -D test-experiment

# Delete the branch from the remote repository
git push origin --delete feature-login
```

### Sort Branches by Update Date

List branches ordered by the date of their latest commit (most recently updated branch first):

```bash
# Sort local branches by commit date (most recent first)
git branch --sort=-committerdate

# Include human-readable relative dates in the output
git branch --sort=-committerdate --format="%(committerdate:relative)%09%(refname:short)"

# Sort including remote tracking branches
git branch -a --sort=-committerdate
```

### Moving a Commit from One Branch to Another

To move a commit (e.g. `abc1234`) committed to the wrong branch (`branch-A`) onto the correct branch (`branch-B`):

```bash
# 1. Switch to the target branch
git checkout branch-B

# 2. Cherry-pick the commit onto branch-B
git cherry-pick abc1234

# 3. Switch back to the original branch
git checkout branch-A

# 4. Remove the commit from branch-A (if it was the most recent commit)
git reset --hard HEAD~1
```

> [!CAUTION]
> Only use `git reset --hard` if you have not pushed `branch-A` to a shared remote repository. If already pushed, use `git revert abc1234` instead.

---

## Stashing Changes

Use `git stash` to set aside uncommitted changes and return to a clean working directory.

```bash
# Stash tracked modified files
git stash

# Stash including untracked files with a descriptive message
git stash push -u -m "WIP: navbar redesign"

# List all saved stashes
git stash list
```

### Difference Between `git stash pop` and `git stash apply`

| Feature | `git stash pop` | `git stash apply` |
| --- | --- | --- |
| **Applies changes** | Yes | Yes |
| **Removes from stash list** | **Yes** (drops `stash@{0}`) | **No** (retains entry on the stack) |
| **Best used when** | You are done with the stash and want it consumed | You want to apply the same stash to multiple branches |

```bash
# Apply and delete the latest stash from the list
git stash pop

# Apply changes while keeping the stash intact
git stash apply

# Apply a specific stash from the list without deleting it
git stash apply stash@{2}

# Manually drop a stash entry
git stash drop stash@{0}

# Clear all stashes permanently
git stash clear
```

---

## Cleaning & Resetting

### `git clean -fd`

Removes untracked files and directories from your working tree.

```bash
# 1. Always run a dry run first to preview what would be deleted
git clean -nd

# 2. Force delete untracked files (-f) and untracked directories (-d)
git clean -fd

# Remove untracked files, directories, AND gitignored files
git clean -fdx
```

> [!WARNING]
> `git clean -fd` permanently deletes untracked files from disk. They cannot be recovered from Git history. Always use `git clean -nd` first.

### Resetting Commits (`--soft` vs `--hard`)

```bash
# Move HEAD back by 1 commit; all changes remain staged in the index
git reset --soft HEAD~1

# Move HEAD back by 1 commit; all staged and unstaged changes are destroyed
git reset --hard HEAD~1
```

### Commit Shortcut: `git commit -am`

Stages all modified tracked files and commits them in a single step (does not add newly created untracked files):

```bash
git commit -am "Commit message"
```
