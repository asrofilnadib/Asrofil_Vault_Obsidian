	## Commit Message Conventions
	
| Prefix     | Usage                        | Description                                        |
| ---------- | ---------------------------- | -------------------------------------------------- |
| `chore`    | `chore: update dependencies` | Perubahan kecil yang tidak berpengaruh ke aplikasi |
| `docs`     | `docs: update README`        | Perubahan terhadap dokumentasi                     |
| `style`    | `style: fix indentation`     | Mengubah format kode (tidak mengubah logika)       |
| `feat`     | `feat: add login feature`    | Menambahkan atau menghapus fitur                   |
| `fix`      | `fix: resolve login bug`     | Memperbaiki bug                                    |
| `refactor` | `refactor: optimize query`   | Mengubah kode tanpa mengubah fungsinya             |
| `test`     | `test: add unit tests`       | Penambahan/perubahan pada test                     |

**Example:**

```bash
git commit -m "feat: add user authentication"
git commit -m "fix: resolve memory leak in service"
git commit -m "docs: update API documentation"
```

---

## Repository Setup

### Clone Repository

```bash
# Clone repository
git clone <repo_url>

# Clone specific branch
git clone -b <branch_name> <repo_url>

# Clone into new folder
git clone -b <branch_name> <repo_url> <new_folder_name>
```

### Remote Management

```bash
# Add remote repository
git remote add <origin> <repo_url>

# Remove remote repository
git remote remove <origin>

# View remote repositories
git remote -v

# Change remote URL
git remote set-url <origin> <new_repo_url>

# Set default branch
git remote set-head origin <branch_name>
```

---

## Branch Management

### View Branches

```bash
# View local branches
git branch

# View remote branches
git branch -r

# View all branches (local + remote)
git branch -a

# View branches with last commit
git branch -v
```

### Create & Switch Branches

```bash
# Create new branch
git branch <branch_name>

# Switch to branch
git checkout <branch_name>

# Create and switch to new branch
git checkout -b <branch_name>

# Switch to branch (newer syntax)
git switch <branch_name>

# Create and switch (newer syntax)
git switch -c <branch_name>
```

### Rename & Delete Branches

```bash
# Rename current branch
git branch -m <new_branch_name>

# Rename branch (not current)
git branch -m <old_name> <new_name>

# Set branch name for remote
git branch -M <branch_name>

# Delete local branch
git branch -d <branch_name>

# Force delete local branch
git branch -D <branch_name>

# Delete remote branch
git push origin --delete <branch_name>
```

---

## Working with Changes

### Staging & Committing

```bash
# Stage all changes
git add .

# Stage specific file
git add <file_name>

# Stage multiple files
git add <file1> <file2> <file3>

# Commit with message
git commit -m "commit message"

# Commit all changes (skip staging)
git commit -a -m "commit message"

# Amend last commit
git commit --amend -m "new message"

# Amend without changing message
git commit --amend --no-edit
```

### Viewing Changes

```bash
# View unstaged changes
git diff

# View staged changes
git diff --staged

# View changes in specific file
git diff <file_name>

# View changes between branches
git diff <branch1>..<branch2>
```

### Status & History

```bash
# View working directory status
git status

# Compact status
git status -s

# View commit history
git log

# One-line log
git log --oneline

# View specific branch log
git log <origin>/<branch_name>

# View graph
git log --graph --oneline --all

# View last N commits
git log -n 5

# View commits by author
git log --author="username"

# Exit log view: press 'q'
```

---

## Synchronizing Changes

### Fetch & Pull

```bash
# Fetch from remote
git fetch <origin>

# Fetch all remotes
git fetch --all

# Pull from remote (fetch + merge)
git pull <origin> <branch_name>

# Pull with rebase
git pull <origin> <branch_name> --rebase
```

### Push

```bash
# Push to remote
git push <origin> <branch_name>

# Push all branches
git push --all

# Push with tags
git push --tags

# Force push (use with caution!)
git push --force

# Safer force push
git push --force-with-lease

# Set upstream branch
git push -u <origin> <branch_name>
```

---

## Undoing Changes

### Reset

```bash
# Undo last commit (keep changes)
git reset --soft HEAD~1

# Undo last commit (unstage changes)
git reset HEAD~1

# Undo last commit (discard changes)
git reset --hard HEAD~1

# Undo specific file
git reset HEAD <file_name>

# Reset to specific commit
git reset --soft <commit_hash>
```

### Revert

```bash
# Revert specific commit (creates new commit)
git revert <commit_hash>

# Revert without committing
git revert --no-commit <commit_hash>
```

### Stash

```bash
# Stash current changes
git stash

# Stash with message
git stash save "message"

# List stashes
git stash list

# Apply last stash
git stash apply

# Apply specific stash
git stash apply stash@{2}

# Apply and remove stash
git stash pop

# Remove stash
git stash drop stash@{2}

# Clear all stashes
git stash clear
```

### Discard Changes

```bash
# Discard unstaged changes in file
git checkout -- <file_name>

# Discard all unstaged changes
git checkout -- .

# Clean untracked files (dry run)
git clean -n

# Clean untracked files
git clean -f

# Clean including directories
git clean -fd
```

---

## Merging & Rebasing

### Merge

```bash
# Merge branch into current
git merge <branch_name>

# Merge without fast-forward
git merge --no-ff <branch_name>

# Abort merge
git merge --abort
```

### Rebase

```bash
# Rebase current branch
git rebase <branch_name>

# Interactive rebase (last 3 commits)
git rebase -i HEAD~3

# Continue rebase after resolving conflicts
git rebase --continue

# Skip current commit in rebase
git rebase --skip

# Abort rebase
git rebase --abort
```

### Handling Conflicts

```bash
# When conflicts occur:
# 1. Resolve conflicts in files
# 2. Stage resolved files
git add <file_name>

# 3. Continue merge/rebase
git merge --continue
# or
git rebase --continue
```

---

## Configuration

### User Configuration

```bash
# Set username (global)
git config --global user.name "Your Name"

# Set email (global)
git config --global user.email "your.email@example.com"

# Set username (local repo)
git config user.name "Your Name"

# Set credential username
git config credential.username "username"

# View all configs
git config --list

# View specific config
git config user.name
```

### Credential Management

```bash
# Configure GitHub credentials
git config --global credential.https://github.com.username <username>

# Use credential manager for GitHub
git credential-manager github --help

# Set remote URL with token
git remote set-url origin https://<username>:<TOKEN>@github.com/<username>/<repo>.git

# Cache credentials (15 min default)
git config --global credential.helper cache

# Cache for specific time (1 hour)
git config --global credential.helper 'cache --timeout=3600'

# Store credentials permanently (less secure)
git config --global credential.helper store
```

---

## Advanced Operations

### Tags

```bash
# List tags
git tag

# Create tag
git tag <tag_name>

# Create annotated tag
git tag -a v1.0.0 -m "Version 1.0.0"

# Tag specific commit
git tag <tag_name> <commit_hash>

# Push tag to remote
git push origin <tag_name>

# Push all tags
git push origin --tags

# Delete local tag
git tag -d <tag_name>

# Delete remote tag
git push origin --delete <tag_name>
```

### Cherry-pick

```bash
# Apply specific commit to current branch
git cherry-pick <commit_hash>

# Cherry-pick multiple commits
git cherry-pick <commit1> <commit2>

# Cherry-pick without committing
git cherry-pick --no-commit <commit_hash>
```

### Submodules

```bash
# Add submodule
git submodule add <repo_url> <path>

# Initialize submodules
git submodule init

# Update submodules
git submodule update

# Clone with submodules
git clone --recursive <repo_url>
```

---

## Useful Aliases

Add these to `~/.gitconfig`:

```ini
[alias]
    st = status
    co = checkout
    br = branch
    ci = commit
    unstage = reset HEAD --
    last = log -1 HEAD
    visual = log --graph --oneline --all
    amend = commit --amend --no-edit
    undo = reset --soft HEAD~1
```

**Usage:**

```bash
git st          # Instead of git status
git co main     # Instead of git checkout main
git visual      # View commit graph
```

---

## Quick Reference

### Daily Workflow

```bash
# 1. Start work
git pull origin main

# 2. Make changes and stage
git add .

# 3. Commit changes
git commit -m "feat: add new feature"

# 4. Push to remote
git push origin main
```

### Handling Push Conflicts

```bash
# If remote has changes you don't have:
git pull origin main --rebase

# If conflicts occur:
# 1. Resolve conflicts in files
# 2. Stage resolved files
git add .

# 3. Continue rebase
git rebase --continue

# 4. Push changes
git push origin main
```

---

## Best Practices

✅ **Do:**

- Write clear, descriptive commit messages
- Commit frequently with logical changes
- Pull before pushing
- Use branches for features
- Review changes before committing (`git diff`)

❌ **Don't:**

- Force push to shared branches
- Commit sensitive data (passwords, keys)
- Make huge commits with unrelated changes
- Commit directly to main/master in team projects
- Ignore merge conflicts

---

## Emergency Commands

```bash
# Lost your changes? Check reflog
git reflog

# Recover deleted branch
git checkout -b <branch_name> <commit_hash_from_reflog>

# Abort everything
git reset --hard HEAD

# Go back to last known good state
git reset --hard origin/<branch_name>
```