# Troubleshooting Guide 🔧

Solutions to common Git problems and error messages.

## Table of Contents

1. [Common Error Messages](#common-error-messages)
2. [Merge Conflicts](#merge-conflicts)
3. [Push and Pull Issues](#push-and-pull-issues)
4. [Branch Problems](#branch-problems)
5. [Commit Issues](#commit-issues)
6. [Repository Issues](#repository-issues)
7. [Performance Problems](#performance-problems)
8. [Recovery Scenarios](#recovery-scenarios)

---

## Common Error Messages

### "fatal: not a git repository"

**Problem:** You're not in a Git repository.

**Solution:**
```bash
# Check if you're in the right directory
pwd

# Initialize a repository if needed
git init

# Or navigate to the repository
cd /path/to/repository
```

---

### "error: Your local changes would be overwritten"

**Problem:** You have uncommitted changes that conflict with incoming changes.

**Solutions:**

**Option 1: Commit your changes**
```bash
git add .
git commit -m "Save local changes"
git pull
```

**Option 2: Stash your changes**
```bash
git stash
git pull
git stash pop
```

**Option 3: Discard your changes (careful!)**
```bash
git reset --hard
git pull
```

---

### "error: failed to push some refs"

**Problem:** Remote has changes you don't have locally.

**Solution:**
```bash
# Pull first, then push
git pull
git push

# Or if you prefer rebase
git pull --rebase
git push
```

---

### "fatal: refusing to merge unrelated histories"

**Problem:** Trying to merge two repositories with no common ancestor.

**Solution:**
```bash
git pull origin main --allow-unrelated-histories
```

---

### "error: pathspec did not match any file(s)"

**Problem:** File or branch doesn't exist.

**Solutions:**
```bash
# Check if file exists
ls <filename>

# Check branch names
git branch -a

# Make sure you're typing the name correctly
git checkout <correct-branch-name>
```

---

## Merge Conflicts

### Understanding Conflict Markers

When you see a conflict, the file will contain:

```
<<<<<<< HEAD
Your current changes
=======
Incoming changes
>>>>>>> branch-name
```

### Resolving Conflicts

**Step 1: Identify conflicted files**
```bash
git status
# Look for "both modified"
```

**Step 2: Open and edit files**
- Keep your changes, or
- Keep incoming changes, or
- Combine both, or
- Write something new

**Step 3: Remove markers**
Delete `<<<<<<<`, `=======`, and `>>>>>>>` lines

**Step 4: Stage resolved files**
```bash
git add <file>
```

**Step 5: Complete merge**
```bash
git commit
# Or if rebasing
git rebase --continue
```

### Aborting a Merge

```bash
# Abort merge
git merge --abort

# Abort rebase
git rebase --abort

# Abort cherry-pick
git cherry-pick --abort
```

### Tools for Resolving Conflicts

```bash
# Use merge tool
git mergetool

# See conflict from both sides
git diff

# See your version
git show :2:<file>

# See their version
git show :3:<file>

# Accept your version
git checkout --ours <file>

# Accept their version
git checkout --theirs <file>
```

---

## Push and Pull Issues

### "Authentication failed"

**Problem:** Can't authenticate with remote.

**Solutions:**

**For HTTPS:**
```bash
# Use personal access token instead of password
# Generate token at GitHub settings > Developer settings > Personal access tokens

# Update credentials
git config --global credential.helper cache
```

**For SSH:**
```bash
# Generate SSH key
ssh-keygen -t ed25519 -C "your.email@example.com"

# Add key to ssh-agent
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519

# Add public key to GitHub
cat ~/.ssh/id_ed25519.pub
# Copy and paste to GitHub Settings > SSH Keys
```

---

### "Permission denied (publickey)"

**Problem:** SSH authentication failing.

**Solution:**
```bash
# Test SSH connection
ssh -T git@github.com

# Check if SSH key is added
ssh-add -l

# Add SSH key
ssh-add ~/.ssh/id_ed25519

# Check GitHub SSH key setup
# Visit: https://github.com/settings/keys
```

---

### "Updates were rejected because the tip of your current branch is behind"

**Problem:** Remote is ahead of local.

**Solution:**
```bash
# Option 1: Pull and merge
git pull origin main
git push

# Option 2: Pull with rebase
git pull --rebase origin main
git push

# Option 3: Force push (dangerous!)
git push --force-with-lease
```

---

### "Cannot push to remote repository"

**Problem:** Various push issues.

**Diagnosis:**
```bash
# Check remote URL
git remote -v

# Check branch tracking
git branch -vv

# Check if remote exists
git remote show origin
```

**Solutions:**
```bash
# Set upstream if not set
git push -u origin <branch-name>

# Fix remote URL if wrong
git remote set-url origin <correct-url>
```

---

## Branch Problems

### "error: branch already exists"

**Problem:** Trying to create a branch that exists.

**Solutions:**
```bash
# Use a different name
git branch new-unique-name

# Or checkout existing branch
git checkout existing-branch

# Or delete old branch first
git branch -d existing-branch
git branch new-name
```

---

### "Cannot delete branch 'branch-name'"

**Problem:** Branch not fully merged.

**Solutions:**
```bash
# Check what's not merged
git log main..branch-name

# Force delete (if you're sure)
git branch -D branch-name

# Merge first, then delete
git checkout main
git merge branch-name
git branch -d branch-name
```

---

### "You are in 'detached HEAD' state"

**Problem:** Checked out a commit directly instead of a branch.

**Understanding:**
- Your HEAD points to a commit, not a branch
- Commits made here may be lost

**Solutions:**

**If you want to keep work:**
```bash
# Create a branch at current position
git checkout -b new-branch-name
```

**If you want to go back:**
```bash
# Return to a branch
git checkout main
```

---

## Commit Issues

### "Undo last commit (not pushed)"

**Solutions:**

**Keep changes staged:**
```bash
git reset --soft HEAD~1
```

**Keep changes unstaged:**
```bash
git reset HEAD~1
```

**Discard changes:**
```bash
git reset --hard HEAD~1
```

---

### "Undo last commit (already pushed)"

**Solution:**
```bash
# Create a revert commit
git revert HEAD
git push

# Don't use reset --hard if others have pulled!
```

---

### "Change last commit message"

**Before pushing:**
```bash
git commit --amend -m "New message"
```

**After pushing:**
```bash
git commit --amend -m "New message"
git push --force-with-lease
# Warning: Only do this on branches you own!
```

---

### "Add file to last commit"

**Solution:**
```bash
git add forgotten-file.txt
git commit --amend --no-edit
```

---

### "Committed to wrong branch"

**Solution:**
```bash
# Save commit
git branch new-correct-branch

# Remove from current branch
git reset HEAD~1 --hard

# Switch to correct branch
git checkout new-correct-branch
```

---

## Repository Issues

### "Repository is too slow"

**Solutions:**

**Clean up:**
```bash
# Garbage collection
git gc

# Aggressive optimization
git gc --aggressive

# Prune old objects
git prune
```

**Reduce history:**
```bash
# Clone with limited depth
git clone --depth 1 <url>

# Fetch with limited depth
git fetch --depth 1
```

---

### "'.git' folder is huge"

**Solutions:**

**Find large files:**
```bash
# Find large files in history
git rev-list --objects --all \
  | git cat-file --batch-check='%(objecttype) %(objectname) %(objectsize) %(rest)' \
  | awk '/^blob/ {print substr($0,6)}' \
  | sort --numeric-sort --key=2 \
  | tail -n 10
```

**Remove large files:**
```bash
# Using BFG Repo-Cleaner (recommended)
# Download from: https://rtyley.github.io/bfg-repo-cleaner/
java -jar bfg.jar --strip-blobs-bigger-than 50M repo.git

# Using git filter-branch (slower)
git filter-branch --tree-filter 'rm -f large-file.zip' HEAD
```

---

### "Cannot access remote repository"

**Diagnosis:**
```bash
# Check remote URL
git remote -v

# Test connectivity
ping github.com

# Test SSH (for SSH URLs)
ssh -T git@github.com
```

**Solutions:**
```bash
# Update remote URL
git remote set-url origin <new-url>

# Remove and re-add remote
git remote remove origin
git remote add origin <url>

# Check firewall/proxy settings
# May need to configure proxy:
git config --global http.proxy http://proxy.example.com:8080
```

---

## Performance Problems

### "Git operations are slow"

**Solutions:**

**Enable file system monitor:**
```bash
git config core.fsmonitor true
```

**Disable file mode checking:**
```bash
git config core.fileMode false
```

**Reduce delta depth:**
```bash
git config pack.depth 10
```

**Use shallow clone:**
```bash
git clone --depth 1 --single-branch <url>
```

---

### "Git status is slow"

**Solutions:**

**Ignore working directory:**
```bash
git status --untracked-files=no
```

**Update index:**
```bash
git update-index --refresh
```

**Enable parallel index preload:**
```bash
git config core.preloadindex true
```

---

## Recovery Scenarios

### "I deleted a file and committed"

**Solution:**
```bash
# Find commit that deleted file
git log --all --full-history -- <file>

# Restore from previous commit
git checkout <commit-before-deletion> -- <file>
git commit -m "Restore deleted file"
```

---

### "I deleted a branch by accident"

**Solution:**
```bash
# Find the commit where branch was
git reflog

# Recreate branch at that commit
git checkout -b recovered-branch <commit-hash>
```

---

### "I lost my commits"

**Solution:**
```bash
# Check reflog
git reflog

# Find your commit
# Create branch at that commit
git branch recovered <commit-hash>

# Or reset to that commit
git reset --hard <commit-hash>
```

---

### "I need to undo a merge"

**Before pushing:**
```bash
git reset --hard HEAD~1
```

**After pushing:**
```bash
git revert -m 1 <merge-commit-hash>
git push
```

---

### "I pushed sensitive data"

**Immediate actions:**

1. **Remove from latest commit:**
```bash
git rm --cached sensitive-file
echo "sensitive-file" >> .gitignore
git commit --amend -m "Remove sensitive file"
git push --force-with-lease
```

2. **Remove from history:**
```bash
# Using BFG Repo-Cleaner
java -jar bfg.jar --delete-files sensitive-file.txt repo.git

# Cleanup
git reflog expire --expire=now --all
git gc --prune=now --aggressive
```

3. **Rotate credentials** - If you pushed passwords, API keys, etc., change them immediately!

---

## Diagnostic Commands

### Check repository health
```bash
# Check integrity
git fsck --full

# Verify connectivity
git remote show origin

# Check object count
git count-objects -vH
```

### Get more information
```bash
# Verbose status
git status -v

# Show tracking branches
git branch -vv

# Show remote branches
git remote show origin

# Show configuration
git config --list --show-origin
```

### Debug Git commands
```bash
# Run with trace
GIT_TRACE=1 git status

# Curl verbose
GIT_CURL_VERBOSE=1 git push
```

---

## Prevention Tips

### 1. Commit Often
Small, frequent commits are easier to manage and undo.

### 2. Use Branches
Don't work directly on `main`. Create feature branches.

### 3. Pull Before Push
Always pull latest changes before pushing.

### 4. Review Before Committing
Use `git status` and `git diff` before committing.

### 5. Use .gitignore
Prevent committing unwanted files.

```bash
# Create .gitignore
cat > .gitignore << EOF
node_modules/
.env
*.log
.DS_Store
EOF
```

### 6. Backup Important Branches
```bash
# Create backup branch
git branch backup-main main
```

### 7. Use Force Push Carefully
```bash
# Safer force push
git push --force-with-lease

# Never force push shared branches!
```

---

## Getting Help

### Built-in Help
```bash
# Command help
git help <command>
git <command> --help

# Quick help
git <command> -h
```

### Community Resources
- [Stack Overflow Git Tag](https://stackoverflow.com/questions/tagged/git)
- [Git Official Documentation](https://git-scm.com/doc)
- [GitHub Community](https://github.community/)

### Debug Mode
```bash
# Run Git with debugging
GIT_TRACE=1 git <command>
GIT_TRACE_PACKET=1 git <command>
```

---

## Emergency Contacts (Commands)

```bash
# When in doubt
git status
git log --oneline -10
git reflog

# Abort current operation
git merge --abort
git rebase --abort
git cherry-pick --abort

# Go back to last known good state
git reset --hard origin/main

# Nuclear option (destroys uncommitted work!)
git reset --hard HEAD
git clean -fd
```

---

[← Back: Cheat Sheet](./06-cheat-sheet.md) | [Back to Main →](../README.md)
