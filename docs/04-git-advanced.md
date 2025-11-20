# Git Advanced Topics ⚡

Master advanced Git techniques to level up your version control skills.

## Table of Contents

1. [Interactive Rebase](#interactive-rebase)
2. [Cherry-Picking](#cherry-picking)
3. [Stashing](#stashing)
4. [Git Reset and Revert](#git-reset-and-revert)
5. [Git Reflog](#git-reflog)
6. [Git Tags](#git-tags)
7. [Git Hooks](#git-hooks)
8. [Git Bisect](#git-bisect)
9. [Submodules](#submodules)
10. [Advanced Tips](#advanced-tips)

---

## Interactive Rebase

Interactive rebase allows you to edit, reorder, squash, or drop commits.

### Basic Interactive Rebase

```bash
# Rebase last 3 commits
git rebase -i HEAD~3

# Rebase since a specific commit
git rebase -i <commit-hash>

# Rebase onto a branch
git rebase -i main
```

### Rebase Commands

When the editor opens, you'll see:

```
pick abc1234 Add feature X
pick def5678 Fix typo
pick ghi9012 Update documentation

# Commands:
# p, pick = use commit
# r, reword = use commit, but edit message
# e, edit = use commit, but stop for amending
# s, squash = use commit, but meld into previous
# f, fixup = like squash, but discard message
# d, drop = remove commit
```

### Common Rebase Scenarios

**Squash commits:**
```
pick abc1234 Add feature X
squash def5678 Fix typo in feature X
squash ghi9012 Another fix for feature X
```
Result: All three commits become one.

**Reword commit message:**
```
pick abc1234 Add feature X
reword def5678 Fix typo
pick ghi9012 Update documentation
```

**Reorder commits:**
```
pick ghi9012 Update documentation
pick abc1234 Add feature X
pick def5678 Fix typo
```

### Aborting a Rebase

```bash
git rebase --abort
```

### Continuing After Conflict

```bash
# After resolving conflicts
git add .
git rebase --continue

# Skip a commit
git rebase --skip
```

**⚠️ Warning:** Never rebase commits that have been pushed to a shared branch!

---

## Cherry-Picking

Apply specific commits from one branch to another.

### Basic Cherry-Pick

```bash
# Apply a single commit
git cherry-pick <commit-hash>

# Apply multiple commits
git cherry-pick <commit-1> <commit-2>

# Apply a range of commits
git cherry-pick <start-commit>..<end-commit>
```

### Cherry-Pick with Options

```bash
# Cherry-pick without committing
git cherry-pick -n <commit-hash>

# Edit commit message
git cherry-pick -e <commit-hash>

# Add sign-off
git cherry-pick -s <commit-hash>
```

### Handling Conflicts

```bash
# After resolving conflicts
git add .
git cherry-pick --continue

# Abort cherry-pick
git cherry-pick --abort
```

### Use Cases

- Apply a hotfix to multiple branches
- Port a feature to a different branch
- Recover specific commits

---

## Stashing

Save uncommitted changes temporarily without committing.

### Basic Stash Operations

```bash
# Stash changes
git stash

# Stash with a message
git stash save "Work in progress on feature X"

# Stash including untracked files
git stash -u

# Stash including ignored files
git stash -a
```

### Viewing Stashes

```bash
# List stashes
git stash list

# Show stash contents
git stash show
git stash show -p  # With diff

# Show specific stash
git stash show stash@{1}
```

### Applying Stashes

```bash
# Apply most recent stash
git stash apply

# Apply and remove stash
git stash pop

# Apply specific stash
git stash apply stash@{2}

# Apply to a new branch
git stash branch new-branch-name
```

### Managing Stashes

```bash
# Remove a stash
git stash drop stash@{1}

# Remove all stashes
git stash clear

# Create a branch from stash
git stash branch feature-branch
```

### Use Cases

- Switch branches with uncommitted work
- Pull changes with local modifications
- Test different approaches
- Quickly save work before trying something risky

---

## Git Reset and Revert

### Git Reset

Move the current branch pointer and optionally modify staging area and working directory.

**Three modes:**

```bash
# Soft reset - Keep changes staged
git reset --soft HEAD~1

# Mixed reset (default) - Keep changes unstaged
git reset HEAD~1
git reset --mixed HEAD~1

# Hard reset - Discard changes completely
git reset --hard HEAD~1
```

**Visual representation:**
```
--soft:  HEAD moves, staging & working directory unchanged
--mixed: HEAD moves, staging cleared, working directory unchanged
--hard:  HEAD moves, staging & working directory cleared
```

**Use cases:**
```bash
# Undo last commit, keep changes
git reset --soft HEAD~1

# Undo multiple commits
git reset --hard HEAD~3

# Reset to specific commit
git reset --hard <commit-hash>

# Unstage files
git reset HEAD file.txt
```

### Git Revert

Create a new commit that undoes changes from a previous commit.

```bash
# Revert last commit
git revert HEAD

# Revert specific commit
git revert <commit-hash>

# Revert without committing
git revert -n <commit-hash>

# Revert a merge commit
git revert -m 1 <merge-commit-hash>
```

### Reset vs Revert

**Use Reset when:**
- Changes haven't been pushed
- You want to rewrite history
- Working on a private branch

**Use Revert when:**
- Changes have been pushed
- Working on a shared branch
- You want to preserve history

---

## Git Reflog

Reflog tracks all changes to HEAD, allowing you to recover "lost" commits.

### Viewing Reflog

```bash
# Show reflog
git reflog

# Show reflog for specific branch
git reflog show branch-name

# Show detailed reflog
git log -g

# Show reflog for last 30 days
git reflog --since='30 days ago'
```

### Recovering Lost Commits

```bash
# View reflog
git reflog

# Find the commit you want
# (e.g., abc1234)

# Create branch at that commit
git branch recovery abc1234

# Or reset to that commit
git reset --hard abc1234
```

### Common Recovery Scenarios

**Recover from hard reset:**
```bash
git reflog
git reset --hard HEAD@{1}
```

**Recover deleted branch:**
```bash
git reflog
git checkout -b recovered-branch <commit-hash>
```

**Undo a rebase:**
```bash
git reflog
git reset --hard HEAD@{5}  # Before rebase
```

---

## Git Tags

Tags mark specific points in history, typically for releases.

### Creating Tags

```bash
# Lightweight tag
git tag v1.0.0

# Annotated tag (recommended)
git tag -a v1.0.0 -m "Release version 1.0.0"

# Tag specific commit
git tag -a v1.0.0 <commit-hash> -m "Version 1.0.0"
```

### Viewing Tags

```bash
# List tags
git tag

# List tags with pattern
git tag -l "v1.*"

# Show tag details
git show v1.0.0
```

### Pushing Tags

```bash
# Push specific tag
git push origin v1.0.0

# Push all tags
git push origin --tags

# Push only annotated tags
git push origin --follow-tags
```

### Deleting Tags

```bash
# Delete local tag
git tag -d v1.0.0

# Delete remote tag
git push origin --delete v1.0.0
# or
git push origin :refs/tags/v1.0.0
```

### Checking Out Tags

```bash
# View code at tag
git checkout v1.0.0

# Create branch from tag
git checkout -b version-1 v1.0.0
```

---

## Git Hooks

Hooks are scripts that run automatically at specific points in the Git workflow.

### Types of Hooks

**Client-side:**
- `pre-commit`: Before commit is created
- `prepare-commit-msg`: Before commit message editor
- `commit-msg`: Validate commit message
- `post-commit`: After commit is created
- `pre-push`: Before push
- `pre-rebase`: Before rebase

**Server-side:**
- `pre-receive`: Before push is accepted
- `update`: Like pre-receive, per branch
- `post-receive`: After push is accepted

### Creating a Hook

```bash
# Navigate to hooks directory
cd .git/hooks

# Create pre-commit hook
cat > pre-commit << 'EOF'
#!/bin/bash
echo "Running tests before commit..."
npm test
if [ $? -ne 0 ]; then
    echo "Tests failed! Commit aborted."
    exit 1
fi
EOF

# Make executable
chmod +x pre-commit
```

### Example Hooks

**Pre-commit: Run linter**
```bash
#!/bin/bash
npm run lint
```

**Commit-msg: Validate message format**
```bash
#!/bin/bash
commit_msg=$(cat "$1")
if ! echo "$commit_msg" | grep -qE "^(feat|fix|docs|style|refactor|test|chore):"; then
    echo "ERROR: Commit message must start with type (feat|fix|docs|...):"
    exit 1
fi
```

**Pre-push: Run tests**
```bash
#!/bin/bash
npm test
exit $?
```

### Hook Tools

Consider using tools like:
- **Husky**: Easy Git hooks for Node.js projects
- **pre-commit**: Framework for managing Git hooks

---

## Git Bisect

Find the commit that introduced a bug using binary search.

### Basic Bisect

```bash
# Start bisect
git bisect start

# Mark current commit as bad
git bisect bad

# Mark a known good commit
git bisect good <commit-hash>

# Git checks out middle commit
# Test it and mark as good or bad
git bisect good  # or git bisect bad

# Continue until bug is found
# Git will identify the problematic commit

# End bisect
git bisect reset
```

### Automated Bisect

```bash
# Start bisect
git bisect start HEAD v1.0.0

# Run a test script automatically
git bisect run npm test

# Git automatically finds the bad commit
```

### Example Workflow

```bash
# Bug exists now, but worked at tag v1.0
git bisect start
git bisect bad HEAD
git bisect good v1.0

# Test each commit Git checks out
# If bug exists: git bisect bad
# If bug doesn't exist: git bisect good

# After finding problematic commit
git bisect reset
```

---

## Submodules

Include external repositories within your repository.

### Adding Submodules

```bash
# Add a submodule
git submodule add https://github.com/user/repo.git path/to/submodule

# Commit the submodule
git commit -m "Add submodule"
```

### Cloning with Submodules

```bash
# Clone and initialize submodules
git clone --recurse-submodules https://github.com/user/repo.git

# Or after cloning
git submodule init
git submodule update
```

### Updating Submodules

```bash
# Update all submodules
git submodule update --remote

# Update specific submodule
git submodule update --remote path/to/submodule

# Pull changes in submodule
cd path/to/submodule
git pull
cd ../..
git add path/to/submodule
git commit -m "Update submodule"
```

### Removing Submodules

```bash
# Remove submodule
git submodule deinit path/to/submodule
git rm path/to/submodule
git commit -m "Remove submodule"
```

---

## Advanced Tips

### Aliases

Create shortcuts for common commands:

```bash
# Simple aliases
git config --global alias.st status
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.ci commit

# Complex aliases
git config --global alias.lg "log --oneline --graph --all --decorate"
git config --global alias.unstage "reset HEAD --"
git config --global alias.last "log -1 HEAD"
```

### Searching History

```bash
# Search commit messages
git log --grep="bug fix"

# Search for code changes
git log -S "function_name"

# Search by author
git log --author="John Doe"

# Show commits affecting a file
git log --follow filename.txt
```

### Comparing Branches

```bash
# Show commits in branch-a not in branch-b
git log branch-a..branch-b

# Show commits in either branch but not both
git log branch-a...branch-b

# Show files that differ
git diff --name-only main feature
```

### Cleaning Repository

```bash
# Remove untracked files (dry run)
git clean -n

# Remove untracked files
git clean -f

# Remove untracked files and directories
git clean -fd

# Remove ignored files too
git clean -fdx
```

### Archive Repository

```bash
# Create zip archive
git archive --format=zip --output=project.zip HEAD

# Create tar archive
git archive --format=tar HEAD | gzip > project.tar.gz

# Archive specific directory
git archive --format=zip --output=docs.zip HEAD:docs/
```

---

## Practice Exercises

### Exercise 1: Interactive Rebase

1. Create 3 commits with messages "First", "Second", "Third"
2. Use interactive rebase to squash them into one
3. Change the final commit message

<details>
<summary>Solution</summary>

```bash
echo "1" > file.txt && git add . && git commit -m "First"
echo "2" >> file.txt && git add . && git commit -m "Second"
echo "3" >> file.txt && git add . && git commit -m "Third"

git rebase -i HEAD~3
# Change to:
# pick abc1234 First
# squash def5678 Second
# squash ghi9012 Third
# Save and edit commit message
```
</details>

### Exercise 2: Stashing

1. Make changes to a file (don't commit)
2. Stash the changes
3. Switch to another branch
4. Return and apply the stash

<details>
<summary>Solution</summary>

```bash
echo "Changes" >> file.txt
git stash save "Work in progress"
git checkout other-branch
git checkout main
git stash pop
```
</details>

---

## Quick Reference

```bash
# Rebase
git rebase -i HEAD~3

# Cherry-pick
git cherry-pick <commit>

# Stash
git stash
git stash pop

# Reset
git reset --soft HEAD~1
git reset --hard HEAD~1

# Revert
git revert <commit>

# Tags
git tag -a v1.0.0 -m "Version 1.0"
git push --tags

# Reflog
git reflog
git reset --hard HEAD@{1}
```

---

[← Back: Git Collaboration](./03-git-collaboration.md) | [Next: Exercises →](./05-exercises.md)
