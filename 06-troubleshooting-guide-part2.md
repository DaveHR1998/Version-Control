# Git and GitHub Troubleshooting Guide - Part 2

## Remote Repository Issues

### Remote Repository Not Found

**Problem:** `git push` or `git pull` fails with "repository not found" error

**Solution:**
1. Check if the remote exists:
   ```bash
   git remote -v
   ```

2. Verify the URL is correct:
   - For HTTPS: `https://github.com/username/repository.git`
   - For SSH: `git@github.com:username/repository.git`

3. If incorrect, update it:
   ```bash
   git remote set-url origin https://github.com/username/repository.git
   ```

4. Check if you have access to the repository:
   - Ensure you're logged in to GitHub
   - Verify you have the necessary permissions
   - For private repositories, ensure you're using the correct authentication

### "Remote Already Exists" Error

**Problem:** `git remote add origin` fails with "remote origin already exists" error

**Solution:**
1. Check current remotes:
   ```bash
   git remote -v
   ```

2. Either update the existing remote:
   ```bash
   git remote set-url origin https://github.com/username/repository.git
   ```

3. Or remove it and add again:
   ```bash
   git remote remove origin
   git remote add origin https://github.com/username/repository.git
   ```

### Authentication Issues

**Problem:** Git asks for username and password repeatedly or authentication fails

**Solution:**
1. For HTTPS URLs:
   - Use a credential helper:
     ```bash
     # Windows
     git config --global credential.helper wincred
     
     # macOS
     git config --global credential.helper osxkeychain
     
     # Linux
     git config --global credential.helper cache
     ```

2. If using GitHub with 2FA:
   - Generate a personal access token on GitHub
   - Use the token as your password

3. Switch to SSH authentication:
   ```bash
   # Change remote URL from HTTPS to SSH
   git remote set-url origin git@github.com:username/repository.git
   ```

### Push Rejection

**Problem:** `git push` is rejected with "failed to push some refs" error

**Solution:**
1. Pull the latest changes first:
   ```bash
   git pull origin main
   ```

2. If there are conflicts, resolve them:
   - Edit conflicted files
   - `git add .`
   - `git commit`

3. If you want to force push (use with caution!):
   ```bash
   git push -f origin main
   ```
   ⚠️ Warning: This overwrites remote changes and can cause data loss!

4. If branches have diverged completely:
   ```bash
   # Create a merge commit
   git pull --no-rebase
   
   # Or rebase your changes
   git pull --rebase
   ```

## Pull and Fetch Issues

### "Cannot Pull with Rebase: You Have Unstaged Changes"

**Problem:** Git won't let you pull with rebase because you have local changes

**Solution:**
1. Stash your changes:
   ```bash
   git stash
   git pull --rebase
   git stash pop
   ```

2. Or commit your changes:
   ```bash
   git commit -m "WIP"
   git pull --rebase
   ```

### "Your Local Changes Would Be Overwritten"

**Problem:** Git refuses to pull because local changes would be overwritten

**Solution:**
1. Stash your changes:
   ```bash
   git stash
   git pull
   git stash pop
   ```

2. If you want to discard your changes:
   ```bash
   git reset --hard
   git pull
   ```

3. If you want to keep your changes and the remote changes:
   ```bash
   # Save your changes
   git add .
   git commit -m "My local changes"
   
   # Pull with merge
   git pull
   ```

### Fetch Doesn't Update Working Directory

**Problem:** After `git fetch`, you don't see the changes in your files

**Solution:**
- This is normal behavior. `git fetch` only downloads changes but doesn't apply them.
- To see the changes:
  ```bash
  # View differences
  git diff origin/main
  
  # Apply changes
  git merge origin/main
  # Or
  git rebase origin/main
  ```

## Rebase Issues

### Rebase Conflicts

**Problem:** Conflicts occur during `git rebase`

**Solution:**
1. Resolve conflicts in each file:
   - Edit files to fix conflicts
   - `git add .`
   - `git rebase --continue`

2. To abort the rebase:
   ```bash
   git rebase --abort
   ```

3. For complex rebases, consider using a merge instead:
   ```bash
   git rebase --abort
   git merge branch-name
   ```

### "Cannot Rebase: You Have Unstaged Changes"

**Problem:** Git won't let you rebase because you have local changes

**Solution:**
1. Stash your changes:
   ```bash
   git stash
   git rebase origin/main
   git stash pop
   ```

2. Or commit your changes:
   ```bash
   git commit -m "WIP"
   git rebase origin/main
   ```

### Lost Commits After Rebase

**Problem:** You can't find commits after a rebase operation

**Solution:**
1. Check the reflog to find lost commits:
   ```bash
   git reflog
   ```

2. Recover the lost commit:
   ```bash
   # Create a new branch at the lost commit
   git checkout -b recovery-branch <commit-hash>
   
   # Or reset your current branch to it
   git reset --hard <commit-hash>
   ```

## GitHub-Specific Issues

### Cannot Create Pull Request

**Problem:** Unable to create a pull request on GitHub

**Solution:**
1. Ensure your branch is pushed to GitHub:
   ```bash
   git push -u origin your-branch-name
   ```

2. Check if the branch already has an open PR

3. Verify you have permission to create PRs in the repository

4. If the base repository is a fork, make sure you're creating the PR to the correct upstream repository

### Pull Request Shows Too Many Commits

**Problem:** Your PR includes commits that shouldn't be there

**Solution:**
1. Identify which commits should be in the PR:
   ```bash
   git log base-branch..your-branch
   ```

2. Create a new branch from the base branch:
   ```bash
   git checkout base-branch
   git checkout -b clean-pr-branch
   ```

3. Cherry-pick only the relevant commits:
   ```bash
   git cherry-pick <commit-hash-1> <commit-hash-2>
   ```

4. Force push to update your PR branch:
   ```bash
   git push -f origin clean-pr-branch:your-branch-name
   ```

### GitHub Actions Workflow Failures

**Problem:** GitHub Actions CI/CD workflows fail

**Solution:**
1. Check the workflow logs on GitHub:
   - Go to repository → Actions → Failed workflow → Job details

2. Common issues:
   - Missing dependencies: Update your workflow YAML to install required packages
   - Failed tests: Fix the failing tests locally, then push again
   - Environment issues: Ensure your workflow uses the correct environment variables

3. Test locally before pushing:
   - Run tests locally: `npm test`, `pytest`, etc.
   - Use pre-commit hooks to catch issues early

## Advanced Issues

### Git Performance Problems

**Problem:** Git operations are slow, especially in large repositories

**Solution:**
1. Clean up unnecessary files:
   ```bash
   git gc
   ```

2. For very large repositories:
   ```bash
   git gc --aggressive
   ```

3. Use shallow clones for large repositories:
   ```bash
   git clone --depth 1 https://github.com/username/repository.git
   ```

4. Consider using Git LFS for large files:
   ```bash
   git lfs install
   git lfs track "*.psd"
   ```

### Detached HEAD State

**Problem:** You're in "detached HEAD state" and unsure what to do

**Solution:**
1. If you want to keep your changes:
   ```bash
   # Create a new branch at current position
   git checkout -b new-branch-name
   ```

2. If you want to return to a branch:
   ```bash
   # First, note the commit hash if needed
   git log -1
   
   # Then checkout a branch
   git checkout main
   ```

3. If you made commits in detached HEAD state that you want to keep:
   ```bash
   # Note the commit hash
   git log -1
   
   # Checkout a branch and cherry-pick
   git checkout main
   git cherry-pick <commit-hash>
   ```

### Accidentally Committed Sensitive Information

**Problem:** You committed sensitive data (passwords, API keys, etc.)

**Solution:**
1. Remove the sensitive data from your latest commit:
   ```bash
   # Edit the file to remove sensitive data
   git add .
   git commit --amend
   ```

2. If it's in an older commit, use BFG Repo-Cleaner or git-filter-repo:
   ```bash
   # Using git-filter-repo
   pip install git-filter-repo
   git filter-repo --replace-text passwords.txt
   ```

3. Force push to update the repository:
   ```bash
   git push -f
   ```

4. Important follow-up steps:
   - Revoke and replace any exposed credentials
   - Add sensitive files to .gitignore
   - Consider using environment variables or a secure vault

### Large Files Causing Problems

**Problem:** Repository is bloated with large files

**Solution:**
1. Use Git LFS (Large File Storage):
   ```bash
   # Install Git LFS
   git lfs install
   
   # Track large file types
   git lfs track "*.psd" "*.zip" "*.mp4"
   
   # Add .gitattributes
   git add .gitattributes
   git commit -m "Configure Git LFS"
   ```

2. For existing large files:
   ```bash
   # Find large files
   git rev-list --objects --all | grep -f <(git verify-pack -v .git/objects/pack/*.idx | sort -k 3 -n | tail -10 | awk '{print $1}')
   
   # Remove them from history (use with caution!)
   git filter-repo --strip-blobs-bigger-than 10M
   ```

## Recovery Scenarios

### Recovering Deleted Branches

**Problem:** You accidentally deleted a branch

**Solution:**
1. Find the last commit on the branch:
   ```bash
   git reflog
   ```

2. Create a new branch at that commit:
   ```bash
   git checkout -b recovered-branch <commit-hash>
   ```

### Recovering Uncommitted Changes

**Problem:** You lost uncommitted changes (e.g., after a hard reset)

**Solution:**
1. Check if the changes are stashed:
   ```bash
   git stash list
   git stash apply
   ```

2. If you did a reset, check the reflog:
   ```bash
   git reflog
   git checkout <commit-hash>
   ```

3. If using an IDE, check for local history/backups

### Recovering After a Bad Merge or Rebase

**Problem:** A merge or rebase went wrong and you want to start over

**Solution:**
1. If you haven't pushed:
   ```bash
   # Find the commit before the operation
   git reflog
   
   # Reset to that commit
   git reset --hard <commit-hash>
   ```

2. If you've pushed and others might have pulled:
   ```bash
   # Create a revert commit
   git revert <bad-commit-hash>
   # Or for a merge
   git revert -m 1 <merge-commit-hash>
   ```

## Preventative Measures

### Best Practices to Avoid Common Issues

1. **Pull before pushing**:
   ```bash
   git pull origin main
   ```

2. **Create branches for new features**:
   ```bash
   git checkout -b feature/new-feature
   ```

3. **Commit often with clear messages**:
   ```bash
   git commit -m "Fix: Resolve null pointer in user authentication"
   ```

4. **Use .gitignore properly**:
   ```bash
   # Add common exclusions
   echo "node_modules/" >> .gitignore
   echo "*.log" >> .gitignore
   echo ".env" >> .gitignore
   ```

5. **Set up pre-commit hooks** for linting and testing

6. **Backup important branches**:
   ```bash
   git tag backup-main main
   ```

7. **Review changes before committing**:
   ```bash
   git diff
   git add -p
   ```

8. **Use meaningful branch names**:
   - `feature/user-authentication`
   - `bugfix/login-error`
   - `hotfix/security-vulnerability`

Remember, most Git problems can be solved without losing work. Git is designed to preserve your data, even when commands seem destructive. When in doubt, make a backup of your repository before attempting complex operations.
