# Advanced Git Techniques

This guide covers advanced Git operations for users who have already mastered the basics of Git and GitHub.

## Git Internals

### Understanding Git Objects

Git stores all data as four types of objects:

1. **Blobs**: The content of files
2. **Trees**: Directory listings, pointing to blobs or other trees
3. **Commits**: Pointers to trees, with metadata (author, date, message)
4. **Tags**: Named pointers to specific commits

```bash
# View a commit object
git cat-file -p <commit-hash>

# View a tree object
git cat-file -p <tree-hash>

# View a blob object
git cat-file -p <blob-hash>
```

### The Git Reference System

Git uses references (refs) to track branches, tags, and other pointers:

```bash
# List all references
git show-ref

# View the reference log
git reflog
```

## Advanced History Manipulation

### Interactive Rebase

Interactive rebase allows you to modify commit history in various ways:

```bash
# Start an interactive rebase for the last 5 commits
git rebase -i HEAD~5
```

In the interactive rebase editor, you can:
- `pick`: Keep the commit as is
- `reword`: Change the commit message
- `edit`: Stop for amending
- `squash`: Combine with previous commit and keep messages
- `fixup`: Combine with previous commit and discard message
- `drop`: Remove the commit entirely
- `reorder`: Change the order of commits by reordering lines

### Cherry-Picking

Cherry-picking allows you to apply specific commits from one branch to another:

```bash
# Apply a single commit to current branch
git cherry-pick <commit-hash>

# Apply multiple commits
git cherry-pick <commit-hash-1> <commit-hash-2>

# Cherry-pick without committing
git cherry-pick -n <commit-hash>
```

### Commit Fixup and Autosquash

```bash
# Create a fixup commit
git commit --fixup=<commit-hash>

# Automatically squash fixup commits during rebase
git rebase -i --autosquash <base-commit>
```

## Advanced Branching Techniques

### Orphan Branches

An orphan branch starts with no commit history:

```bash
# Create an orphan branch
git checkout --orphan new-history

# Remove all files from staging
git rm -rf .

# Now you can start with a clean slate
```

This is useful for creating completely separate history trees (e.g., for GitHub Pages).

### Worktrees

Git worktrees allow you to check out multiple branches simultaneously in different directories:

```bash
# Add a worktree for a branch
git worktree add ../path-to-new-dir branch-name

# List worktrees
git worktree list

# Remove a worktree
git worktree remove ../path-to-new-dir
```

## Stashing Advanced Usage

### Stashing Specific Files

```bash
# Stash only specific files
git stash push path/to/file1.txt path/to/file2.txt

# Stash with a message
git stash push -m "Work in progress on feature X"
```

### Stash Management

```bash
# List all stashes
git stash list

# Show the content of a stash
git stash show stash@{1}

# Show the full diff of a stash
git stash show -p stash@{1}

# Apply a specific stash without removing it
git stash apply stash@{1}

# Remove a specific stash
git stash drop stash@{1}

# Create a branch from a stash
git stash branch new-branch stash@{1}
```

## Advanced Merging Techniques

### Merge Strategies

Git offers several merge strategies for different scenarios:

```bash
# Recursive strategy (default)
git merge -s recursive branch-name

# Octopus strategy (for merging multiple branches)
git merge -s octopus branch1 branch2 branch3

# Ours strategy (ignore all changes from other branch)
git merge -s ours branch-name

# Subtree strategy (for merging subtrees)
git merge -s subtree --no-ff branch-name
```

### Merge Options

```bash
# Resolve conflicts favoring our side
git merge -X ours branch-name

# Resolve conflicts favoring their side
git merge -X theirs branch-name

# Ignore whitespace changes
git merge -X ignore-space-change branch-name
```

## Git Hooks

Git hooks are scripts that run automatically when certain Git events occur.

### Common Hook Types

- `pre-commit`: Runs before a commit is created
- `prepare-commit-msg`: Runs before the commit message editor is launched
- `commit-msg`: Runs after the commit message is saved
- `post-commit`: Runs after a commit is created
- `pre-push`: Runs before pushing to a remote
- `pre-receive`: Runs on the remote when receiving a push
- `update`: Runs on the remote for each branch being updated
- `post-receive`: Runs on the remote after a push is completed

### Creating a Simple Hook

1. Navigate to the `.git/hooks` directory in your repository
2. Create a file with the hook name (without extension) or rename the sample
3. Make it executable
4. Write your script

Example `pre-commit` hook to prevent committing large files:

```bash
#!/bin/bash

# Maximum file size in bytes (5MB)
max_size=5242880

# Check all staged files
git diff --staged --name-only | while read file; do
  # Skip if file is deleted
  if [ -f "$file" ]; then
    size=$(stat -c %s "$file" 2>/dev/null || stat -f %z "$file" 2>/dev/null)
    if [ "$size" -gt $max_size ]; then
      echo "Error: $file is larger than 5MB. Please don't commit large files."
      exit 1
    fi
  fi
done
```

Make it executable:
```bash
chmod +x .git/hooks/pre-commit
```

## Submodules and Subtrees

### Git Submodules

Submodules allow you to include other Git repositories within your repository:

```bash
# Add a submodule
git submodule add https://github.com/username/repo.git path/to/submodule

# Initialize submodules after cloning a repository
git submodule init
git submodule update

# Clone a repository with submodules
git clone --recurse-submodules https://github.com/username/repo.git

# Update all submodules
git submodule update --remote

# Execute a command in each submodule
git submodule foreach 'git pull origin main'
```

### Git Subtrees

Subtrees are an alternative to submodules:

```bash
# Add a subtree
git subtree add --prefix=path/to/subtree https://github.com/username/repo.git main --squash

# Update a subtree
git subtree pull --prefix=path/to/subtree https://github.com/username/repo.git main --squash

# Push changes to the subtree's repository
git subtree push --prefix=path/to/subtree https://github.com/username/repo.git main
```

## Advanced Git Configuration

### Aliases

Create shortcuts for common commands:

```bash
# Add an alias
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.ci commit
git config --global alias.st status

# Create complex aliases
git config --global alias.lg "log --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"
```

### Custom Log Formats

```bash
# One line per commit
git config --global alias.l "log --oneline"

# Detailed log
git config --global alias.ll "log --pretty=format:'%C(yellow)%h%Cred%d %Creset%s%Cblue [%cn]' --decorate --numstat"

# Graph view
git config --global alias.lg "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"
```

## Git Bisect for Debugging

Git bisect helps you find which commit introduced a bug:

```bash
# Start bisect
git bisect start

# Mark the current commit as bad
git bisect bad

# Mark a known good commit
git bisect good <commit-hash>

# Git will checkout a commit halfway between good and bad
# Test the code and mark it
git bisect good  # or git bisect bad

# Continue until Git identifies the first bad commit
# When done, reset to your original branch
git bisect reset
```

### Automated Bisect

```bash
# Start bisect with a test script
git bisect start HEAD <good-commit>
git bisect run ./test-script.sh
```

## Git Refspecs

Refspecs map references between remote and local repositories:

```bash
# Fetch a specific branch
git fetch origin master:refs/remotes/origin/master

# Push a local branch to a differently named remote branch
git push origin local-branch:remote-branch

# Delete a remote branch
git push origin :branch-to-delete
```

## Git Filter-Branch and Filter-Repo

These tools allow you to rewrite history extensively:

```bash
# Remove a file from all commits
git filter-branch --force --index-filter 'git rm --cached --ignore-unmatch path/to/file' --prune-empty --tag-name-filter cat -- --all
```

For more complex operations, use the newer `git-filter-repo` tool:

```bash
# Install git-filter-repo
pip install git-filter-repo

# Remove a file from history
git filter-repo --path path/to/file --invert-paths
```

⚠️ **Warning**: These commands rewrite history and should be used with extreme caution, especially on shared repositories.

## Practical Exercise: Advanced Git Workflow

### Exercise: Git Bisect

1. Clone a repository with a known bug
   ```bash
   git clone https://github.com/example/buggy-project.git
   cd buggy-project
   ```

2. Identify a good commit and a bad commit
   ```bash
   # Bad commit (current state)
   git bisect start
   git bisect bad
   
   # Good commit (e.g., a month ago)
   git bisect good $(git rev-list --max-count=1 --before="1 month ago" HEAD)
   ```

3. Test each commit and mark as good or bad
   ```bash
   # After testing
   git bisect good  # or git bisect bad
   ```

4. When the bug-introducing commit is found, reset and fix
   ```bash
   git bisect reset
   git checkout -b fix-bug
   # Make your fix
   git commit -m "Fix bug introduced in commit X"
   ```

### Exercise: Interactive Rebase

1. Create a series of commits
   ```bash
   echo "Line 1" > file.txt
   git add file.txt
   git commit -m "Add line 1"
   
   echo "Line 2" >> file.txt
   git add file.txt
   git commit -m "Add line 2"
   
   echo "Line 3" >> file.txt
   git add file.txt
   git commit -m "Add line 3 with typo"
   
   echo "Line 4" >> file.txt
   git add file.txt
   git commit -m "Add line 4"
   ```

2. Use interactive rebase to clean up history
   ```bash
   git rebase -i HEAD~4
   ```

3. In the editor:
   - Change "pick" to "reword" for the third commit to fix the message
   - Change "pick" to "squash" for the fourth commit to combine it with the third
   - Save and close

4. View the cleaned-up history
   ```bash
   git log --oneline
   ```

## Advanced Git Tips and Tricks

### Recovering Lost Commits

```bash
# View reflog
git reflog

# Recover a commit
git checkout -b recovery-branch <commit-hash>
```

### Finding Bugs with Git Blame

```bash
# See who last modified each line of a file
git blame path/to/file

# Ignore whitespace changes
git blame -w path/to/file

# Show line numbers
git blame -L 10,20 path/to/file
```

### Temporarily Stashing Uncommitted Changes

```bash
# Stash changes and apply immediately after an operation
git stash
git pull
git stash pop
```

### Creating and Applying Patches

```bash
# Create a patch from the last commit
git format-patch -1 HEAD

# Create patches for all commits not in main
git format-patch main

# Apply a patch
git apply patch-file.patch

# Apply a patch as a commit
git am patch-file.patch
```

### Cleaning Up Untracked Files

```bash
# Show what would be deleted
git clean -n

# Delete untracked files
git clean -f

# Delete untracked files and directories
git clean -fd

# Delete ignored files too
git clean -fX
```

### Finding the First Commit

```bash
git rev-list --max-parents=0 HEAD
```

### Viewing Commit Changes

```bash
# Show changes in a commit
git show <commit-hash>

# Show only stats
git show --stat <commit-hash>

# Show changes to a specific file
git show <commit-hash>:path/to/file
```

## Conclusion

These advanced Git techniques will help you manage complex projects and workflows more efficiently. Remember that with great power comes great responsibility—many of these commands can rewrite history and potentially cause data loss if used incorrectly. Always make backups before performing major operations, especially on shared repositories.
