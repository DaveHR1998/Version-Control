# Git and GitHub Tutorial: From Beginner to Advanced

Welcome to this comprehensive Git and GitHub tutorial! This guide is designed to take you from the very basics of version control to advanced Git techniques, with step-by-step instructions and explanations.

## Table of Contents
1. [Introduction to Version Control](#introduction-to-version-control)
2. [Getting Started with Git](#getting-started-with-git)
3. [Basic Git Commands](#basic-git-commands)
4. [Working with GitHub](#working-with-github)
5. [Branching and Merging](#branching-and-merging)
6. [Collaboration Workflows](#collaboration-workflows)
7. [Advanced Git Techniques](#advanced-git-techniques)
8. [Best Practices](#best-practices)
9. [Troubleshooting Common Issues](#troubleshooting-common-issues)

## Introduction to Version Control

### What is Version Control?
Version control is a system that records changes to files over time so that you can recall specific versions later. It allows you to:
- Track changes to your code
- Revert to previous versions if needed
- Collaborate with others without overwriting each other's work
- Maintain different versions of a project simultaneously

### Types of Version Control Systems
1. **Local Version Control Systems**: Simple database on your local machine
2. **Centralized Version Control Systems (CVCS)**: Single server storing all versioned files (e.g., SVN)
3. **Distributed Version Control Systems (DVCS)**: Every user has a complete copy of the repository (e.g., Git)

### Why Git?
- **Distributed**: Each developer has a complete copy of the repository
- **Fast**: Most operations are local, reducing network latency
- **Data integrity**: Git uses checksums to ensure data integrity
- **Branching**: Git's branching model is lightweight and powerful
- **Open source**: Free and widely adopted

## Getting Started with Git

### Installing Git
- **Windows**: Download and install from [git-scm.com](https://git-scm.com/download/win)
- **macOS**: Install via Homebrew with `brew install git` or download from [git-scm.com](https://git-scm.com/download/mac)
- **Linux**: Use your distribution's package manager, e.g., `sudo apt-get install git` (Ubuntu/Debian)

### Configuring Git
After installation, set up your identity:

```bash
# Set your name
git config --global user.name "Your Name"

# Set your email
git config --global user.email "your.email@example.com"

# Check your settings
git config --list
```

### Creating Your First Repository
```bash
# Create a new directory
mkdir my-first-repo
cd my-first-repo

# Initialize a Git repository
git init
```

## Basic Git Commands

### Understanding the Git Workflow
Git has three main states that your files can reside in:
1. **Modified**: You've changed the file but haven't committed it yet
2. **Staged**: You've marked a modified file to go into your next commit
3. **Committed**: The data is safely stored in your local database

### Tracking Files
```bash
# Check status of your repository
git status

# Add a file to staging area
git add filename.txt

# Add all files to staging area
git add .

# Remove a file from staging area
git reset filename.txt
```

### Committing Changes
```bash
# Commit staged changes
git commit -m "Your commit message here"

# Commit all changes (skips staging)
git commit -a -m "Your commit message here"
```

### Viewing History
```bash
# View commit history
git log

# View compact history
git log --oneline

# View graphical representation
git log --graph --oneline --decorate
```

## Working with GitHub

### What is GitHub?
GitHub is a web-based hosting service for Git repositories. It provides:
- A centralized location to store repositories
- Tools for collaboration (pull requests, issues, etc.)
- Project management features
- Social coding features

### Creating a GitHub Account
1. Go to [github.com](https://github.com)
2. Fill out the sign-up form
3. Choose a free or paid plan
4. Verify your email address

### Creating a Repository on GitHub
1. Click the "+" icon in the top right corner
2. Select "New repository"
3. Fill in repository name and description
4. Choose public or private
5. Initialize with README (optional)
6. Click "Create repository"

### Connecting Local Repository to GitHub
```bash
# Add remote repository
git remote add origin https://github.com/username/repository-name.git

# Verify remote
git remote -v

# Push your local repository to GitHub
git push -u origin main
```

### Cloning a Repository
```bash
# Clone a repository
git clone https://github.com/username/repository-name.git

# Clone to a specific folder
git clone https://github.com/username/repository-name.git folder-name
```

## Branching and Merging

### Understanding Branches
A branch represents an independent line of development. It allows you to:
- Work on new features without affecting the main codebase
- Experiment with ideas
- Collaborate with others without conflicts

### Working with Branches
```bash
# List all branches
git branch

# Create a new branch
git branch branch-name

# Switch to a branch
git checkout branch-name

# Create and switch to a new branch
git checkout -b branch-name

# Delete a branch
git branch -d branch-name
```

### Merging Branches
```bash
# Switch to the target branch (e.g., main)
git checkout main

# Merge another branch into current branch
git merge branch-name

# Abort a merge in case of conflicts
git merge --abort
```

### Handling Merge Conflicts
When Git can't automatically merge changes, you'll need to resolve conflicts manually:

1. Git will mark the conflicted files
2. Open the files and look for conflict markers (`<<<<<<<`, `=======`, `>>>>>>>`)
3. Edit the files to resolve conflicts
4. Add the resolved files with `git add`
5. Complete the merge with `git commit`

## Collaboration Workflows

### Pull Requests
A pull request (PR) is a method of submitting contributions to a project:

1. Fork the repository (on GitHub)
2. Clone your fork locally
3. Create a branch for your changes
4. Make and commit your changes
5. Push your branch to your fork
6. Create a pull request on GitHub

### Keeping Your Fork Updated
```bash
# Add the original repository as "upstream"
git remote add upstream https://github.com/original-owner/original-repository.git

# Fetch changes from upstream
git fetch upstream

# Merge changes into your local main branch
git checkout main
git merge upstream/main

# Push the updated main to your fork
git push origin main
```

### Collaborative Workflows
1. **Centralized Workflow**: Everyone works on the main branch
2. **Feature Branch Workflow**: Each feature is developed in its own branch
3. **Gitflow Workflow**: Strict branching model with dedicated branches for features, releases, and hotfixes
4. **Forking Workflow**: Contributors fork the repository and submit pull requests

## Advanced Git Techniques

### Stashing Changes
```bash
# Save changes temporarily
git stash

# List stashes
git stash list

# Apply most recent stash
git stash apply

# Apply specific stash
git stash apply stash@{n}

# Remove most recent stash
git stash drop

# Apply and remove most recent stash
git stash pop
```

### Rebasing
```bash
# Rebase current branch onto another branch
git rebase branch-name

# Interactive rebase
git rebase -i HEAD~3  # Rebase last 3 commits
```

### Cherry-Picking
```bash
# Apply a specific commit to current branch
git cherry-pick commit-hash
```

### Tagging
```bash
# Create a lightweight tag
git tag tag-name

# Create an annotated tag
git tag -a v1.0 -m "Version 1.0"

# List tags
git tag

# Push tags to remote
git push origin --tags
```

### Reflog
```bash
# View reference logs
git reflog

# Recover deleted branch
git checkout -b recovered-branch commit-hash
```

### Submodules
```bash
# Add a submodule
git submodule add https://github.com/username/repository.git path/to/submodule

# Initialize and update submodules
git submodule update --init --recursive
```

## Best Practices

### Commit Messages
- Write clear, concise commit messages
- Use the imperative mood ("Add feature" not "Added feature")
- First line should be 50 characters or less
- Provide detailed explanation in the body if necessary

### Repository Organization
- Use a `.gitignore` file to exclude unnecessary files
- Include a README.md with project information
- Document your code and workflows

### Workflow Tips
- Commit often, push regularly
- Pull before you push
- Use branches for features and bug fixes
- Review code before merging

## Troubleshooting Common Issues

### Undoing Changes
```bash
# Discard changes in working directory
git checkout -- filename

# Undo last commit but keep changes
git reset --soft HEAD^

# Undo last commit and discard changes
git reset --hard HEAD^

# Undo a public commit (creates a new commit)
git revert commit-hash
```

### Fixing Mistakes
```bash
# Amend the last commit
git commit --amend -m "New commit message"

# Force push (use with caution!)
git push -f origin branch-name
```

### Common Error Messages
- **"fatal: refusing to merge unrelated histories"**: Use `git pull origin main --allow-unrelated-histories`
- **"fatal: remote origin already exists"**: Use `git remote rm origin` before adding a new origin
- **"error: failed to push some refs"**: Pull changes first with `git pull origin main`

---

This tutorial covers the fundamentals of Git and GitHub. As you practice these commands and concepts, you'll become more comfortable with version control and be able to manage your projects more effectively.

For more information, check out:
- [Git Documentation](https://git-scm.com/doc)
- [GitHub Guides](https://guides.github.com)
- [Pro Git Book](https://git-scm.com/book/en/v2)
