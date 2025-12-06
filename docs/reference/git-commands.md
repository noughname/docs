---
title: Git Commands Cheat Sheet
layout: default
parent: Reference
nav_order: 3
---

# Git Commands Cheat Sheet

Quick reference for essential Git commands.

{: .fs-6 .fw-300 }

---

## Table of Contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## Basic Commands

### Initialize Repository

```bash
# Create a new repository
git init

# Clone existing repository
git clone https://github.com/user/repo.git

# Clone to specific directory
git clone https://github.com/user/repo.git my-folder
```

### Configuration

```bash
# Set user name
git config --global user.name "Your Name"

# Set email
git config --global user.email "your.email@example.com"

# List all settings
git config --list

# Edit global config
git config --global --edit
```

---

## Checking Status

### View Changes

```bash
# Show status of working directory
git status

# Show compact status
git status -s

# Show branch information
git status -b
```

### View Differences

```bash
# Show unstaged changes
git diff

# Show staged changes
git diff --staged

# Show changes in specific file
git diff filename.txt

# Show word-level diff
git diff --word-diff
```

---

## Making Changes

### Stage Changes

```bash
# Stage specific file
git add filename.txt

# Stage all changes
git add .

# Stage all modified and deleted files (not new files)
git add -u

# Stage with patch mode (interactive)
git add -p
```

### Commit Changes

```bash
# Commit staged changes
git commit -m "Commit message"

# Commit with detailed message
git commit -m "Short description" -m "Detailed explanation"

# Stage and commit all tracked files
git commit -am "Commit message"

# Amend last commit
git commit --amend

# Amend without changing message
git commit --amend --no-edit
```

### Undo Changes

```bash
# Unstage file (keep changes)
git reset HEAD filename.txt

# Discard changes in working directory
git checkout -- filename.txt

# Unstage all files
git reset HEAD

# Undo last commit (keep changes)
git reset --soft HEAD~1

# Undo last commit (discard changes)
git reset --hard HEAD~1
```

---

## Branching

### Create Branches

```bash
# Create new branch
git branch feature-name

# Create and switch to branch
git checkout -b feature-name

# Create branch from specific commit
git branch feature-name abc123
```

### Switch Branches

```bash
# Switch to existing branch
git checkout branch-name

# Switch to previous branch
git checkout -

# Switch using new syntax (Git 2.23+)
git switch branch-name
```

### List Branches

```bash
# List local branches
git branch

# List all branches (local and remote)
git branch -a

# List remote branches
git branch -r

# Show last commit on each branch
git branch -v
```

### Delete Branches

```bash
# Delete merged branch
git branch -d branch-name

# Force delete branch
git branch -D branch-name

# Delete remote branch
git push origin --delete branch-name
```

---

## Merging

### Merge Branches

```bash
# Merge branch into current branch
git merge feature-branch

# Merge without fast-forward
git merge --no-ff feature-branch

# Merge and squash commits
git merge --squash feature-branch
```

### Resolve Conflicts

```bash
# Show files with conflicts
git status

# Mark conflict as resolved
git add filename.txt

# Abort merge
git merge --abort

# Continue after resolving
git commit
```

---

## Remote Repositories

### Remote Operations

```bash
# List remotes
git remote -v

# Add remote
git remote add origin https://github.com/user/repo.git

# Change remote URL
git remote set-url origin https://github.com/user/new-repo.git

# Remove remote
git remote remove origin

# Rename remote
git remote rename old-name new-name
```

### Fetch and Pull

```bash
# Fetch changes from remote
git fetch origin

# Fetch all remotes
git fetch --all

# Pull changes (fetch + merge)
git pull origin main

# Pull with rebase
git pull --rebase origin main
```

### Push Changes

```bash
# Push to remote
git push origin main

# Push and set upstream
git push -u origin feature-branch

# Push all branches
git push --all

# Push tags
git push --tags

# Force push (use with caution!)
git push --force origin main
```

---

## History

### View Commit History

```bash
# Show commit history
git log

# One line per commit
git log --oneline

# Show graph
git log --graph --oneline

# Show last N commits
git log -n 5

# Show commits by author
git log --author="John Doe"

# Show commits in date range
git log --since="2 weeks ago" --until="yesterday"
```

### Show Commit Details

```bash
# Show commit details
git show abc123

# Show specific file from commit
git show abc123:path/to/file.txt

# Show changes in commit
git show --stat abc123
```

### Search History

```bash
# Search commit messages
git log --grep="bug fix"

# Search code changes
git log -S "function_name"

# Show file history
git log --follow filename.txt
```

---

## Stashing

### Save Work Temporarily

```bash
# Stash changes
git stash

# Stash with message
git stash save "Work in progress"

# Stash including untracked files
git stash -u

# List stashes
git stash list
```

### Apply Stashed Changes

```bash
# Apply most recent stash
git stash apply

# Apply specific stash
git stash apply stash@{2}

# Apply and remove stash
git stash pop

# Remove stash
git stash drop stash@{0}

# Clear all stashes
git stash clear
```

---

## Tags

### Create Tags

```bash
# Create lightweight tag
git tag v1.0.0

# Create annotated tag
git tag -a v1.0.0 -m "Version 1.0.0"

# Tag specific commit
git tag v1.0.0 abc123
```

### List and Show Tags

```bash
# List all tags
git tag

# List tags matching pattern
git tag -l "v1.*"

# Show tag details
git show v1.0.0
```

### Push and Delete Tags

```bash
# Push tag to remote
git push origin v1.0.0

# Push all tags
git push --tags

# Delete local tag
git tag -d v1.0.0

# Delete remote tag
git push origin --delete v1.0.0
```

---

## Advanced Operations

### Rebase

```bash
# Rebase current branch onto main
git rebase main

# Interactive rebase last 3 commits
git rebase -i HEAD~3

# Continue rebase after resolving conflicts
git rebase --continue

# Abort rebase
git rebase --abort

# Skip current commit
git rebase --skip
```

### Cherry-Pick

```bash
# Apply commit to current branch
git cherry-pick abc123

# Cherry-pick multiple commits
git cherry-pick abc123 def456

# Cherry-pick without committing
git cherry-pick --no-commit abc123
```

### Reset and Revert

```bash
# Soft reset (keep changes staged)
git reset --soft HEAD~1

# Mixed reset (keep changes unstaged)
git reset --mixed HEAD~1

# Hard reset (discard changes)
git reset --hard HEAD~1

# Revert commit (create new commit)
git revert abc123

# Revert multiple commits
git revert abc123 def456
```

---

## Submodules

### Working with Submodules

```bash
# Add submodule
git submodule add https://github.com/user/repo.git path/to/submodule

# Initialize submodules
git submodule init

# Update submodules
git submodule update

# Clone with submodules
git clone --recursive https://github.com/user/repo.git

# Update all submodules to latest
git submodule update --remote
```

---

## Useful Aliases

### Configure Aliases

```bash
# Add aliases to ~/.gitconfig
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.ci commit
git config --global alias.st status
git config --global alias.unstage 'reset HEAD --'
git config --global alias.last 'log -1 HEAD'
git config --global alias.lg "log --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"
```

### Use Aliases

```bash
git co main          # checkout main
git br -a            # branch -a
git ci -m "message"  # commit -m "message"
git st               # status
git unstage file.txt # reset HEAD -- file.txt
git last             # log -1 HEAD
git lg               # pretty log graph
```

---

## Troubleshooting

### Common Issues

#### Undo Last Commit

```bash
# Keep changes
git reset --soft HEAD~1

# Discard changes
git reset --hard HEAD~1
```

#### Fix Wrong Branch

```bash
# Move commits to new branch
git branch feature-branch
git reset --hard HEAD~3
git checkout feature-branch
```

#### Clean Working Directory

```bash
# Remove untracked files
git clean -n  # dry run
git clean -f  # force remove

# Remove untracked files and directories
git clean -fd

# Remove ignored files too
git clean -fdx
```

#### Recover Deleted Commit

```bash
# Show reflog
git reflog

# Checkout commit
git checkout abc123

# Create branch from commit
git branch recovered-branch abc123
```

---

## Best Practices

### Commit Messages

Good commit message format:

```
Short summary (50 chars or less)

Detailed explanation if needed. Wrap at 72 characters.
Explain what and why, not how.

- Bullet points are okay
- Use present tense: "Add feature" not "Added feature"
- Reference issues: "Fixes #123"
```

### Branch Naming

Common conventions:

```
feature/user-authentication
bugfix/login-error
hotfix/security-patch
release/v1.2.0
docs/api-documentation
```

### Workflow Tips

```bash
# Always pull before pushing
git pull --rebase origin main
git push origin feature-branch

# Keep commits small and focused
git add -p  # Stage changes interactively

# Review before committing
git diff --staged

# Keep main branch clean
git checkout main
git pull
git checkout -b feature-branch
```

---

## Quick Reference Table

| Command | Description |
|---------|-------------|
| `git status` | Show working directory status |
| `git add <file>` | Stage file |
| `git commit -m "msg"` | Commit staged changes |
| `git push` | Push to remote |
| `git pull` | Fetch and merge |
| `git branch` | List branches |
| `git checkout <branch>` | Switch branch |
| `git merge <branch>` | Merge branch |
| `git log` | Show history |
| `git diff` | Show changes |

---

## Additional Resources

- [Official Git Documentation](https://git-scm.com/doc)
- [Pro Git Book](https://git-scm.com/book/en/v2)
- [Git Cheat Sheet (GitHub)](https://education.github.com/git-cheat-sheet-education.pdf)
- [Oh Shit, Git!?!](https://ohshitgit.com/) - How to fix common mistakes

---

## Related Topics

- [GitHub Pages Setup]({% link docs/guides/github-pages.md %})
- [Configuration Examples]({% link docs/reference/config-examples.md %})
