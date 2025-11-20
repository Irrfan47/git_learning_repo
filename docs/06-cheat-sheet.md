# Git Cheat Sheet 📋

Quick reference for essential Git commands.

## Table of Contents

1. [Setup and Configuration](#setup-and-configuration)
2. [Creating Repositories](#creating-repositories)
3. [Making Changes](#making-changes)
4. [Branching](#branching)
5. [Remote Repositories](#remote-repositories)
6. [Viewing History](#viewing-history)
7. [Undoing Changes](#undoing-changes)
8. [Advanced Commands](#advanced-commands)

---

## Setup and Configuration

### Initial Setup
```bash
# Set username
git config --global user.name "Your Name"

# Set email
git config --global user.email "your.email@example.com"

# Set default editor
git config --global core.editor "vim"

# Set default branch name
git config --global init.defaultBranch main

# Enable color output
git config --global color.ui auto
```

### View Configuration
```bash
# List all settings
git config --list

# List with origin
git config --list --show-origin

# Get specific value
git config user.name

# Edit config file
git config --global --edit
```

---

## Creating Repositories

```bash
# Initialize new repository
git init

# Initialize with specific branch name
git init -b main

# Clone existing repository
git clone <url>

# Clone to specific directory
git clone <url> <directory>

# Clone specific branch
git clone -b <branch> <url>

# Clone with depth limit (shallow clone)
git clone --depth 1 <url>
```

---

## Making Changes

### Staging Files
```bash
# Stage specific file
git add <file>

# Stage multiple files
git add <file1> <file2>

# Stage all changes
git add .
git add -A

# Stage all modified files (not new)
git add -u

# Stage by patch (interactive)
git add -p
```

### Committing
```bash
# Commit staged changes
git commit -m "Commit message"

# Commit with detailed message (opens editor)
git commit

# Stage and commit in one step
git commit -am "Message"

# Amend last commit
git commit --amend

# Amend without changing message
git commit --amend --no-edit

# Empty commit (no changes)
git commit --allow-empty -m "Empty commit"
```

### Viewing Changes
```bash
# Show unstaged changes
git diff

# Show staged changes
git diff --staged
git diff --cached

# Show changes in specific file
git diff <file>

# Compare branches
git diff <branch1>..<branch2>

# Compare commits
git diff <commit1> <commit2>

# Show only file names
git diff --name-only

# Show statistics
git diff --stat
```

### Status
```bash
# Show status
git status

# Short format
git status -s
git status --short

# Show branch and tracking info
git status -sb
```

---

## Branching

### Creating and Switching
```bash
# List branches
git branch

# List all branches (including remote)
git branch -a

# Create new branch
git branch <branch-name>

# Create and switch to branch
git checkout -b <branch-name>
git switch -c <branch-name>

# Switch to branch
git checkout <branch-name>
git switch <branch-name>

# Switch to previous branch
git checkout -
git switch -
```

### Merging and Deleting
```bash
# Merge branch into current
git merge <branch>

# Merge with no fast-forward
git merge --no-ff <branch>

# Merge and squash commits
git merge --squash <branch>

# Abort merge
git merge --abort

# Delete branch
git branch -d <branch>

# Force delete branch
git branch -D <branch>

# Delete remote branch
git push origin --delete <branch>
```

### Renaming
```bash
# Rename current branch
git branch -m <new-name>

# Rename specific branch
git branch -m <old-name> <new-name>
```

---

## Remote Repositories

### Managing Remotes
```bash
# List remotes
git remote

# List with URLs
git remote -v

# Add remote
git remote add <name> <url>

# Remove remote
git remote remove <name>

# Rename remote
git remote rename <old> <new>

# Change URL
git remote set-url <name> <url>

# Show remote details
git remote show <name>
```

### Fetching and Pulling
```bash
# Fetch from remote
git fetch

# Fetch specific remote
git fetch <remote>

# Fetch specific branch
git fetch <remote> <branch>

# Fetch all remotes
git fetch --all

# Fetch and prune deleted branches
git fetch --prune

# Pull from remote
git pull

# Pull specific remote/branch
git pull <remote> <branch>

# Pull with rebase
git pull --rebase
```

### Pushing
```bash
# Push to remote
git push

# Push specific branch
git push <remote> <branch>

# Push and set upstream
git push -u <remote> <branch>

# Push all branches
git push --all

# Push tags
git push --tags

# Force push (dangerous!)
git push --force

# Force push (safer)
git push --force-with-lease

# Delete remote branch
git push <remote> --delete <branch>
```

---

## Viewing History

### Basic Logs
```bash
# Show commit history
git log

# One line per commit
git log --oneline

# Show graph
git log --graph

# All branches with graph
git log --oneline --graph --all

# Last N commits
git log -n <number>
git log -5

# With diffs
git log -p

# With stats
git log --stat
```

### Filtering Logs
```bash
# By author
git log --author="Name"

# By date
git log --since="2 weeks ago"
git log --after="2024-01-01"
git log --before="2024-12-31"

# By message
git log --grep="bug fix"

# By file
git log -- <file>

# By content change
git log -S "function_name"
git log -G "regex_pattern"
```

### Showing Commits
```bash
# Show specific commit
git show <commit>

# Show last commit
git show HEAD

# Show file from commit
git show <commit>:<file>

# Show commit stats
git show --stat <commit>
```

### Other History Commands
```bash
# Show who changed each line
git blame <file>

# Show reflog
git reflog

# Show branch history
git log <branch>

# Show differences between branches
git log <branch1>..<branch2>
```

---

## Undoing Changes

### Unstaging and Discarding
```bash
# Unstage file
git reset HEAD <file>
git restore --staged <file>

# Discard changes in file
git checkout -- <file>
git restore <file>

# Discard all changes
git reset --hard HEAD

# Remove untracked files (dry run)
git clean -n

# Remove untracked files
git clean -f

# Remove untracked files and directories
git clean -fd
```

### Resetting Commits
```bash
# Undo last commit, keep changes staged
git reset --soft HEAD~1

# Undo last commit, keep changes unstaged
git reset HEAD~1
git reset --mixed HEAD~1

# Undo last commit, discard changes
git reset --hard HEAD~1

# Reset to specific commit
git reset --hard <commit>
```

### Reverting
```bash
# Revert last commit
git revert HEAD

# Revert specific commit
git revert <commit>

# Revert without committing
git revert -n <commit>

# Revert merge commit
git revert -m 1 <merge-commit>
```

---

## Advanced Commands

### Stashing
```bash
# Stash changes
git stash

# Stash with message
git stash save "message"

# Stash including untracked
git stash -u

# List stashes
git stash list

# Show stash
git stash show
git stash show -p

# Apply stash
git stash apply
git stash apply stash@{0}

# Apply and remove stash
git stash pop

# Remove stash
git stash drop stash@{0}

# Clear all stashes
git stash clear

# Create branch from stash
git stash branch <branch-name>
```

### Cherry-Picking
```bash
# Apply specific commit
git cherry-pick <commit>

# Cherry-pick multiple commits
git cherry-pick <commit1> <commit2>

# Cherry-pick range
git cherry-pick <start>..<end>

# Cherry-pick without committing
git cherry-pick -n <commit>

# Continue after conflict
git cherry-pick --continue

# Abort cherry-pick
git cherry-pick --abort
```

### Rebasing
```bash
# Rebase on branch
git rebase <branch>

# Interactive rebase
git rebase -i HEAD~<n>
git rebase -i <commit>

# Continue after resolving conflicts
git rebase --continue

# Skip current commit
git rebase --skip

# Abort rebase
git rebase --abort
```

### Tags
```bash
# List tags
git tag

# Create lightweight tag
git tag <tag-name>

# Create annotated tag
git tag -a <tag-name> -m "message"

# Tag specific commit
git tag <tag-name> <commit>

# Show tag
git show <tag-name>

# Push tag
git push origin <tag-name>

# Push all tags
git push --tags

# Delete local tag
git tag -d <tag-name>

# Delete remote tag
git push origin --delete <tag-name>

# Checkout tag
git checkout <tag-name>
```

### Searching
```bash
# Search in files
git grep "search term"

# Search in specific file type
git grep "search" -- "*.js"

# Search with line numbers
git grep -n "search"

# Search for whole word
git grep -w "search"

# Count matches
git grep -c "search"
```

### Other Useful Commands
```bash
# Show differences between working tree and index
git diff-index HEAD

# Show commit graph
git log --graph --oneline --all

# Archive repository
git archive --format=zip HEAD > archive.zip

# Find commits that introduced/removed string
git log -S "string"

# Find when file was deleted
git log --all --full-history -- <file>

# Show file at specific commit
git show <commit>:<file>

# Count commits
git rev-list --count HEAD

# Check repository integrity
git fsck

# Garbage collection
git gc

# Optimize repository
git gc --aggressive
```

---

## Common Aliases

Add these to your `.gitconfig` or use `git config --global alias.<name> <command>`:

```bash
# Status shorthand
git config --global alias.st status

# Checkout shorthand
git config --global alias.co checkout

# Branch shorthand
git config --global alias.br branch

# Commit shorthand
git config --global alias.ci commit

# Pretty log
git config --global alias.lg "log --oneline --graph --all --decorate"

# Undo last commit
git config --global alias.undo "reset --soft HEAD~1"

# Show last commit
git config --global alias.last "log -1 HEAD --stat"

# List aliases
git config --global alias.aliases "config --get-regexp ^alias\."
```

---

## Git Patterns and Formulas

### Safe Force Push
```bash
git push --force-with-lease origin <branch>
```

### Update Feature Branch from Main
```bash
git checkout feature-branch
git fetch origin
git rebase origin/main
```

### Squash All Commits
```bash
git reset --soft <first-commit>
git commit -m "Squashed commit"
```

### Undo Force Push (if you have reflog)
```bash
git reflog
git reset --hard <commit-before-force-push>
git push --force-with-lease
```

### Delete All Local Branches Except Main
```bash
git branch | grep -v "main" | xargs git branch -D
```

### Show Files Changed in Commit
```bash
git show --name-only <commit>
```

---

## Emergency Commands

### I Committed to Wrong Branch
```bash
git reset HEAD~ --soft
git stash
git checkout correct-branch
git stash pop
git add .
git commit -m "Commit message"
```

### I Need to Undo a Public Commit
```bash
git revert <commit-hash>
git push
```

### I Accidentally Deleted a Branch
```bash
git reflog
git checkout -b <branch-name> <commit-hash>
```

### I Need to Change the Last Commit Message
```bash
git commit --amend -m "New message"
# If already pushed (use carefully!)
git push --force-with-lease
```

---

## Quick Reference Card

```
📝 MAKING CHANGES          🌳 BRANCHING
git add .                  git branch <name>
git commit -m "msg"        git checkout -b <name>
git status                 git merge <branch>
git diff                   git branch -d <name>

🔄 SYNCING                 📜 HISTORY
git fetch                  git log
git pull                   git log --oneline
git push                   git show <commit>
git push -u origin main    git reflog

↩️  UNDOING                🏷️  TAGS
git restore <file>         git tag <name>
git reset --soft HEAD~1    git tag -a <name> -m "msg"
git reset --hard HEAD~1    git push --tags
git revert <commit>        git tag -d <name>
```

---

## Resources

- [Official Git Documentation](https://git-scm.com/doc)
- [Pro Git Book](https://git-scm.com/book/en/v2)
- [Git Reference](https://git-scm.com/docs)
- [GitHub Cheat Sheet](https://education.github.com/git-cheat-sheet-education.pdf)

---

[← Back: Exercises](./05-exercises.md) | [Next: Troubleshooting →](./07-troubleshooting.md)
