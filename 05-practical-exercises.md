# Git and GitHub Practical Exercises

This document contains hands-on exercises to help you practice and reinforce your Git and GitHub skills. Complete these exercises in order, as they build upon each other from basic to advanced concepts.

## Exercise 1: Basic Git Workflow

**Objective:** Create a local repository, make changes, and commit them.

### Steps:

1. Create a new directory for your project
   ```bash
   mkdir my-first-git-project
   cd my-first-git-project
   ```

2. Initialize a Git repository
   ```bash
   git init
   ```

3. Create a README file
   ```bash
   echo "# My First Git Project" > README.md
   ```

4. Check the status of your repository
   ```bash
   git status
   ```

5. Add the README file to the staging area
   ```bash
   git add README.md
   ```

6. Commit the changes
   ```bash
   git commit -m "Initial commit: Add README"
   ```

7. Modify the README file
   ```bash
   echo "This is my first Git project. I'm learning Git!" >> README.md
   ```

8. Check the differences
   ```bash
   git diff
   ```

9. Stage and commit the changes
   ```bash
   git add README.md
   git commit -m "Update README with description"
   ```

10. View the commit history
    ```bash
    git log
    ```

## Exercise 2: Working with Branches

**Objective:** Create branches, switch between them, and merge changes.

### Steps:

1. Create a new branch
   ```bash
   git branch feature-1
   ```

2. List all branches
   ```bash
   git branch
   ```

3. Switch to the new branch
   ```bash
   git checkout feature-1
   ```

4. Create a new file in this branch
   ```bash
   echo "# Feature 1" > feature1.md
   echo "This file contains information about Feature 1." >> feature1.md
   ```

5. Stage and commit the changes
   ```bash
   git add feature1.md
   git commit -m "Add Feature 1 documentation"
   ```

6. Switch back to the main branch
   ```bash
   git checkout main
   ```

7. Verify that the feature1.md file is not in the main branch
   ```bash
   ls
   ```

8. Merge the feature branch into main
   ```bash
   git merge feature-1
   ```

9. Verify that the feature1.md file is now in the main branch
   ```bash
   ls
   ```

10. Delete the feature branch since it's been merged
    ```bash
    git branch -d feature-1
    ```

## Exercise 3: Handling Merge Conflicts

**Objective:** Experience and resolve merge conflicts.

### Steps:

1. Create a new branch for a feature
   ```bash
   git checkout -b feature-2
   ```

2. Modify the README.md file in this branch
   ```bash
   echo "## Feature 2" >> README.md
   echo "This project now includes Feature 2." >> README.md
   ```

3. Commit the changes
   ```bash
   git add README.md
   git commit -m "Add Feature 2 information to README"
   ```

4. Switch back to the main branch
   ```bash
   git checkout main
   ```

5. Modify the same part of the README.md file differently
   ```bash
   echo "## Main Branch Update" >> README.md
   echo "This is an update from the main branch." >> README.md
   ```

6. Commit the changes
   ```bash
   git add README.md
   git commit -m "Update README in main branch"
   ```

7. Try to merge the feature branch (this will cause a conflict)
   ```bash
   git merge feature-2
   ```

8. Open README.md in a text editor to resolve the conflict
   - You'll see conflict markers (`<<<<<<<`, `=======`, `>>>>>>>`)
   - Edit the file to keep the content you want
   - Remove the conflict markers

9. Mark the conflict as resolved
   ```bash
   git add README.md
   ```

10. Complete the merge
    ```bash
    git commit
    ```

11. View the commit history with the merge
    ```bash
    git log --graph --oneline
    ```

## Exercise 4: Working with GitHub

**Objective:** Create a GitHub repository and push your local repository to it.

### Prerequisites:
- A GitHub account
- Git configured with your name and email

### Steps:

1. Create a new repository on GitHub
   - Go to github.com and log in
   - Click the "+" icon in the top right and select "New repository"
   - Name it "my-first-git-project"
   - Do not initialize with README, .gitignore, or license
   - Click "Create repository"

2. Connect your local repository to GitHub
   ```bash
   git remote add origin https://github.com/your-username/my-first-git-project.git
   ```

3. Verify the remote connection
   ```bash
   git remote -v
   ```

4. Push your local repository to GitHub
   ```bash
   git push -u origin main
   ```

5. Refresh the GitHub page to see your code online

6. Make a change locally
   ```bash
   echo "## Updated from local" >> README.md
   git add README.md
   git commit -m "Update README from local"
   ```

7. Push the change to GitHub
   ```bash
   git push
   ```

8. View the changes on GitHub

## Exercise 5: Collaboration Simulation

**Objective:** Simulate collaboration by using multiple local directories.

### Steps:

1. Create a "collaborator" directory
   ```bash
   cd ..
   mkdir collaborator
   cd collaborator
   ```

2. Clone your GitHub repository
   ```bash
   git clone https://github.com/your-username/my-first-git-project.git
   cd my-first-git-project
   ```

3. Create a new branch for a feature
   ```bash
   git checkout -b feature-3
   ```

4. Make changes
   ```bash
   echo "# Feature 3" > feature3.md
   echo "This is a new feature added by a collaborator." >> feature3.md
   ```

5. Commit and push the branch
   ```bash
   git add feature3.md
   git commit -m "Add Feature 3"
   git push -u origin feature-3
   ```

6. Go to GitHub and create a pull request
   - Click on "Compare & pull request"
   - Add a description
   - Click "Create pull request"

7. Go back to your original directory
   ```bash
   cd ../../my-first-git-project
   ```

8. Fetch the latest changes
   ```bash
   git fetch
   ```

9. Check out the pull request locally
   ```bash
   git checkout feature-3
   ```

10. Review the changes
    ```bash
    git log -p
    ```

11. Go back to GitHub and merge the pull request
    - Click "Merge pull request"
    - Click "Confirm merge"

12. Update your local main branch
    ```bash
    git checkout main
    git pull
    ```

13. Verify that feature3.md is now in your main branch
    ```bash
    ls
    ```

## Exercise 6: Git History and Time Travel

**Objective:** Learn to navigate and manipulate Git history.

### Steps:

1. View the detailed commit history
   ```bash
   git log --oneline --graph --all
   ```

2. Check out a specific commit (detached HEAD state)
   ```bash
   git checkout <commit-hash>  # Use a hash from your log
   ```

3. Look at the files in this state
   ```bash
   ls
   cat README.md
   ```

4. Return to the main branch
   ```bash
   git checkout main
   ```

5. Create a branch from an older commit
   ```bash
   git checkout -b old-state <commit-hash>  # Use an earlier hash
   ```

6. Make a change in this branch
   ```bash
   echo "# Change from the past" > time-travel.md
   git add time-travel.md
   git commit -m "Add file from historical branch"
   ```

7. Switch back to main and merge this branch
   ```bash
   git checkout main
   git merge old-state
   ```

## Exercise 7: Undoing Changes

**Objective:** Practice different ways to undo changes in Git.

### Steps:

1. Make a change you'll discard
   ```bash
   echo "This will be discarded" > temp.txt
   ```

2. Check the status
   ```bash
   git status
   ```

3. Discard the change
   ```bash
   git restore temp.txt  # For Git 2.23+
   # OR
   git checkout -- temp.txt  # For older Git versions
   ```

4. Make and stage a change
   ```bash
   echo "This will be unstaged" > temp.txt
   git add temp.txt
   ```

5. Unstage the change
   ```bash
   git restore --staged temp.txt  # For Git 2.23+
   # OR
   git reset HEAD temp.txt  # For older Git versions
   ```

6. Make a commit you'll amend
   ```bash
   echo "Initial content" > amend-example.txt
   git add amend-example.txt
   git commit -m "Add file with typo in message"
   ```

7. Amend the commit
   ```bash
   echo "Additional content" >> amend-example.txt
   git add amend-example.txt
   git commit --amend -m "Add file with correct message"
   ```

8. Make a commit you'll revert
   ```bash
   echo "This will be reverted" > revert-example.txt
   git add revert-example.txt
   git commit -m "Add file to be reverted"
   ```

9. Revert the commit
   ```bash
   git revert HEAD
   ```

## Exercise 8: Advanced Branching Strategy

**Objective:** Implement a Git Flow-like branching strategy.

### Steps:

1. Create a develop branch
   ```bash
   git checkout -b develop
   ```

2. Create a feature branch from develop
   ```bash
   git checkout -b feature/user-authentication develop
   ```

3. Make changes in the feature branch
   ```bash
   mkdir src
   echo "function authenticate() { return true; }" > src/auth.js
   ```

4. Commit the changes
   ```bash
   git add src/auth.js
   git commit -m "Implement user authentication"
   ```

5. Merge the feature into develop
   ```bash
   git checkout develop
   git merge --no-ff feature/user-authentication -m "Merge feature: user authentication"
   ```

6. Create a release branch
   ```bash
   git checkout -b release/1.0 develop
   ```

7. Make release preparations
   ```bash
   echo "version = '1.0.0'" > version.txt
   git add version.txt
   git commit -m "Bump version to 1.0.0"
   ```

8. Merge the release to main and develop
   ```bash
   git checkout main
   git merge --no-ff release/1.0 -m "Release 1.0.0"
   git tag -a v1.0.0 -m "Version 1.0.0"
   
   git checkout develop
   git merge --no-ff release/1.0 -m "Merge release 1.0.0 back to develop"
   ```

9. Delete the release branch
   ```bash
   git branch -d release/1.0
   ```

10. View the branch structure
    ```bash
    git log --graph --oneline --all
    ```

## Exercise 9: Git Hooks

**Objective:** Create a simple Git hook to enforce commit message standards.

### Steps:

1. Navigate to the hooks directory
   ```bash
   cd .git/hooks
   ```

2. Create a commit-msg hook
   ```bash
   touch commit-msg
   chmod +x commit-msg
   ```

3. Edit the commit-msg file with a text editor and add:
   ```bash
   #!/bin/sh
   
   commit_msg_file=$1
   commit_msg=$(cat "$commit_msg_file")
   
   # Check if commit message starts with a type (feat, fix, docs, etc.)
   if ! echo "$commit_msg" | grep -qE '^(feat|fix|docs|style|refactor|test|chore)(\(.+\))?: .+'; then
       echo "ERROR: Commit message format must be: type(scope): message"
       echo "Examples: feat(auth): add login feature, fix: correct typo"
       exit 1
   fi
   ```

4. Try making a commit with an invalid message
   ```bash
   cd ../..  # Return to repository root
   echo "test hooks" > hooks-test.txt
   git add hooks-test.txt
   git commit -m "invalid message"
   ```

5. Try making a commit with a valid message
   ```bash
   git commit -m "feat: add hooks test file"
   ```

## Exercise 10: Git Rebase and Interactive Rebase

**Objective:** Practice rebasing and cleaning up commit history.

### Steps:

1. Create a feature branch
   ```bash
   git checkout -b feature/rebase-example
   ```

2. Make several small commits
   ```bash
   echo "Step 1" > rebase-example.txt
   git add rebase-example.txt
   git commit -m "feat: add step 1"
   
   echo "Step 2" >> rebase-example.txt
   git add rebase-example.txt
   git commit -m "feat: add step 2"
   
   echo "Step 3 with typo" >> rebase-example.txt
   git add rebase-example.txt
   git commit -m "feat: add step 3 with typo"
   
   echo "Fix typo in step 3" >> rebase-example.txt
   git add rebase-example.txt
   git commit -m "fix: correct typo in step 3"
   ```

3. Use interactive rebase to clean up history
   ```bash
   git rebase -i HEAD~4
   ```

4. In the editor that opens:
   - Leave the first commit as "pick"
   - Change the second commit to "pick"
   - Change the third commit to "edit" (to fix the typo)
   - Change the fourth commit to "squash" (to combine with the third)
   - Save and close the editor

5. When Git stops at the "edit" commit:
   - Edit the rebase-example.txt file to fix the typo
   - Stage the changes
   ```bash
   git add rebase-example.txt
   ```
   - Continue the rebase
   ```bash
   git rebase --continue
   ```

6. Edit the combined commit message when prompted, then save and close

7. View the cleaned-up history
   ```bash
   git log --oneline
   ```

8. Update the main branch with the rebased history
   ```bash
   git checkout main
   git merge feature/rebase-example
   ```

## Conclusion

Congratulations! You've completed a comprehensive set of Git and GitHub exercises. These exercises have covered:

- Basic Git operations
- Branching and merging
- Resolving conflicts
- Working with GitHub
- Collaboration workflows
- History navigation
- Undoing changes
- Advanced branching strategies
- Git hooks
- Rebasing and history cleanup

Continue practicing these skills on your own projects to become proficient with Git and GitHub.
