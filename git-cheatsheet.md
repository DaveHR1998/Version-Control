# Git and GitHub Cheat Sheet

A quick reference guide for common Git and GitHub commands.

## Setup and Configuration

```bash
# Check Git version
git --version

# Configure user information
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"

# Configure default editor
git config --global core.editor "code --wait"  # For VS Code

# List all configurations
git config --list
```

## Creating Repositories

```bash
# Initialize a new repository
git init

# Clone an existing repository
git clone https://github.com/username/repository.git

# Clone to a specific folder
git clone https://github.com/username/repository.git folder-name
```

## Basic Snapshotting

```bash
# Check status
git status

# Add files to staging area
git add filename.txt    # Add specific file
git add .               # Add all files
git add *.txt           # Add all text files

# Remove files from staging area
git restore --staged filename.txt

# Commit changes
git commit -m "Commit message"
git commit -a -m "Commit message"  # Add and commit in one step

# Amend last commit
git commit --amend -m "New commit message"
```

## Viewing Changes

```bash
# Show differences
git diff                # Unstaged changes
git diff --staged       # Staged changes
git diff HEAD           # All changes

# Show commit history
git log
git log --oneline       # Compact view
git log --graph         # With branch graph
git log -p              # With diffs
git log --stat          # With stats

# Show specific commit
git show commit-hash
```

## Undoing Changes

```bash
# Discard changes in working directory
git restore filename.txt
git checkout -- filename.txt  # Older Git versions

# Unstage changes
git restore --staged filename.txt
git reset HEAD filename.txt   # Older Git versions

# Reset to a previous commit
git reset commit-hash         # Soft (keep changes)
git reset --hard commit-hash  # Hard (discard changes)

# Revert a commit
git revert commit-hash
```

## Branching and Merging

```bash
# List branches
git branch              # Local branches
git branch -a           # All branches including remote
git branch -r           # Remote branches only

# Create branch
git branch branch-name
git checkout -b branch-name  # Create and switch

# Switch branches
git checkout branch-name
git switch branch-name       # Git 2.23+

# Rename branch
git branch -m old-name new-name

# Delete branch
git branch -d branch-name    # Safe delete
git branch -D branch-name    # Force delete

# Merge branch
git merge branch-name

# Abort merge
git merge --abort
```

## Remote Repositories

```bash
# List remotes
git remote -v

# Add remote
git remote add origin https://github.com/username/repo.git

# Remove remote
git remote remove origin

# Fetch from remote
git fetch origin
git fetch --all

# Pull from remote
git pull origin main

# Push to remote
git push origin main
git push -u origin main      # Set upstream

# Delete remote branch
git push origin --delete branch-name
```

## Stashing

```bash
# Save changes to stash
git stash
git stash save "message"

# List stashes
git stash list

# Apply stash
git stash apply            # Latest stash
git stash apply stash@{n}  # Specific stash

# Apply and remove stash
git stash pop

# Remove stash
git stash drop stash@{n}
git stash clear            # Remove all stashes
```

## Tagging

```bash
# List tags
git tag

# Create tag
git tag v1.0.0                         # Lightweight tag
git tag -a v1.0.0 -m "Version 1.0.0"   # Annotated tag

# Tag a specific commit
git tag -a v1.0.0 commit-hash -m "Version 1.0.0"

# Push tags
git push origin v1.0.0    # Specific tag
git push origin --tags    # All tags

# Delete tag
git tag -d v1.0.0         # Local
git push origin --delete v1.0.0  # Remote
```

## Advanced Operations

```bash
# Interactive rebase
git rebase -i HEAD~3

# Cherry-pick
git cherry-pick commit-hash

# Bisect
git bisect start
git bisect bad
git bisect good commit-hash
git bisect reset

# Reflog (recovery)
git reflog
git checkout HEAD@{n}

# Clean untracked files
git clean -n    # Dry run
git clean -f    # Force delete
git clean -fd   # Force delete including directories
```

## GitHub Specific

```bash
# Create pull request (from GitHub UI)
# After pushing a branch to GitHub:
# - Go to repository
# - Click "Compare & pull request"
# - Fill in details and submit

# Clone with SSH
git clone git@github.com:username/repository.git

# Add SSH remote
git remote add origin git@github.com:username/repository.git

# Fork workflow
# 1. Fork on GitHub
# 2. Clone your fork
git clone https://github.com/your-username/repository.git

# 3. Add upstream remote
git remote add upstream https://github.com/original-owner/repository.git

# 4. Sync fork
git fetch upstream
git checkout main
git merge upstream/main
```

## Git Workflows

### Feature Branch Workflow
```bash
# Start a feature
git checkout -b feature-name

# Work on feature
# ... make changes ...
git add .
git commit -m "Add feature"

# Push feature to remote
git push -u origin feature-name

# Create pull request (on GitHub)
# Merge pull request (on GitHub)

# Update local main
git checkout main
git pull
```

### Git Flow
```bash
# Feature
git checkout develop
git checkout -b feature/name
# ... work ...
git checkout develop
git merge --no-ff feature/name

# Release
git checkout -b release/1.0 develop
# ... finalize ...
git checkout main
git merge --no-ff release/1.0
git tag -a v1.0.0
git checkout develop
git merge --no-ff release/1.0

# Hotfix
git checkout -b hotfix/issue main
# ... fix ...
git checkout main
git merge --no-ff hotfix/issue
git tag -a v1.0.1
git checkout develop
git merge --no-ff hotfix/issue
```

## Common .gitignore Patterns

```
# Node.js
node_modules/
npm-debug.log

# Python
__pycache__/
*.py[cod]
*$py.class
venv/
.env

# Java
*.class
*.jar
target/

# C/C++
*.o
*.obj
*.exe

# IDE files
.idea/
.vscode/
*.sublime-*

# OS files
.DS_Store
Thumbs.db

# Logs
*.log
logs/

# Build directories
build/
dist/
```

## Git Aliases (add to .gitconfig)

```
[alias]
    st = status
    co = checkout
    br = branch
    ci = commit
    unstage = restore --staged
    last = log -1 HEAD
    lg = log --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit
```
