# Git and GitHub Troubleshooting Guide - Part 1

This guide addresses common issues you might encounter when using Git and GitHub, along with their solutions.

## Installation and Setup Issues

### Git Not Recognized as a Command

**Problem:** When typing `git` commands, you get "git is not recognized as an internal or external command"

**Solution:**
1. Verify Git is installed:
   - Windows: Check in Add/Remove Programs
   - macOS: Run `which git`
   - Linux: Run `which git`

2. If not installed, install Git:
   - Windows: Download from [git-scm.com](https://git-scm.com/download/win)
   - macOS: `brew install git` or download from [git-scm.com](https://git-scm.com/download/mac)
   - Linux: `sudo apt-get install git` (Ubuntu/Debian)

3. If installed but not recognized:
   - Restart your terminal/command prompt
   - Add Git to your PATH environment variable:
     - Windows: During installation, select "Use Git from the Windows Command Prompt"
     - macOS/Linux: Add `export PATH=$PATH:/path/to/git/bin` to your `.bash_profile` or `.zshrc`

### Git Config Issues

**Problem:** Git doesn't remember your username or email

**Solution:**
1. Check your current configuration:
   ```bash
   git config --list
   ```

2. Set your name and email:
   ```bash
   git config --global user.name "Your Name"
   git config --global user.email "your.email@example.com"
   ```

3. If you need different settings for a specific repository:
   ```bash
   cd /path/to/repo
   git config user.name "Project Name"
   git config user.email "project.email@example.com"
   ```

### SSH Key Problems

**Problem:** Unable to connect to GitHub via SSH

**Solution:**
1. Verify your SSH key is set up:
   ```bash
   ls -la ~/.ssh
   ```

2. If no key exists, create one:
   ```bash
   ssh-keygen -t ed25519 -C "your.email@example.com"
   ```

3. Add the key to your SSH agent:
   ```bash
   eval "$(ssh-agent -s)"
   ssh-add ~/.ssh/id_ed25519
   ```

4. Copy the public key and add it to GitHub:
   ```bash
   cat ~/.ssh/id_ed25519.pub
   ```
   - Go to GitHub → Settings → SSH and GPG keys → New SSH key
   - Paste your key and save

5. Test the connection:
   ```bash
   ssh -T git@github.com
   ```

## Repository Issues

### Cannot Initialize Repository

**Problem:** `git init` returns an error or doesn't create a .git directory

**Solution:**
1. Check permissions:
   ```bash
   # Linux/macOS
   ls -la
   
   # Windows
   dir /a
   ```

2. Ensure you have write permissions to the directory

3. Try with administrator/sudo privileges:
   ```bash
   # Windows: Run CMD as Administrator
   # macOS/Linux
   sudo git init
   ```

### Repository Corruption

**Problem:** Git reports "corrupt loose object" or "bad object" errors

**Solution:**
1. Try to repair the repository:
   ```bash
   git fsck --full
   ```

2. If that doesn't work, try:
   ```bash
   git gc --aggressive
   ```

3. If still corrupted, clone a fresh copy (if available):
   ```bash
   cd ..
   git clone https://github.com/username/repository.git new-repo
   ```

4. As a last resort, backup your files (not the .git directory) and reinitialize:
   ```bash
   # Backup files
   cp -r repo-files backup/
   
   # Reinitialize
   rm -rf .git
   git init
   git add .
   git commit -m "Reinitialize repository"
   ```

## Commit Issues

### Unable to Commit

**Problem:** `git commit` fails with errors

**Solution:**
1. Check if you have staged files:
   ```bash
   git status
   ```

2. If no files are staged, add them:
   ```bash
   git add .
   ```

3. If Git asks for user identity:
   ```bash
   git config --global user.name "Your Name"
   git config --global user.email "your.email@example.com"
   ```

4. If you get a "hooks failed" error, check your pre-commit hooks:
   ```bash
   cat .git/hooks/pre-commit
   ```
   - Temporarily bypass hooks: `git commit --no-verify -m "Your message"`

### Amending the Wrong Commit

**Problem:** You used `git commit --amend` but realized it was the wrong commit

**Solution:**
1. If you haven't pushed, reset to the previous state:
   ```bash
   git reset --soft HEAD@{1}
   ```

2. If you've already pushed, create a new commit that reverts the changes:
   ```bash
   git revert HEAD
   ```

### Commit to the Wrong Branch

**Problem:** You committed changes to the wrong branch

**Solution:**
1. If you haven't pushed:
   ```bash
   # Note your commit hash
   git log -1
   
   # Switch to the correct branch
   git checkout correct-branch
   
   # Apply the commit
   git cherry-pick <commit-hash>
   
   # Go back and remove from wrong branch
   git checkout wrong-branch
   git reset --hard HEAD~1
   ```

2. If you've already pushed:
   ```bash
   # Cherry-pick to correct branch
   git checkout correct-branch
   git cherry-pick <commit-hash>
   
   # Create a revert commit on the wrong branch
   git checkout wrong-branch
   git revert <commit-hash>
   ```

## Branching and Merging Issues

### Cannot Create Branch

**Problem:** `git branch` or `git checkout -b` fails

**Solution:**
1. Check if the branch already exists:
   ```bash
   git branch -a
   ```

2. If you have uncommitted changes, either:
   - Commit them: `git commit -m "WIP"`
   - Stash them: `git stash`

3. If the branch name contains special characters, use quotes:
   ```bash
   git checkout -b "branch/with/slashes"
   ```

### Cannot Delete Branch

**Problem:** `git branch -d` fails with "not fully merged" error

**Solution:**
1. If you're sure you want to delete it:
   ```bash
   git branch -D branch-name
   ```

2. If you want to merge it first:
   ```bash
   git checkout main
   git merge branch-name
   git branch -d branch-name
   ```

3. If you want to delete a remote branch:
   ```bash
   git push origin --delete branch-name
   ```

### Merge Conflicts

**Problem:** Git reports merge conflicts during `git merge` or `git pull`

**Solution:**
1. Identify conflicted files:
   ```bash
   git status
   ```

2. Open each conflicted file and look for conflict markers (`<<<<<<<`, `=======`, `>>>>>>>`)

3. Edit the files to resolve conflicts:
   - Keep your changes: Remove their changes and conflict markers
   - Keep their changes: Remove your changes and conflict markers
   - Combine changes: Edit as needed and remove conflict markers

4. After resolving, stage the files:
   ```bash
   git add .
   ```

5. Complete the merge:
   ```bash
   git commit
   ```
   - Git will provide a default merge commit message

6. If you want to abort the merge:
   ```bash
   git merge --abort
   ```

### Accidentally Merged the Wrong Branch

**Problem:** You merged the wrong branch into your current branch

**Solution:**
1. If you haven't pushed:
   ```bash
   # Find the commit before the merge
   git log
   
   # Reset to that commit
   git reset --hard <commit-hash>
   ```

2. If you've already pushed:
   ```bash
   # Create a revert commit for the merge
   git revert -m 1 <merge-commit-hash>
   ```
