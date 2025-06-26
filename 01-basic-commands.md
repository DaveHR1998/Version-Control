# Basic Git Commands with Examples

This guide provides detailed examples of basic Git commands with explanations of what's happening behind the scenes.

## Setting Up Git

### Checking Git Version
```bash
git --version
```
This shows which version of Git you have installed. It's useful to know when following tutorials that might reference specific Git features.

### Configuring Git
```bash
# Global configuration (applies to all repositories)
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"

# Repository-specific configuration (overrides global)
git config user.name "Project Specific Name"
git config user.email "project.specific@example.com"
```

Git needs to know who you are when making commits. This information is embedded in every commit you make.

## Creating and Initializing Repositories

### Creating a New Repository
```bash
mkdir my-project
cd my-project
git init
```

When you run `git init`:
- Git creates a hidden `.git` directory
- This directory contains all the metadata and object database for your repository
- Your project is now under version control

### Checking Repository Status
```bash
git status
```

This command shows:
- Which branch you're on
- Which files are tracked/untracked
- Which files have changes that are staged or unstaged

## Tracking Changes

### Adding Files to Staging Area
```bash
# Add a specific file
git add file.txt

# Add multiple files
git add file1.txt file2.txt

# Add all files in current directory
git add .

# Add all files matching a pattern
git add *.txt
```

The staging area (or index) is a middle ground between your working directory and the repository. It's where you prepare changes for a commit.

### Removing Files from Staging Area
```bash
git restore --staged file.txt
```

This unstages a file without discarding its changes.

### Committing Changes
```bash
# Basic commit
git commit -m "Add initial files"

# Detailed commit with title and description
git commit -m "Add user authentication feature" -m "This implements login, registration, and password reset functionality using JWT tokens."
```

A commit:
- Creates a snapshot of your staged changes
- Assigns a unique identifier (hash) to the snapshot
- Records author, date, and commit message
- Points to the previous commit, forming a chain

### Viewing Commit History
```bash
# Basic log
git log

# Compact log
git log --oneline

# Graphical log
git log --graph --oneline --decorate

# Log with file changes
git log --stat

# Log for a specific file
git log -- file.txt
```

## Working with Remote Repositories

### Adding a Remote Repository
```bash
git remote add origin https://github.com/username/repository.git
```

This creates a connection named "origin" pointing to the GitHub repository URL.

### Viewing Remote Repositories
```bash
# List remotes
git remote -v

# Get details about a specific remote
git remote show origin
```

### Fetching from Remote
```bash
# Fetch from default remote (origin)
git fetch

# Fetch from specific remote
git fetch upstream
```

Fetching downloads new data from a remote repository but doesn't integrate it into your working files.

### Pulling from Remote
```bash
git pull origin main
```

Pulling is essentially a `git fetch` followed by a `git merge`. It downloads changes and immediately updates your current branch.

### Pushing to Remote
```bash
# First push with upstream tracking
git push -u origin main

# Subsequent pushes
git push
```

The `-u` flag sets up tracking, which means your local branch knows which remote branch to push to and pull from.

## Practical Exercise: First Repository

1. Create a new directory for your project
   ```bash
   mkdir hello-git
   cd hello-git
   ```

2. Initialize a Git repository
   ```bash
   git init
   ```

3. Create a simple HTML file
   ```bash
   echo "<!DOCTYPE html>
   <html>
   <head>
       <title>Hello Git</title>
   </head>
   <body>
       <h1>Hello, Git!</h1>
       <p>This is my first Git repository.</p>
   </body>
   </html>" > index.html
   ```

4. Check the status
   ```bash
   git status
   ```

5. Add the file to staging
   ```bash
   git add index.html
   ```

6. Commit the file
   ```bash
   git commit -m "Add initial HTML file"
   ```

7. Make a change to the file
   ```bash
   echo "
       <footer>
           <p>Created with Git</p>
       </footer>" >> index.html
   ```

8. Check the difference
   ```bash
   git diff
   ```

9. Stage and commit the change
   ```bash
   git add index.html
   git commit -m "Add footer to HTML file"
   ```

10. View the commit history
    ```bash
    git log --oneline
    ```

This exercise demonstrates the basic Git workflow: make changes, stage them, commit them, and view the history.
