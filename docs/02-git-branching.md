# Git Branching 🌳

Branching is one of Git's most powerful features. It allows you to diverge from the main line of development and work on features, fixes, or experiments independently.

## Table of Contents

1. [What is a Branch?](#what-is-a-branch)
2. [Why Use Branches?](#why-use-branches)
3. [Branch Operations](#branch-operations)
4. [Merging Branches](#merging-branches)
5. [Merge Conflicts](#merge-conflicts)
6. [Branching Strategies](#branching-strategies)
7. [Best Practices](#best-practices)

---

## What is a Branch?

A branch is simply a pointer to a specific commit. The default branch is usually called `main` (or `master` in older repositories).

```
main:    A---B---C
              \
feature:       D---E
```

Think of branches as parallel universes where you can make changes without affecting the main timeline.

---

## Why Use Branches?

- **Feature Development**: Work on new features without breaking the main code
- **Bug Fixes**: Fix bugs in isolation
- **Experimentation**: Try new ideas safely
- **Collaboration**: Multiple developers can work simultaneously
- **Release Management**: Maintain multiple versions

---

## Branch Operations

### Viewing Branches

```bash
# List all local branches
git branch

# List all branches (local and remote)
git branch -a

# List remote branches only
git branch -r

# Show more details
git branch -v

# Show merged branches
git branch --merged

# Show unmerged branches
git branch --no-merged
```

### Creating Branches

```bash
# Create a new branch
git branch feature-login

# Create and switch to a new branch
git checkout -b feature-login
# or (newer syntax)
git switch -c feature-login

# Create a branch from a specific commit
git branch feature-name <commit-hash>
```

### Switching Branches

```bash
# Switch to an existing branch
git checkout feature-login
# or (newer syntax)
git switch feature-login

# Switch to previous branch
git checkout -
# or
git switch -
```

**Note:** Before switching branches, make sure to:
- Commit your changes, or
- Stash your changes (covered in Advanced topics)

### Renaming Branches

```bash
# Rename current branch
git branch -m new-name

# Rename a different branch
git branch -m old-name new-name
```

### Deleting Branches

```bash
# Delete a merged branch
git branch -d feature-login

# Force delete (even if not merged)
git branch -D feature-login

# Delete a remote branch
git push origin --delete feature-login
```

---

## Merging Branches

### Fast-Forward Merge

When the target branch hasn't changed since the feature branch was created:

```bash
# Switch to target branch
git checkout main

# Merge feature branch
git merge feature-login
```

**Example:**
```
Before:
main:    A---B---C
              \
feature:       D---E

After fast-forward:
main:    A---B---C---D---E
```

### Three-Way Merge

When both branches have new commits:

```bash
git checkout main
git merge feature-login
```

**Example:**
```
Before:
main:    A---B---C---F
              \
feature:       D---E

After merge:
main:    A---B---C---F---M
              \         /
feature:       D---E----
```

### Merge with No Fast-Forward

Force a merge commit even if fast-forward is possible:

```bash
git merge --no-ff feature-login
```

**Why?** Preserves the history that a feature branch existed.

### Aborting a Merge

If things go wrong:

```bash
git merge --abort
```

---

## Merge Conflicts

### What Causes Conflicts?

Conflicts occur when:
- The same line in a file was changed in both branches
- A file was modified in one branch and deleted in another

### Identifying Conflicts

```bash
# After attempting a merge
git status
# Shows "both modified" for conflicted files
```

### Resolving Conflicts

1. **Open the conflicted file**. You'll see conflict markers:

```
<<<<<<< HEAD
This is the content from the current branch
=======
This is the content from the branch being merged
>>>>>>> feature-branch
```

2. **Edit the file** to resolve the conflict:
   - Keep one version, or
   - Keep both versions, or
   - Write something entirely new

3. **Remove the conflict markers** (`<<<<<<<`, `=======`, `>>>>>>>`)

4. **Stage the resolved file**:
```bash
git add resolved-file.txt
```

5. **Complete the merge**:
```bash
git commit
# Git will create a default merge commit message
```

### Tools for Resolving Conflicts

```bash
# Use a merge tool
git mergetool

# See the conflict from both perspectives
git diff

# See which files have conflicts
git status
```

---

## Branching Strategies

### 1. Feature Branch Workflow

- Create a branch for each feature
- Merge back to main when complete
- Delete the feature branch

```bash
git checkout -b feature-user-profile
# ... work on feature ...
git checkout main
git merge feature-user-profile
git branch -d feature-user-profile
```

### 2. Git Flow

A more structured approach:

- **main**: Production-ready code
- **develop**: Integration branch
- **feature/***: New features
- **release/***: Release preparation
- **hotfix/***: Emergency fixes

```bash
# Start a feature
git checkout -b feature/user-auth develop

# Finish a feature
git checkout develop
git merge feature/user-auth
git branch -d feature/user-auth
```

### 3. GitHub Flow

Simpler than Git Flow:

- **main**: Always deployable
- **feature branches**: Short-lived, merged via pull requests

```bash
# Create feature branch
git checkout -b add-login-form

# Push to remote
git push -u origin add-login-form
# Create pull request on GitHub
# After review, merge and delete branch
```

---

## Best Practices

### Branch Naming

Use clear, descriptive names:

```bash
# Good
feature/user-authentication
bugfix/login-error
hotfix/security-patch
experiment/new-ui

# Bad
branch1
test
my-branch
```

### Commit Before Switching

Always commit or stash before switching branches:

```bash
# Check for uncommitted changes
git status

# If you have changes
git commit -m "Save work in progress"
# or
git stash
```

### Keep Branches Short-Lived

- Merge or delete branches soon after completing work
- Long-lived branches are harder to merge
- Aim for hours or days, not weeks or months

### Update Feature Branches Regularly

Keep your branch up-to-date with the main branch:

```bash
git checkout feature-branch
git merge main
# or
git rebase main
```

### Delete Merged Branches

Clean up after merging:

```bash
# Delete local branch
git branch -d feature-branch

# Delete remote branch
git push origin --delete feature-branch
```

---

## Common Scenarios

### Scenario 1: Start a New Feature

```bash
# Make sure main is up to date
git checkout main
git pull

# Create and switch to feature branch
git checkout -b feature-user-profile

# Make changes
echo "User profile code" > profile.txt
git add profile.txt
git commit -m "Add user profile feature"

# Push to remote
git push -u origin feature-user-profile
```

### Scenario 2: Merge a Feature

```bash
# Update main
git checkout main
git pull

# Merge feature
git merge feature-user-profile

# Push
git push

# Delete branch
git branch -d feature-user-profile
git push origin --delete feature-user-profile
```

### Scenario 3: Fix a Bug

```bash
# Create bugfix branch from main
git checkout main
git checkout -b bugfix-login-error

# Fix the bug
# ... make changes ...
git add .
git commit -m "Fix login error for special characters"

# Merge back
git checkout main
git merge bugfix-login-error
git push

# Clean up
git branch -d bugfix-login-error
```

---

## Practice Exercises

### Exercise 1: Basic Branching

1. Create a new branch called `experiment`
2. Add a new file and commit it
3. Switch back to `main`
4. Verify the file doesn't exist in `main`
5. Merge `experiment` into `main`
6. Delete the `experiment` branch

<details>
<summary>Solution</summary>

```bash
git checkout -b experiment
echo "Experimental feature" > experiment.txt
git add experiment.txt
git commit -m "Add experimental feature"
git checkout main
ls experiment.txt  # Should not exist
git merge experiment
ls experiment.txt  # Now exists
git branch -d experiment
```
</details>

### Exercise 2: Merge Conflict Resolution

1. Create a branch called `branch-a`
2. Modify line 1 of a file and commit
3. Switch to `main`
4. Modify the same line differently and commit
5. Try to merge `branch-a` into `main`
6. Resolve the conflict

<details>
<summary>Solution</summary>

```bash
echo "Original content" > conflict.txt
git add conflict.txt
git commit -m "Add conflict.txt"

git checkout -b branch-a
echo "Change from branch-a" > conflict.txt
git add conflict.txt
git commit -m "Update from branch-a"

git checkout main
echo "Change from main" > conflict.txt
git add conflict.txt
git commit -m "Update from main"

git merge branch-a
# Conflict occurs!

# Edit conflict.txt to resolve
# Remove markers and keep desired content
git add conflict.txt
git commit -m "Resolve merge conflict"
```
</details>

---

## Quick Reference

```bash
# Create branch
git branch <name>
git checkout -b <name>

# Switch branch
git checkout <name>
git switch <name>

# List branches
git branch
git branch -a

# Merge
git merge <branch>

# Delete branch
git branch -d <name>

# Rename branch
git branch -m <new-name>
```

---

[← Back: Git Basics](./01-git-basics.md) | [Next: Git Collaboration →](./03-git-collaboration.md)
