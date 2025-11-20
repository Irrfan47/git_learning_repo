# Git Collaboration 🤝

Learn how to work with remote repositories and collaborate effectively with other developers using Git.

## Table of Contents

1. [Remote Repositories](#remote-repositories)
2. [Cloning](#cloning)
3. [Fetching and Pulling](#fetching-and-pulling)
4. [Pushing Changes](#pushing-changes)
5. [Pull Requests](#pull-requests)
6. [Forking Workflow](#forking-workflow)
7. [Collaboration Best Practices](#collaboration-best-practices)

---

## Remote Repositories

A remote repository is a version of your project hosted on the internet or network. The most common platforms are GitHub, GitLab, and Bitbucket.

### Viewing Remotes

```bash
# List remote repositories
git remote

# List with URLs
git remote -v

# Show detailed information
git remote show origin
```

### Adding Remotes

```bash
# Add a remote repository
git remote add origin https://github.com/username/repository.git

# Add a different remote
git remote add upstream https://github.com/original/repository.git
```

### Renaming and Removing Remotes

```bash
# Rename a remote
git remote rename origin destination

# Remove a remote
git remote remove origin
```

### Common Remote Names

- **origin**: Your primary remote (usually your GitHub repository)
- **upstream**: The original repository (when you've forked)
- **production**: Production server
- **staging**: Staging server

---

## Cloning

Cloning creates a local copy of a remote repository.

### Basic Clone

```bash
# Clone a repository
git clone https://github.com/username/repository.git

# Clone to a specific directory
git clone https://github.com/username/repository.git my-folder

# Clone a specific branch
git clone -b develop https://github.com/username/repository.git
```

### Clone Methods

**HTTPS (easier, requires password/token):**
```bash
git clone https://github.com/username/repository.git
```

**SSH (more secure, requires SSH key setup):**
```bash
git clone git@github.com:username/repository.git
```

### After Cloning

```bash
# View remote configuration
git remote -v

# List all branches
git branch -a

# Check out a remote branch
git checkout -b local-branch origin/remote-branch
```

---

## Fetching and Pulling

### Git Fetch

Downloads changes from remote without merging:

```bash
# Fetch from origin
git fetch

# Fetch from specific remote
git fetch origin

# Fetch a specific branch
git fetch origin main

# Fetch all remotes
git fetch --all

# Prune deleted remote branches
git fetch --prune
```

**What it does:**
- Updates remote-tracking branches
- Downloads new commits
- Does NOT modify your working files

### Git Pull

Fetches and merges changes in one step:

```bash
# Pull from current branch's remote
git pull

# Pull from specific remote and branch
git pull origin main

# Pull with rebase instead of merge
git pull --rebase

# Pull all submodules
git pull --recurse-submodules
```

**What it does:**
```
git pull = git fetch + git merge
```

### Fetch vs Pull

**Use `git fetch` when:**
- You want to see changes before integrating
- You want to review what others have done
- You're cautious and prefer manual merging

**Use `git pull` when:**
- You want to quickly sync with remote
- You're confident about merging
- You're working on a clean branch

---

## Pushing Changes

### Basic Push

```bash
# Push to remote
git push

# Push to specific remote and branch
git push origin main

# Push and set upstream (first time)
git push -u origin feature-branch

# Push all branches
git push --all

# Push tags
git push --tags
```

### Force Push (Use with Caution!)

```bash
# Force push (overwrites remote)
git push --force
# or safer version
git push --force-with-lease
```

**⚠️ Warning:** Force push can cause data loss for collaborators!

**When to use:**
- After rebasing your feature branch
- To clean up commit history before merging
- **Never** on shared branches like `main`

### Push New Branch

```bash
# Create and push a new branch
git checkout -b feature-new
git push -u origin feature-new

# The -u flag sets upstream tracking
```

### Common Push Errors

**Error: "Updates were rejected"**
```bash
# Solution: Pull first, then push
git pull
git push
```

**Error: "No upstream branch"**
```bash
# Solution: Set upstream
git push -u origin branch-name
```

---

## Pull Requests

Pull requests (PRs) are a way to propose changes and get them reviewed before merging.

### Creating a Pull Request (GitHub)

1. **Push your branch:**
   ```bash
   git push -u origin feature-branch
   ```

2. **On GitHub:**
   - Navigate to the repository
   - Click "Compare & pull request"
   - Add title and description
   - Choose reviewers
   - Click "Create pull request"

### PR Best Practices

**Title:**
- Be clear and concise
- Example: "Add user authentication feature"

**Description:**
- Explain what changes were made
- Explain why changes were needed
- Reference related issues
- Include screenshots if UI changes

**Example PR Description:**
```markdown
## What
Adds user authentication using JWT tokens

## Why
Needed to secure API endpoints and manage user sessions

## How
- Implemented JWT token generation
- Added middleware for authentication
- Updated user model with password hashing

## Related Issues
Fixes #123

## Screenshots
[Attach screenshots if applicable]
```

### Reviewing Pull Requests

```bash
# Fetch PR to local branch
git fetch origin pull/123/head:pr-123
git checkout pr-123

# Test the changes
npm test

# Return to your branch
git checkout main
```

### Updating a Pull Request

```bash
# Make more changes
git add .
git commit -m "Address review comments"
git push
# PR updates automatically!
```

---

## Forking Workflow

Forking is used for contributing to projects you don't have write access to.

### 1. Fork the Repository

On GitHub, click the "Fork" button to create your copy.

### 2. Clone Your Fork

```bash
git clone https://github.com/your-username/repository.git
cd repository
```

### 3. Add Upstream Remote

```bash
git remote add upstream https://github.com/original-owner/repository.git
git remote -v
```

### 4. Keep Your Fork Updated

```bash
# Fetch from upstream
git fetch upstream

# Merge upstream changes
git checkout main
git merge upstream/main

# Push to your fork
git push origin main
```

### 5. Create Feature Branch

```bash
git checkout -b feature-contribution
# Make your changes
git add .
git commit -m "Add new feature"
git push -u origin feature-contribution
```

### 6. Create Pull Request

Create a PR from your fork to the original repository on GitHub.

---

## Collaboration Best Practices

### 1. Communicate

- Write clear commit messages
- Provide context in PRs
- Comment on code reviews constructively
- Keep team informed of progress

### 2. Pull Often

```bash
# Start of day
git checkout main
git pull

# Before starting work
git checkout -b new-feature
git pull origin main
```

### 3. Push Regularly

```bash
# Push at least daily
git push
# Even if work is not complete
```

### 4. Small, Focused Changes

- Keep PRs small and focused
- One feature or fix per PR
- Easier to review and merge

### 5. Review Code Thoroughly

- Read every line
- Test changes locally
- Provide constructive feedback
- Approve when satisfied

### 6. Resolve Conflicts Promptly

```bash
# When conflicts arise
git checkout feature-branch
git pull origin main
# Resolve conflicts
git add .
git commit
git push
```

### 7. Use Branch Protection

On GitHub, protect your `main` branch:
- Require pull request reviews
- Require status checks to pass
- Prevent force pushes
- Prevent deletion

---

## Common Collaboration Workflows

### Workflow 1: Feature Branch

```bash
# 1. Update main
git checkout main
git pull

# 2. Create feature branch
git checkout -b feature-login

# 3. Work and commit
git add .
git commit -m "Implement login form"

# 4. Push
git push -u origin feature-login

# 5. Create PR on GitHub

# 6. After approval and merge
git checkout main
git pull
git branch -d feature-login
```

### Workflow 2: Contributing to Open Source

```bash
# 1. Fork repository on GitHub

# 2. Clone your fork
git clone https://github.com/your-username/project.git
cd project

# 3. Add upstream
git remote add upstream https://github.com/original/project.git

# 4. Create branch
git checkout -b fix-typo

# 5. Make changes
git add .
git commit -m "Fix typo in documentation"

# 6. Push to your fork
git push -u origin fix-typo

# 7. Create PR to upstream
```

### Workflow 3: Hotfix

```bash
# 1. Create hotfix branch from main
git checkout main
git pull
git checkout -b hotfix-security-issue

# 2. Fix the issue
git add .
git commit -m "Fix security vulnerability"

# 3. Push
git push -u origin hotfix-security-issue

# 4. Create PR for immediate review

# 5. After merge
git checkout main
git pull
git branch -d hotfix-security-issue
```

---

## Practice Exercises

### Exercise 1: Remote Operations

1. Create a new repository on GitHub
2. Clone it to your local machine
3. Add a file and commit
4. Push to GitHub
5. Make a change on GitHub
6. Pull the changes locally

<details>
<summary>Solution</summary>

```bash
# After creating repo on GitHub
git clone https://github.com/your-username/test-repo.git
cd test-repo

echo "Hello World" > hello.txt
git add hello.txt
git commit -m "Add hello.txt"
git push

# Make change on GitHub (edit file in web interface)

git pull
```
</details>

### Exercise 2: Fork and Contribute

1. Fork a public repository
2. Clone your fork
3. Add upstream remote
4. Create a feature branch
5. Make a change and commit
6. Push to your fork
7. Create a pull request

<details>
<summary>Solution</summary>

```bash
# After forking on GitHub
git clone https://github.com/your-username/forked-repo.git
cd forked-repo

git remote add upstream https://github.com/original/repo.git

git checkout -b fix-readme

echo "Additional info" >> README.md
git add README.md
git commit -m "Update README with additional info"

git push -u origin fix-readme

# Create PR on GitHub from fix-readme to upstream/main
```
</details>

---

## Quick Reference

```bash
# Clone
git clone <url>

# Remotes
git remote -v
git remote add origin <url>

# Fetch
git fetch
git fetch origin

# Pull
git pull
git pull origin main

# Push
git push
git push -u origin branch

# Upstream
git push -u origin feature
git branch --set-upstream-to=origin/main
```

---

## Troubleshooting

### Can't Push: "Repository not found"

Check your remote URL:
```bash
git remote -v
git remote set-url origin https://github.com/username/repo.git
```

### Merge Conflicts After Pull

```bash
# Resolve conflicts in files
git add .
git commit
```

### Accidentally Committed to Wrong Branch

```bash
# Move commit to correct branch
git branch feature-branch
git reset HEAD~1
git checkout feature-branch
```

---

[← Back: Git Branching](./02-git-branching.md) | [Next: Git Advanced →](./04-git-advanced.md)
