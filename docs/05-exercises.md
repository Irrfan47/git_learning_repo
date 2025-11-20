# Practical Exercises 💪

Hands-on exercises to practice Git commands and workflows in real-world scenarios.

## Table of Contents

1. [Beginner Exercises](#beginner-exercises)
2. [Intermediate Exercises](#intermediate-exercises)
3. [Advanced Exercises](#advanced-exercises)
4. [Workflow Scenarios](#workflow-scenarios)
5. [Challenge Projects](#challenge-projects)

---

## Beginner Exercises

### Exercise 1: First Repository

**Goal:** Create your first Git repository and make commits.

**Tasks:**
1. Create a new directory called `my-first-repo`
2. Initialize it as a Git repository
3. Create a file `hello.txt` with the text "Hello, Git!"
4. Stage and commit the file
5. Modify the file by adding a second line: "I'm learning Git!"
6. Stage and commit the change
7. View the commit history

<details>
<summary>💡 Hints</summary>

- Use `git init` to initialize
- Use `git add` to stage files
- Use `git commit -m` to commit
- Use `git log` to view history
</details>

<details>
<summary>✅ Solution</summary>

```bash
mkdir my-first-repo
cd my-first-repo
git init

echo "Hello, Git!" > hello.txt
git add hello.txt
git commit -m "Add hello.txt with greeting"

echo "I'm learning Git!" >> hello.txt
git add hello.txt
git commit -m "Add learning message to hello.txt"

git log --oneline
```
</details>

---

### Exercise 2: Working with Status and Diff

**Goal:** Practice checking status and viewing changes.

**Tasks:**
1. In your repository, create three files: `file1.txt`, `file2.txt`, `file3.txt`
2. Stage only `file1.txt` and `file2.txt`
3. Check the repository status
4. View the diff for staged changes
5. View the diff for unstaged changes
6. Commit the staged files
7. Stage and commit `file3.txt` separately

<details>
<summary>✅ Solution</summary>

```bash
echo "File 1" > file1.txt
echo "File 2" > file2.txt
echo "File 3" > file3.txt

git add file1.txt file2.txt
git status

git diff --staged
git diff

git commit -m "Add file1 and file2"

git add file3.txt
git commit -m "Add file3"

git log --oneline
```
</details>

---

### Exercise 3: Undoing Changes

**Goal:** Practice reverting unwanted changes.

**Tasks:**
1. Create a file `experiment.txt` and commit it
2. Modify the file but don't stage it
3. View the changes with `git diff`
4. Discard the unstaged changes
5. Verify the file is back to its original state

<details>
<summary>✅ Solution</summary>

```bash
echo "Original content" > experiment.txt
git add experiment.txt
git commit -m "Add experiment.txt"

echo "Unwanted changes" >> experiment.txt
git diff

git restore experiment.txt
# or: git checkout -- experiment.txt

cat experiment.txt  # Should show only original content
```
</details>

---

## Intermediate Exercises

### Exercise 4: Branch Management

**Goal:** Practice creating, switching, and managing branches.

**Tasks:**
1. Create a branch called `feature-1`
2. Switch to the new branch
3. Create a file `feature1.txt` and commit it
4. Switch back to `main`
5. Create another branch called `feature-2` from `main`
6. Create a file `feature2.txt` and commit it
7. Merge `feature-1` into `main`
8. View the branch list and delete `feature-1`

<details>
<summary>✅ Solution</summary>

```bash
git checkout -b feature-1
echo "Feature 1 content" > feature1.txt
git add feature1.txt
git commit -m "Add feature 1"

git checkout main
git checkout -b feature-2
echo "Feature 2 content" > feature2.txt
git add feature2.txt
git commit -m "Add feature 2"

git checkout main
git merge feature-1
git branch
git branch -d feature-1
```
</details>

---

### Exercise 5: Merge Conflicts

**Goal:** Learn to handle merge conflicts.

**Tasks:**
1. Create a file `shared.txt` with content "Original line" on `main`
2. Commit the file
3. Create branch `branch-a` and modify line 1 to "Change from branch A"
4. Commit on `branch-a`
5. Switch to `main` and modify line 1 to "Change from main"
6. Commit on `main`
7. Try to merge `branch-a` into `main`
8. Resolve the conflict by keeping both changes
9. Complete the merge

<details>
<summary>✅ Solution</summary>

```bash
echo "Original line" > shared.txt
git add shared.txt
git commit -m "Add shared.txt"

git checkout -b branch-a
echo "Change from branch A" > shared.txt
git add shared.txt
git commit -m "Update from branch A"

git checkout main
echo "Change from main" > shared.txt
git add shared.txt
git commit -m "Update from main"

git merge branch-a
# Conflict occurs!

# Edit shared.txt to:
# Change from main
# Change from branch A

git add shared.txt
git commit -m "Merge branch-a with resolved conflicts"
```
</details>

---

### Exercise 6: Remote Repository

**Goal:** Practice working with remote repositories.

**Setup:** You'll need a GitHub account for this exercise.

**Tasks:**
1. Create a new repository on GitHub (don't initialize with README)
2. Connect your local repository to the remote
3. Push your `main` branch to GitHub
4. Create a new branch `feature-remote`
5. Make changes and push the branch to GitHub
6. View your branches on GitHub

<details>
<summary>✅ Solution</summary>

```bash
# After creating repo on GitHub
git remote add origin https://github.com/your-username/your-repo.git
git push -u origin main

git checkout -b feature-remote
echo "Remote feature" > remote.txt
git add remote.txt
git commit -m "Add remote feature"
git push -u origin feature-remote

git branch -a  # See all branches including remote
```
</details>

---

## Advanced Exercises

### Exercise 7: Interactive Rebase

**Goal:** Clean up commit history with interactive rebase.

**Tasks:**
1. Create 5 commits with messages: "Add A", "Add B", "Fix A", "Add C", "Fix B"
2. Use interactive rebase to:
   - Squash "Fix A" into "Add A"
   - Squash "Fix B" into "Add B"
   - Reorder so "Add C" comes before the squashed commits
3. View the cleaned history

<details>
<summary>✅ Solution</summary>

```bash
echo "A" > file.txt && git add . && git commit -m "Add A"
echo "B" >> file.txt && git add . && git commit -m "Add B"
echo "A fixed" > file.txt && git add . && git commit -m "Fix A"
echo "C" >> file.txt && git add . && git commit -m "Add C"
echo "B fixed" >> file.txt && git add . && git commit -m "Fix B"

git rebase -i HEAD~5

# Change to:
# pick <hash> Add C
# pick <hash> Add A
# fixup <hash> Fix A
# pick <hash> Add B
# fixup <hash> Fix B

git log --oneline  # See cleaned history
```
</details>

---

### Exercise 8: Stashing Workflow

**Goal:** Practice using stash in a realistic workflow.

**Tasks:**
1. Start working on `feature-branch` and make some changes (don't commit)
2. Urgently need to fix a bug on `main`
3. Stash your changes with a descriptive message
4. Switch to `main`, create `bugfix` branch
5. Fix the bug and merge to `main`
6. Return to `feature-branch`
7. Apply your stashed changes
8. Complete and commit your feature

<details>
<summary>✅ Solution</summary>

```bash
git checkout -b feature-branch
echo "Feature in progress" > feature.txt
git add feature.txt
# Don't commit yet

git stash save "WIP: Feature in progress"

git checkout main
git checkout -b bugfix
echo "Bug fix" > fix.txt
git add fix.txt
git commit -m "Fix critical bug"
git checkout main
git merge bugfix
git branch -d bugfix

git checkout feature-branch
git stash pop

# Complete the feature
echo "Feature complete" >> feature.txt
git add feature.txt
git commit -m "Complete feature"
```
</details>

---

### Exercise 9: Cherry-Pick

**Goal:** Apply specific commits from one branch to another.

**Tasks:**
1. Create a `develop` branch with 3 commits
2. You need one specific commit on `main`
3. Use cherry-pick to apply that commit to `main`
4. Verify the commit exists on both branches

<details>
<summary>✅ Solution</summary>

```bash
git checkout -b develop
echo "Feature 1" > f1.txt && git add . && git commit -m "Add feature 1"
echo "Feature 2" > f2.txt && git add . && git commit -m "Add feature 2"
echo "Feature 3" > f3.txt && git add . && git commit -m "Add feature 3"

# Get hash of "Add feature 2"
git log --oneline

git checkout main
git cherry-pick <hash-of-feature-2>

# Verify
git log --oneline
ls  # Should see f2.txt but not f1.txt or f3.txt
```
</details>

---

### Exercise 10: Git Bisect

**Goal:** Find a bug using binary search.

**Tasks:**
1. Create a repository with 10 commits
2. Introduce a "bug" in commit 6 (e.g., add "BUG" to a file)
3. Use `git bisect` to find the commit that introduced the bug

<details>
<summary>✅ Solution</summary>

```bash
git init bisect-test
cd bisect-test

for i in {1..10}; do
  if [ $i -eq 6 ]; then
    echo "Code with BUG" > code.txt
  else
    echo "Good code $i" > code.txt
  fi
  git add code.txt
  git commit -m "Commit $i"
done

git bisect start
git bisect bad HEAD
git bisect good HEAD~9

# At each step, check code.txt
# If it contains "BUG": git bisect bad
# If it's clean: git bisect good

# Git will identify commit 6
git bisect reset
```
</details>

---

## Workflow Scenarios

### Scenario 1: Feature Development

**Situation:** You're developing a new login feature.

**Workflow:**
1. Update `main` branch
2. Create `feature/login` branch
3. Implement the feature in multiple commits
4. Keep your branch updated with `main`
5. Clean up your commit history
6. Push and create a pull request

<details>
<summary>📝 Step-by-Step</summary>

```bash
# 1. Update main
git checkout main
git pull origin main

# 2. Create feature branch
git checkout -b feature/login

# 3. Implement feature
echo "Login form HTML" > login.html
git add login.html
git commit -m "Add login form HTML"

echo "Login validation JS" > login.js
git add login.js
git commit -m "Add login validation"

echo "Login styles CSS" > login.css
git add login.css
git commit -m "Add login styles"

# 4. Keep branch updated
git fetch origin
git merge origin/main
# or: git rebase origin/main

# 5. Clean up commits (optional)
git rebase -i HEAD~3

# 6. Push and create PR
git push -u origin feature/login
# Create PR on GitHub
```
</details>

---

### Scenario 2: Hotfix in Production

**Situation:** Production has a critical bug that needs immediate fixing.

**Workflow:**
1. Create a hotfix branch from `main`
2. Fix the bug
3. Test the fix
4. Merge to `main` and deploy
5. Also merge to `develop` (if you have one)
6. Clean up the hotfix branch

<details>
<summary>📝 Step-by-Step</summary>

```bash
# 1. Create hotfix branch
git checkout main
git pull
git checkout -b hotfix/critical-bug

# 2. Fix the bug
echo "Fixed bug" > fix.txt
git add fix.txt
git commit -m "Fix critical production bug"

# 3. Test
# ... run tests ...

# 4. Merge to main
git checkout main
git merge hotfix/critical-bug
git push origin main
# ... deploy to production ...

# 5. Merge to develop
git checkout develop
git merge hotfix/critical-bug
git push origin develop

# 6. Clean up
git branch -d hotfix/critical-bug
git push origin --delete hotfix/critical-bug
```
</details>

---

### Scenario 3: Collaborating on a Feature

**Situation:** Two developers working on the same feature branch.

**Developer 1:**
1. Creates feature branch
2. Makes initial commit
3. Pushes to remote

**Developer 2:**
1. Fetches the branch
2. Makes changes
3. Pushes to remote

**Developer 1:**
1. Pulls latest changes
2. Continues working
3. Resolves any conflicts

<details>
<summary>📝 Step-by-Step</summary>

```bash
# Developer 1
git checkout -b feature/shared-feature
echo "Initial work" > shared.txt
git add shared.txt
git commit -m "Start shared feature"
git push -u origin feature/shared-feature

# Developer 2
git fetch origin
git checkout -b feature/shared-feature origin/feature/shared-feature
echo "Additional work" >> shared.txt
git add shared.txt
git commit -m "Add more to shared feature"
git push

# Developer 1
git pull  # Get Developer 2's changes
echo "More work" >> shared.txt
git add shared.txt
git commit -m "Continue working on feature"
git push

# Both developers can see each other's work!
```
</details>

---

## Challenge Projects

### Challenge 1: Build a Blog

Create a Git repository to track a simple blog project.

**Requirements:**
- Use feature branches for each blog post
- Create tags for each "release" (published batch of posts)
- Practice merging and conflict resolution
- Set up `.gitignore` for draft files

---

### Challenge 2: Contribute to Open Source

Find a small open-source project and contribute.

**Steps:**
1. Fork the repository
2. Clone your fork
3. Set up upstream remote
4. Find an issue to work on
5. Create a branch
6. Make your changes
7. Write good commit messages
8. Submit a pull request

---

### Challenge 3: Team Workflow Simulation

Simulate a team workflow by yourself.

**Setup:**
- Create a repository with `main`, `develop`, and `feature` branches
- Simulate multiple developers with different branches
- Practice merging, rebasing, and resolving conflicts
- Use pull requests (on GitHub)
- Tag releases

---

## Tips for Practice

1. **Create a Practice Repository:** Set up a dedicated repo for experiments
2. **Don't Fear Mistakes:** You can always reset or start over
3. **Read Error Messages:** Git's errors are usually helpful
4. **Use `git status` Frequently:** Know where you are
5. **Experiment with Different Approaches:** Try multiple ways to solve the same problem
6. **Document Your Learning:** Keep notes on what works
7. **Practice Real Scenarios:** Apply what you learn to actual projects

---

## Self-Assessment Checklist

### Beginner Level ✓
- [ ] Can initialize a repository
- [ ] Can stage and commit changes
- [ ] Can view commit history
- [ ] Can undo uncommitted changes
- [ ] Can view file differences

### Intermediate Level ✓
- [ ] Can create and switch branches
- [ ] Can merge branches
- [ ] Can resolve merge conflicts
- [ ] Can work with remote repositories
- [ ] Can push and pull changes

### Advanced Level ✓
- [ ] Can use interactive rebase
- [ ] Can cherry-pick commits
- [ ] Can use stash effectively
- [ ] Can recover lost commits with reflog
- [ ] Can use Git hooks

---

## Additional Practice Resources

1. **GitHub Learning Lab**: Interactive tutorials
2. **Git Branching Game**: https://learngitbranching.js.org/
3. **Katacoda Git Scenarios**: Interactive command-line scenarios
4. **Try Git**: https://try.github.io/
5. **Git Katas**: https://github.com/eficode-academy/git-katas

---

[← Back: Git Advanced](./04-git-advanced.md) | [Next: Cheat Sheet →](./06-cheat-sheet.md)
