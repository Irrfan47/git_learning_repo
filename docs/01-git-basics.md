# Git Basics 📖

This guide covers the fundamental Git commands you need to get started. Master these basics before moving on to more advanced topics.

## Table of Contents

1. [What is a Repository?](#what-is-a-repository)
2. [Creating a Repository](#creating-a-repository)
3. [The Three States](#the-three-states)
4. [Basic Workflow](#basic-workflow)
5. [Viewing History](#viewing-history)
6. [Essential Commands Reference](#essential-commands-reference)

---

## What is a Repository?

A Git repository (or "repo") is a directory that contains your project files and the entire history of changes made to those files. It includes a hidden `.git` folder that stores all the version control information.

---

## Creating a Repository

### Option 1: Initialize a New Repository

Create a new Git repository in an existing directory:

```bash
# Navigate to your project directory
cd /path/to/your/project

# Initialize Git
git init
```

**What happens:** Git creates a `.git` folder to track your project.

### Option 2: Clone an Existing Repository

Copy a repository from a remote location (like GitHub):

```bash
# Clone a repository
git clone https://github.com/username/repository.git

# Clone to a specific folder
git clone https://github.com/username/repository.git my-folder
```

**What happens:** Git downloads the entire repository including all history.

---

## The Three States

Git has three main states for your files:

```
Working Directory → Staging Area → Repository
     (Modified)      (Staged)      (Committed)
```

1. **Working Directory**: Where you modify files
2. **Staging Area (Index)**: Where you prepare files for commit
3. **Repository**: Where Git permanently stores snapshots

---

## Basic Workflow

### 1. Check Status

Always check what's going on:

```bash
git status
```

**Output shows:**
- Modified files (red)
- Staged files (green)
- Untracked files
- Current branch

**Best Practice:** Run `git status` frequently!

### 2. Stage Changes

Add files to the staging area:

```bash
# Stage a specific file
git add filename.txt

# Stage multiple files
git add file1.txt file2.txt

# Stage all changes in current directory
git add .

# Stage all changes in the repository
git add -A

# Stage all modified files (not new files)
git add -u
```

**What it means:** You're telling Git "I want to include these changes in my next commit."

### 3. Commit Changes

Save your staged changes:

```bash
# Commit with a message
git commit -m "Add user authentication feature"

# Commit with a detailed message (opens editor)
git commit

# Stage and commit in one step (only for tracked files)
git commit -am "Fix login bug"
```

**Writing Good Commit Messages:**
- Use present tense: "Add feature" not "Added feature"
- Be specific and concise
- First line should be under 50 characters
- Add details in subsequent lines if needed

**Examples:**
```
✅ Good: "Fix null pointer exception in user login"
❌ Bad: "Fixed stuff"

✅ Good: "Add password reset functionality"
❌ Bad: "changes"
```

### 4. View Changes

See what you've modified:

```bash
# Show unstaged changes
git diff

# Show staged changes
git diff --staged
# or
git diff --cached

# Show changes in a specific file
git diff filename.txt

# Compare two commits
git diff commit1 commit2
```

---

## Viewing History

### Basic Log

```bash
# Show commit history
git log

# Show concise history (one line per commit)
git log --oneline

# Show history with graph
git log --oneline --graph --all

# Show last N commits
git log -n 5

# Show commits by author
git log --author="John Doe"

# Show commits with diffs
git log -p
```

### Show Specific Commit

```bash
# Show details of a specific commit
git show <commit-hash>

# Show details of the last commit
git show HEAD

# Show details of a specific file in a commit
git show <commit-hash>:path/to/file
```

### History of a File

```bash
# Show commits that modified a file
git log filename.txt

# Show changes to a file
git log -p filename.txt
```

---

## Essential Commands Reference

### Configuration

```bash
# Set your name
git config --global user.name "Your Name"

# Set your email
git config --global user.email "your.email@example.com"

# View all settings
git config --list

# View a specific setting
git config user.name
```

### Getting Help

```bash
# Get help for a command
git help <command>
git <command> --help

# Quick help
git <command> -h
```

### Undoing Changes

```bash
# Discard changes in working directory
git checkout -- filename.txt
# or (newer syntax)
git restore filename.txt

# Unstage a file (keep changes)
git reset HEAD filename.txt
# or (newer syntax)
git restore --staged filename.txt

# Discard all local changes
git reset --hard HEAD
```

**⚠️ Warning:** `git reset --hard` permanently deletes uncommitted changes!

### Removing Files

```bash
# Remove a file (stages the deletion)
git rm filename.txt

# Remove a file from Git but keep it locally
git rm --cached filename.txt

# Remove a directory
git rm -r directory/
```

### Moving/Renaming Files

```bash
# Rename a file
git mv old-name.txt new-name.txt

# This is equivalent to:
# mv old-name.txt new-name.txt
# git rm old-name.txt
# git add new-name.txt
```

### Ignoring Files

Create a `.gitignore` file to exclude files from version control:

```bash
# Create .gitignore
touch .gitignore
```

**Common `.gitignore` patterns:**
```
# Dependencies
node_modules/
vendor/

# Build outputs
dist/
build/
*.exe

# Logs
*.log
logs/

# OS files
.DS_Store
Thumbs.db

# IDE files
.vscode/
.idea/
*.swp

# Environment variables
.env
.env.local
```

---

## Practice Exercises

### Exercise 1: First Repository

1. Create a new directory called `git-practice`
2. Initialize it as a Git repository
3. Create a file called `hello.txt` with some text
4. Stage and commit the file
5. Modify the file and commit again
6. View the commit history

<details>
<summary>Solution</summary>

```bash
mkdir git-practice
cd git-practice
git init
echo "Hello, Git!" > hello.txt
git add hello.txt
git commit -m "Add hello.txt"
echo "Learning Git is fun!" >> hello.txt
git add hello.txt
git commit -m "Update hello.txt with additional text"
git log --oneline
```
</details>

### Exercise 2: Working with Status and Diff

1. In your repository, create two files: `file1.txt` and `file2.txt`
2. Stage only `file1.txt`
3. Check the status
4. View the diff for staged and unstaged changes
5. Stage and commit both files

<details>
<summary>Solution</summary>

```bash
echo "File 1 content" > file1.txt
echo "File 2 content" > file2.txt
git add file1.txt
git status
git diff
git diff --staged
git add file2.txt
git commit -m "Add file1 and file2"
```
</details>

---

## Common Mistakes to Avoid

1. **Not checking status before committing**: Always run `git status` first
2. **Writing vague commit messages**: Be specific about what changed
3. **Committing too much**: Make small, logical commits
4. **Committing too little**: Don't commit every single line change
5. **Not reviewing changes**: Use `git diff` before committing

---

## Next Steps

Once you're comfortable with these basics, move on to:
- [Git Branching](./02-git-branching.md) - Learn to work with branches
- [Git Collaboration](./03-git-collaboration.md) - Work with remote repositories

---

## Quick Cheat Sheet

```bash
# Initialize
git init

# Clone
git clone <url>

# Status
git status

# Stage
git add <file>
git add .

# Commit
git commit -m "message"

# History
git log --oneline

# Diff
git diff

# Help
git help <command>
```

---

[← Back to Main](../README.md) | [Next: Git Branching →](./02-git-branching.md)
