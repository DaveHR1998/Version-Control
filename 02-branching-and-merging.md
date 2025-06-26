# Branching and Merging in Git

This guide explains Git's powerful branching and merging capabilities with practical examples.

## Understanding Branches

A branch in Git is simply a lightweight movable pointer to a commit. The default branch is called `main` (previously `master`). When you create a new branch, you're creating a new pointer to the current commit.

### Why Use Branches?

- **Parallel Development**: Work on multiple features simultaneously
- **Isolation**: Experiment without affecting the stable codebase
- **Collaboration**: Multiple developers can work on the same project without interference
- **Organization**: Keep bug fixes, features, and experiments separate

## Basic Branch Operations

### Viewing Branches

```bash
# List local branches
git branch

# List all branches (local and remote)
git branch -a

# List remote branches
git branch -r

# Show more details about branches
git branch -v
```

### Creating Branches

```bash
# Create a new branch (but stay on current branch)
git branch feature-login

# Create and switch to a new branch
git checkout -b feature-signup

# Create a branch from a specific commit
git branch hotfix-bug123 a7d3f1c
```

When you create a branch, Git simply creates a new pointer - it doesn't change the repository in any other way.

### Switching Between Branches

```bash
# Switch to an existing branch
git checkout feature-login

# Switch back to the previous branch
git checkout -
```

When you switch branches, Git updates the files in your working directory to match the snapshot of the target branch.

### Deleting Branches

```bash
# Delete a fully merged branch
git branch -d feature-done

# Force delete a branch (even if not merged)
git branch -D experimental-feature
```

## Merging Branches

Merging combines the changes from one branch into another.

### Fast-Forward Merge

A fast-forward merge occurs when the target branch is a direct descendant of the current branch.

```bash
# Switch to the receiving branch
git checkout main

# Merge the feature branch
git merge feature-login
```

### Three-Way Merge

When the branches have diverged, Git creates a new "merge commit" that has two parent commits.

```bash
git checkout main
git merge feature-complex
```

### Merge Conflicts

When Git can't automatically resolve differences between branches, it creates a merge conflict.

```bash
# Abort a merge with conflicts
git merge --abort

# After resolving conflicts manually
git add resolved-file.txt
git commit
```

## Practical Exercise: Feature Branch Workflow

1. Create a new repository
   ```bash
   mkdir feature-branch-demo
   cd feature-branch-demo
   git init
   ```

2. Create an initial file and commit
   ```bash
   echo "# Feature Branch Demo" > README.md
   git add README.md
   git commit -m "Initial commit"
   ```

3. Create a feature branch
   ```bash
   git checkout -b feature-navbar
   ```

4. Make changes in the feature branch
   ```bash
   echo "
   ## Navigation Bar
   
   - Home
   - About
   - Contact
   " >> README.md
   git add README.md
   git commit -m "Add navigation bar structure"
   ```

5. Switch back to main branch
   ```bash
   git checkout main
   ```

6. Make a different change in main
   ```bash
   echo "
   ## Project Description
   
   This project demonstrates the feature branch workflow.
   " >> README.md
   git add README.md
   git commit -m "Add project description"
   ```

7. Merge the feature branch into main
   ```bash
   git merge feature-navbar
   ```

8. Resolve any merge conflicts if they occur
   ```bash
   # Edit the file to resolve conflicts
   git add README.md
   git commit -m "Merge feature-navbar into main"
   ```

9. View the commit history with branch graph
   ```bash
   git log --graph --oneline --all
   ```

## Advanced Branching Techniques

### Rebasing

Rebasing is an alternative to merging that rewrites commit history to create a linear sequence.

```bash
# While on feature branch
git rebase main

# Interactive rebase (for cleaning up commits)
git rebase -i HEAD~3
```

⚠️ **Warning**: Never rebase commits that have been pushed to a public repository!

### Branch Management Strategies

1. **GitHub Flow**: Simple workflow with just main and feature branches
   - Create a branch from main
   - Add commits
   - Open a pull request
   - Discuss and review
   - Merge to main

2. **Git Flow**: More structured workflow with multiple branch types
   - `main`: Production-ready code
   - `develop`: Latest delivered development changes
   - `feature/*`: New features
   - `release/*`: Preparing for a release
   - `hotfix/*`: Urgent fixes for production

3. **Trunk-Based Development**: Everyone commits to main (trunk) frequently
   - Short-lived feature branches
   - Feature toggles for incomplete work
   - Continuous integration

## Visualizing Branches

```bash
# Text-based visualization
git log --graph --oneline --decorate --all

# Using a GUI tool
gitk --all
```

## Tips for Effective Branching

1. **Keep branches focused**: Each branch should represent one logical change
2. **Keep branches short-lived**: Merge or delete branches promptly
3. **Name branches consistently**: Use prefixes like `feature/`, `bugfix/`, `hotfix/`
4. **Update branches regularly**: Rebase or merge from main often
5. **Clean up old branches**: Delete branches after they're merged
