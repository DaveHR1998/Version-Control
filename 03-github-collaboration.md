# GitHub Collaboration Guide

This guide covers how to effectively use GitHub for collaboration on software projects.

## Setting Up GitHub

### Creating a GitHub Account

1. Go to [github.com](https://github.com)
2. Fill out the sign-up form
3. Verify your email address
4. Set up two-factor authentication (recommended)

### Connecting to GitHub with SSH (Recommended)

Using SSH keys allows secure authentication without entering your password each time.

```bash
# Generate SSH key
ssh-keygen -t ed25519 -C "your.email@example.com"

# Start the SSH agent
eval "$(ssh-agent -s)"

# Add your SSH key to the agent
ssh-add ~/.ssh/id_ed25519

# Copy the public key to clipboard (use appropriate command for your OS)
# On Windows:
clip < ~/.ssh/id_ed25519.pub
# On macOS:
pbcopy < ~/.ssh/id_ed25519.pub
# On Linux:
xclip -sel clip < ~/.ssh/id_ed25519.pub
```

Then add the key to your GitHub account:
1. Go to GitHub → Settings → SSH and GPG keys
2. Click "New SSH key"
3. Paste your key and save

### Connecting to GitHub with HTTPS and Credential Manager

If you prefer HTTPS:

```bash
# Set credential helper (Windows)
git config --global credential.helper wincred

# Set credential helper (macOS)
git config --global credential.helper osxkeychain

# Set credential helper (Linux)
git config --global credential.helper cache
```

## Working with GitHub Repositories

### Creating a Repository on GitHub

1. Click "+" in the top right corner
2. Select "New repository"
3. Fill in repository details:
   - Name
   - Description
   - Public/Private
   - Initialize with README (optional)
   - Add .gitignore (optional)
   - Choose license (optional)
4. Click "Create repository"

### Cloning a Repository

```bash
# Clone with HTTPS
git clone https://github.com/username/repository.git

# Clone with SSH
git clone git@github.com:username/repository.git

# Clone to a specific directory
git clone https://github.com/username/repository.git my-project-folder
```

### Connecting an Existing Local Repository

```bash
# Add GitHub as a remote
git remote add origin https://github.com/username/repository.git

# Verify remote
git remote -v

# Push your code to GitHub
git push -u origin main
```

## Collaboration Workflows

### The Fork and Pull Request Model

This is the most common way to contribute to open-source projects:

1. **Fork** the repository on GitHub (click the "Fork" button)
2. **Clone** your fork locally
   ```bash
   git clone https://github.com/your-username/repository.git
   ```
3. **Create a branch** for your feature
   ```bash
   git checkout -b feature-name
   ```
4. **Make changes** and commit them
   ```bash
   git add .
   git commit -m "Add new feature"
   ```
5. **Push** the branch to your fork
   ```bash
   git push origin feature-name
   ```
6. **Create a pull request** on GitHub
   - Go to your fork on GitHub
   - Click "Compare & pull request"
   - Fill in the PR description
   - Submit the PR

### Keeping Your Fork Updated

```bash
# Add the original repository as "upstream"
git remote add upstream https://github.com/original-owner/repository.git

# Fetch updates
git fetch upstream

# Update your main branch
git checkout main
git merge upstream/main

# Push the updates to your fork
git push origin main
```

### Direct Collaboration Model

For repositories where you have write access:

1. **Clone** the repository
   ```bash
   git clone https://github.com/organization/repository.git
   ```
2. **Create a branch**
   ```bash
   git checkout -b feature-name
   ```
3. **Make changes** and commit them
4. **Push** the branch
   ```bash
   git push origin feature-name
   ```
5. **Create a pull request** for code review

## Pull Requests

### Creating a Pull Request

1. Go to the repository on GitHub
2. Click "Pull requests" tab
3. Click "New pull request"
4. Select the base branch and compare branch
5. Click "Create pull request"
6. Fill in title and description
7. Submit the pull request

### Anatomy of a Good Pull Request

- **Clear title**: Summarize the change concisely
- **Detailed description**: Explain what, why, and how
- **References**: Link to related issues or documentation
- **Screenshots/GIFs**: For UI changes
- **Tests**: Mention how the change was tested
- **Checklist**: Include a checklist of completed items

### Reviewing Pull Requests

1. Go to the "Pull requests" tab
2. Select a pull request to review
3. Click "Files changed" to see the diff
4. Add comments on specific lines by clicking the "+" icon
5. Submit your review:
   - Comment: General feedback
   - Approve: Ready to merge
   - Request changes: Needs modifications

### Addressing Review Feedback

```bash
# Make requested changes
git add .
git commit -m "Address review feedback"

# Push the changes to the same branch
git push origin feature-name
```

The pull request will automatically update.

## GitHub Issues

### Creating an Issue

1. Go to the "Issues" tab
2. Click "New issue"
3. Fill in title and description
4. Add labels, assignees, projects, and milestones as needed
5. Submit the issue

### Issue Best Practices

- **Be specific**: Clearly describe the problem or feature request
- **Provide context**: Include environment details, steps to reproduce, etc.
- **Use templates**: Many repositories have issue templates
- **One issue per topic**: Don't combine multiple problems or requests

### Linking Issues and Pull Requests

- Mention an issue in a commit message: `git commit -m "Fix bug #42"`
- Mention an issue in a pull request description: "Fixes #42"
- Use keywords like "closes", "fixes", or "resolves" to automatically close issues when PRs are merged

## GitHub Projects and Project Management

### GitHub Projects

GitHub Projects is a flexible tool for organizing and tracking your work:

1. Go to the "Projects" tab
2. Click "New project"
3. Choose a template or start from scratch
4. Add columns (e.g., To do, In progress, Done)
5. Add issues and pull requests to the project

### Milestones

Milestones group issues and pull requests for a specific goal or timeframe:

1. Go to "Issues" → "Milestones"
2. Click "New milestone"
3. Add title, due date, and description
4. Assign issues and PRs to the milestone

## GitHub Actions (CI/CD)

GitHub Actions allows you to automate workflows directly in your repository:

### Creating a Simple Workflow

1. Create a `.github/workflows` directory in your repository
2. Add a YAML file, e.g., `ci.yml`:

```yaml
name: CI

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  build:
    runs-on: ubuntu-latest
    
    steps:
    - uses: actions/checkout@v2
    
    - name: Set up Node.js
      uses: actions/setup-node@v2
      with:
        node-version: '14'
        
    - name: Install dependencies
      run: npm ci
      
    - name: Run tests
      run: npm test
```

### Common GitHub Actions Use Cases

- Running tests on pull requests
- Building and deploying applications
- Publishing packages
- Code quality checks
- Sending notifications

## Practical Exercise: Collaborative Project

### Exercise: Contribute to a Project

1. Form groups of 2-3 students
2. One student creates a repository and adds others as collaborators
3. Set up a simple web page project with HTML, CSS, and JavaScript
4. Create issues for different features:
   - Header and navigation
   - Main content section
   - Footer
   - Styling
5. Each student:
   - Assigns themselves an issue
   - Creates a branch
   - Implements the feature
   - Creates a pull request
   - Reviews another student's PR
6. Merge all PRs and view the final result

### Exercise: Open Source Contribution Simulation

1. One student creates a "main" repository
2. Other students fork the repository
3. Each student:
   - Creates a branch in their fork
   - Makes a small improvement
   - Submits a pull request to the main repository
4. The repository owner reviews and merges PRs
5. Students sync their forks with the updated main repository

## GitHub Best Practices

1. **Write clear commit messages and PR descriptions**
2. **Use issues to track work** before starting development
3. **Review code thoroughly** before merging
4. **Keep PRs small and focused** on a single change
5. **Use branch protection rules** for important branches
6. **Set up CI/CD** to automate testing and deployment
7. **Document your project** with a good README and contributing guidelines
8. **Use semantic versioning** for releases
9. **Archive old repositories** instead of deleting them
10. **Back up your repositories** regularly
