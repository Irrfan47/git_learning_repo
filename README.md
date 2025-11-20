# Git Learning Repository 🚀

Welcome to your comprehensive guide to learning Git! This repository is designed to help you master Git commands from basic to advanced levels through clear explanations, practical examples, and hands-on exercises.

## 📚 Table of Contents

1. [Introduction to Git](#introduction-to-git)
2. [Getting Started](#getting-started)
3. [Learning Paths](#learning-paths)
4. [Quick Reference](#quick-reference)
5. [Contributing](#contributing)

## Introduction to Git

Git is a distributed version control system that tracks changes in your code, enables collaboration, and maintains a complete history of your project. Whether you're working solo or in a team, Git is an essential tool for modern software development.

### Why Learn Git?

- **Version Control**: Keep track of every change in your codebase
- **Collaboration**: Work seamlessly with team members
- **Branching**: Experiment with features without affecting the main code
- **History**: Never lose work and understand how your project evolved
- **Industry Standard**: Used by virtually every software development team

## Getting Started

### Prerequisites

Before you begin, make sure Git is installed on your system:

```bash
git --version
```

If you don't have Git installed, visit [git-scm.com](https://git-scm.com/) to download it.

### Configure Git

Set up your identity (required for commits):

```bash
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"
```

## Learning Paths

This repository contains structured guides to help you learn Git effectively:

### 1. [Git Basics](./docs/01-git-basics.md) 📖
Start here if you're new to Git. Learn fundamental commands like:
- `git init`, `git clone`
- `git add`, `git commit`
- `git status`, `git log`
- `git diff`, `git show`

### 2. [Git Branching](./docs/02-git-branching.md) 🌳
Master branching and merging:
- Creating and switching branches
- Merging strategies
- Resolving conflicts
- Branch management

### 3. [Git Collaboration](./docs/03-git-collaboration.md) 🤝
Learn to work with remote repositories:
- `git push`, `git pull`, `git fetch`
- Pull requests and code reviews
- Forking workflows
- Team collaboration best practices

### 4. [Git Advanced Topics](./docs/04-git-advanced.md) ⚡
Level up your Git skills:
- Interactive rebase
- Cherry-picking commits
- Stashing changes
- Git hooks
- Submodules

### 5. [Practical Exercises](./docs/05-exercises.md) 💪
Hands-on scenarios to practice:
- Real-world problem-solving
- Step-by-step exercises
- Common workflows

### 6. [Git Cheat Sheet](./docs/06-cheat-sheet.md) 📋
Quick reference for all essential commands

### 7. [Troubleshooting Guide](./docs/07-troubleshooting.md) 🔧
Solutions to common Git problems

## Quick Reference

### Most Used Commands

```bash
# Check status
git status

# Stage changes
git add <file>
git add .

# Commit changes
git commit -m "Your message"

# View history
git log --oneline

# Create branch
git branch <branch-name>

# Switch branch
git checkout <branch-name>
# or (newer syntax)
git switch <branch-name>

# Pull latest changes
git pull

# Push your changes
git push
```

## Learning Tips

1. **Practice Regularly**: The best way to learn Git is by using it
2. **Read Error Messages**: Git's error messages are often helpful
3. **Use `git status` Often**: It shows you what's happening
4. **Experiment Safely**: Create test repositories to try new commands
5. **Learn One Thing at a Time**: Don't try to master everything at once

## Additional Resources

- [Official Git Documentation](https://git-scm.com/doc)
- [Pro Git Book](https://git-scm.com/book/en/v2) (Free online)
- [GitHub Learning Lab](https://lab.github.com/)
- [Visualizing Git](https://git-school.github.io/visualizing-git/)

## Contributing

Found a typo or want to improve the content? Contributions are welcome! Please feel free to:
- Open an issue for suggestions
- Submit a pull request with improvements
- Share your feedback

## License

This repository is open source and available for educational purposes.

---

**Happy Learning! 🎉**

Start with [Git Basics](./docs/01-git-basics.md) and work your way through the guides at your own pace.