# GitHub - Comprehensive Cheat Sheet

## 📋 Table of Contents
- [GitHub Basics](#github-basics)
- [Repository Management](#repository-management)
- [Collaboration Workflow](#collaboration-workflow)
- [Pull Requests](#pull-requests)
- [Issues and Project Management](#issues-and-project-management)
- [GitHub Actions (CI/CD)](#github-actions-cicd)
- [GitHub Pages](#github-pages)
- [Security Features](#security-features)
- [GitHub CLI](#github-cli)
- [Advanced Features](#advanced-features)
- [Best Practices](#best-practices)

---

## GitHub Basics

### What is GitHub?

**GitHub** is a web-based platform built on top of Git that provides:
- 🏢 **Repository hosting** - Store and manage your Git repositories
- 👥 **Collaboration tools** - Work together with teams
- 🔍 **Code review** - Review and discuss code changes
- 🤖 **Automation** - CI/CD with GitHub Actions
- 📊 **Project management** - Issues, Projects, and more
- 🌐 **Community** - Open source collaboration

### GitHub vs Git

| Feature | Git | GitHub |
|---------|-----|--------|
| Type | Version control system | Hosting platform |
| Location | Local computer | Cloud (web-based) |
| Purpose | Track changes | Collaboration & hosting |
| Requires | Command line | Web browser or CLI |
| Collaboration | Limited | Built-in tools |

### Account Setup

```bash
# Create account at https://github.com

# Configure Git with GitHub credentials
git config --global user.name "Your Name"
git config --global user.email "your-email@example.com"

# Verify configuration
git config --list
```

### SSH Key Setup

```bash
# Generate SSH key
ssh-keygen -t ed25519 -C "your-email@example.com"

# Start SSH agent
eval "$(ssh-agent -s)"

# Add SSH key to agent
ssh-add ~/.ssh/id_ed25519

# Copy public key to clipboard (Windows)
cat ~/.ssh/id_ed25519.pub | clip

# Copy public key to clipboard (Mac)
pbcopy < ~/.ssh/id_ed25519.pub

# Copy public key to clipboard (Linux)
cat ~/.ssh/id_ed25519.pub | xclip -selection clipboard

# Add key to GitHub: Settings → SSH and GPG keys → New SSH key

# Test connection
ssh -T git@github.com
```

### Personal Access Tokens (PAT)

```bash
# Create PAT: Settings → Developer settings → Personal access tokens → Generate new token

# Use PAT for HTTPS authentication
git clone https://github.com/username/repo.git
# Username: your-username
# Password: your-PAT-token

# Store credentials (avoid repeated login)
git config --global credential.helper cache
git config --global credential.helper 'cache --timeout=3600'

# Or store permanently (less secure)
git config --global credential.helper store
```

---

## Repository Management

### Creating Repositories

#### On GitHub Web

1. Click **"New"** or **"+"** → **"New repository"**
2. Fill in:
   - Repository name
   - Description (optional)
   - Public/Private
   - Initialize with README
   - Add .gitignore
   - Choose license
3. Click **"Create repository"**

#### From Command Line

```bash
# Create local repository
mkdir my-project
cd my-project
git init

# Create first commit
echo "# My Project" > README.md
git add README.md
git commit -m "Initial commit"

# Create repository on GitHub (using GitHub CLI)
gh repo create my-project --public --source=. --remote=origin --push

# Or manually add remote
git remote add origin https://github.com/username/my-project.git
git branch -M main
git push -u origin main
```

### Cloning Repositories

```bash
# Clone with HTTPS
git clone https://github.com/username/repo.git

# Clone with SSH
git clone git@github.com:username/repo.git

# Clone to specific folder
git clone https://github.com/username/repo.git my-folder

# Clone specific branch
git clone -b develop https://github.com/username/repo.git

# Shallow clone (faster, less history)
git clone --depth 1 https://github.com/username/repo.git

# Clone with submodules
git clone --recurse-submodules https://github.com/username/repo.git
```

### Forking Repositories

```bash
# Fork on GitHub Web: Click "Fork" button

# Clone your fork
git clone https://github.com/YOUR-USERNAME/forked-repo.git
cd forked-repo

# Add upstream remote (original repo)
git remote add upstream https://github.com/ORIGINAL-OWNER/original-repo.git

# Verify remotes
git remote -v
# origin    https://github.com/YOUR-USERNAME/forked-repo.git (fetch)
# origin    https://github.com/YOUR-USERNAME/forked-repo.git (push)
# upstream  https://github.com/ORIGINAL-OWNER/original-repo.git (fetch)
# upstream  https://github.com/ORIGINAL-OWNER/original-repo.git (push)

# Sync fork with upstream
git fetch upstream
git checkout main
git merge upstream/main
git push origin main
```

### Repository Settings

#### Visibility

```bash
# Change via GitHub Web:
# Settings → General → Danger Zone → Change repository visibility

# Delete repository:
# Settings → General → Danger Zone → Delete this repository
```

#### Collaborators

```bash
# Add collaborators:
# Settings → Collaborators → Add people

# Set permissions:
# - Read: View and clone
# - Triage: Read + manage issues and PRs
# - Write: Read + push to repository
# - Maintain: Write + manage repository settings
# - Admin: Full access
```

#### Branch Protection

```bash
# Settings → Branches → Add rule

# Common rules:
# ✓ Require pull request reviews before merging
# ✓ Require status checks to pass before merging
# ✓ Require branches to be up to date before merging
# ✓ Include administrators
# ✓ Restrict who can push to matching branches
```

---

## Collaboration Workflow

### Standard Git + GitHub Workflow

```bash
# 1. Clone repository
git clone https://github.com/username/project.git
cd project

# 2. Create feature branch
git checkout -b feature/new-feature

# 3. Make changes
# ... edit files ...

# 4. Commit changes
git add .
git commit -m "feat: add new feature"

# 5. Push to GitHub
git push -u origin feature/new-feature

# 6. Create Pull Request on GitHub

# 7. After PR is merged, update local main
git checkout main
git pull origin main

# 8. Delete feature branch
git branch -d feature/new-feature
git push origin --delete feature/new-feature
```

### Keeping Fork Updated

```bash
# Fetch upstream changes
git fetch upstream

# Merge upstream changes to local main
git checkout main
git merge upstream/main

# Or rebase
git rebase upstream/main

# Push to your fork
git push origin main

# Update your feature branch
git checkout feature-branch
git rebase main
git push -f origin feature-branch
```

### Syncing with Remote

```bash
# Fetch all changes
git fetch origin

# View remote branches
git branch -r

# Checkout remote branch
git checkout -b feature origin/feature

# Pull latest changes
git pull origin main

# Pull with rebase
git pull --rebase origin main

# Force pull (overwrite local)
git fetch origin
git reset --hard origin/main
```

---

## Pull Requests

### Creating Pull Requests

#### Via GitHub Web

1. Push branch to GitHub
2. Navigate to repository
3. Click **"Compare & pull request"**
4. Fill in:
   - Title (descriptive)
   - Description (what, why, how)
   - Reviewers
   - Assignees
   - Labels
   - Projects
   - Milestone
5. Click **"Create pull request"**

#### Via GitHub CLI

```bash
# Create PR
gh pr create --title "Add new feature" --body "Description of changes"

# Create PR interactively
gh pr create

# Create draft PR
gh pr create --draft

# Create PR to specific branch
gh pr create --base develop --head feature-branch

# Create PR and auto-merge when checks pass
gh pr create --auto-merge
```

### PR Description Template

```markdown
## Description
Brief description of changes

## Type of Change
- [ ] Bug fix
- [ ] New feature
- [ ] Breaking change
- [ ] Documentation update

## How Has This Been Tested?
- [ ] Unit tests
- [ ] Integration tests
- [ ] Manual testing

## Checklist
- [ ] Code follows style guidelines
- [ ] Self-review completed
- [ ] Comments added for complex code
- [ ] Documentation updated
- [ ] No new warnings generated
- [ ] Tests added/updated
- [ ] All tests pass

## Screenshots (if applicable)

## Related Issues
Closes #123
Fixes #456
```

### Reviewing Pull Requests

```bash
# View PRs
gh pr list

# View specific PR
gh pr view 123

# Checkout PR locally
gh pr checkout 123

# Review PR
gh pr review 123

# Approve PR
gh pr review 123 --approve

# Request changes
gh pr review 123 --request-changes --body "Please fix..."

# Comment on PR
gh pr review 123 --comment --body "Looks good!"

# Add inline comment
# Do this on GitHub web interface
```

### Managing Pull Requests

```bash
# Merge PR (via CLI)
gh pr merge 123

# Merge with squash
gh pr merge 123 --squash

# Merge with rebase
gh pr merge 123 --rebase

# Merge and delete branch
gh pr merge 123 --delete-branch

# Close PR without merging
gh pr close 123

# Reopen PR
gh pr reopen 123

# View PR status
gh pr status

# View PR checks
gh pr checks 123
```

### PR Best Practices

```bash
# 1. Keep PRs small and focused
# One feature/fix per PR

# 2. Write descriptive titles
# ✓ "feat: add user authentication with JWT"
# ✗ "Update code"

# 3. Provide context in description
# - What changed
# - Why it changed
# - How to test

# 4. Link related issues
# Closes #123

# 5. Request specific reviewers
# People familiar with the code

# 6. Respond to feedback promptly
# Address comments and concerns

# 7. Keep PR updated
git fetch origin main
git rebase origin/main
git push -f origin feature-branch

# 8. Use draft PRs for work in progress
gh pr create --draft
```

---

## Issues and Project Management

### Creating Issues

#### Via GitHub Web

1. Go to **"Issues"** tab
2. Click **"New issue"**
3. Fill in:
   - Title
   - Description
   - Assignees
   - Labels
   - Projects
   - Milestone
4. Click **"Submit new issue"**

#### Via GitHub CLI

```bash
# Create issue
gh issue create --title "Bug: Login fails" --body "Description"

# Create issue interactively
gh issue create

# Create with labels
gh issue create --title "Feature request" --label "enhancement"

# Create with assignee
gh issue create --title "Fix bug" --assignee username
```

### Issue Templates

Create `.github/ISSUE_TEMPLATE/bug_report.md`:

```markdown
---
name: Bug Report
about: Report a bug
title: '[BUG] '
labels: bug
assignees: ''
---

## Describe the bug
A clear description of the bug.

## To Reproduce
Steps to reproduce:
1. Go to '...'
2. Click on '...'
3. See error

## Expected behavior
What should happen.

## Screenshots
If applicable.

## Environment
- OS: [e.g. Windows 10]
- Browser: [e.g. Chrome 90]
- Version: [e.g. 1.0.0]

## Additional context
Any other information.
```

### Managing Issues

```bash
# List issues
gh issue list

# Filter issues
gh issue list --label bug
gh issue list --assignee username
gh issue list --state closed

# View issue
gh issue view 123

# Close issue
gh issue close 123

# Reopen issue
gh issue reopen 123

# Comment on issue
gh issue comment 123 --body "Working on this"

# Edit issue
gh issue edit 123 --title "New title"

# Add labels
gh issue edit 123 --add-label "bug,priority"

# Assign issue
gh issue edit 123 --assignee username
```

### Labels

```bash
# Common labels:
bug             - Something isn't working
enhancement     - New feature request
documentation   - Documentation improvements
duplicate       - Duplicate issue
help wanted     - Need help from community
good first issue - Good for newcomers
invalid         - Not valid issue
wontfix         - Won't be fixed
question        - Question about project

# Create custom label (GitHub Web)
# Issues → Labels → New label

# Via GitHub CLI
gh label create "priority:high" --description "High priority" --color "FF0000"
```

### Milestones

```bash
# Create milestone (GitHub Web)
# Issues → Milestones → New milestone

# Via GitHub CLI
gh api repos/:owner/:repo/milestones -f title="v1.0.0" -f due_on="2024-12-31T23:59:59Z"

# Assign issue to milestone
gh issue edit 123 --milestone "v1.0.0"
```

### GitHub Projects

```bash
# Create project (GitHub Web)
# Projects → New project → Choose template

# Project views:
# - Board (Kanban)
# - Table (Spreadsheet)
# - Roadmap (Timeline)

# Add issues to project
# Drag and drop or use automation

# Automate project
# Settings → Workflows
# - Auto-add new issues
# - Auto-move based on status
# - Auto-close completed items
```

---

## GitHub Actions (CI/CD)

### What are GitHub Actions?

**GitHub Actions** is a CI/CD platform that allows you to automate workflows:
- 🧪 **Testing** - Run tests automatically
- 🏗️ **Building** - Build and compile code
- 📦 **Deployment** - Deploy to servers
- 🔍 **Code quality** - Linting, formatting
- 🤖 **Automation** - Any custom workflow

### Basic Workflow

Create `.github/workflows/ci.yml`:

```yaml
name: CI

on:
  push:
    branches: [ main, develop ]
  pull_request:
    branches: [ main ]

jobs:
  test:
    runs-on: ubuntu-latest
    
    steps:
    - uses: actions/checkout@v3
    
    - name: Set up Node.js
      uses: actions/setup-node@v3
      with:
        node-version: '18'
    
    - name: Install dependencies
      run: npm ci
    
    - name: Run tests
      run: npm test
    
    - name: Run linter
      run: npm run lint
```

### Common Workflow Examples

#### Node.js CI/CD

```yaml
name: Node.js CI/CD

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  build-and-test:
    runs-on: ubuntu-latest
    
    strategy:
      matrix:
        node-version: [16.x, 18.x, 20.x]
    
    steps:
    - uses: actions/checkout@v3
    
    - name: Use Node.js ${{ matrix.node-version }}
      uses: actions/setup-node@v3
      with:
        node-version: ${{ matrix.node-version }}
        cache: 'npm'
    
    - run: npm ci
    - run: npm run build --if-present
    - run: npm test
    
    - name: Upload coverage
      uses: codecov/codecov-action@v3
      with:
        files: ./coverage/lcov.info
```

#### Python CI/CD

```yaml
name: Python CI

on: [push, pull_request]

jobs:
  test:
    runs-on: ubuntu-latest
    
    strategy:
      matrix:
        python-version: ['3.9', '3.10', '3.11']
    
    steps:
    - uses: actions/checkout@v3
    
    - name: Set up Python ${{ matrix.python-version }}
      uses: actions/setup-python@v4
      with:
        python-version: ${{ matrix.python-version }}
    
    - name: Install dependencies
      run: |
        python -m pip install --upgrade pip
        pip install -r requirements.txt
        pip install pytest pytest-cov
    
    - name: Run tests
      run: pytest --cov=./ --cov-report=xml
    
    - name: Upload coverage
      uses: codecov/codecov-action@v3
```

#### Docker Build and Push

```yaml
name: Docker Build and Push

on:
  push:
    branches: [ main ]
    tags: [ 'v*' ]

jobs:
  docker:
    runs-on: ubuntu-latest
    
    steps:
    - uses: actions/checkout@v3
    
    - name: Set up Docker Buildx
      uses: docker/setup-buildx-action@v2
    
    - name: Login to DockerHub
      uses: docker/login-action@v2
      with:
        username: ${{ secrets.DOCKERHUB_USERNAME }}
        password: ${{ secrets.DOCKERHUB_TOKEN }}
    
    - name: Build and push
      uses: docker/build-push-action@v4
      with:
        context: .
        push: true
        tags: user/app:latest
```

#### Deploy to GitHub Pages

```yaml
name: Deploy to GitHub Pages

on:
  push:
    branches: [ main ]

jobs:
  deploy:
    runs-on: ubuntu-latest
    
    steps:
    - uses: actions/checkout@v3
    
    - name: Setup Node.js
      uses: actions/setup-node@v3
      with:
        node-version: '18'
    
    - name: Install and Build
      run: |
        npm ci
        npm run build
    
    - name: Deploy
      uses: peaceiris/actions-gh-pages@v3
      with:
        github_token: ${{ secrets.GITHUB_TOKEN }}
        publish_dir: ./dist
```

### Secrets Management

```bash
# Add secrets: Settings → Secrets and variables → Actions → New repository secret

# Use in workflow
env:
  API_KEY: ${{ secrets.API_KEY }}
  DATABASE_URL: ${{ secrets.DATABASE_URL }}

# Common secrets:
# - GITHUB_TOKEN (automatically provided)
# - API keys
# - Deployment credentials
# - SSH keys
```

### Workflow Triggers

```yaml
# On push to specific branches
on:
  push:
    branches: [ main, develop ]

# On pull request
on:
  pull_request:
    branches: [ main ]

# On tag push
on:
  push:
    tags:
      - 'v*'

# On schedule (cron)
on:
  schedule:
    - cron: '0 0 * * *'  # Daily at midnight

# On manual trigger
on:
  workflow_dispatch:

# Multiple events
on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]
  schedule:
    - cron: '0 0 * * 0'  # Weekly
```

### Managing Workflows

```bash
# List workflows
gh workflow list

# View workflow runs
gh run list

# View specific run
gh run view 123456

# Watch run in progress
gh run watch

# Download artifacts
gh run download 123456

# Re-run failed jobs
gh run rerun 123456

# Cancel run
gh run cancel 123456
```

---

## GitHub Pages

### Setting Up GitHub Pages

#### Static Site

```bash
# 1. Create repository: username.github.io

# 2. Add HTML files
echo "<h1>Hello World</h1>" > index.html
git add index.html
git commit -m "Add homepage"
git push origin main

# 3. Visit: https://username.github.io
```

#### Project Site

```bash
# Settings → Pages → Source → Select branch (main or gh-pages)
# Choose folder: / (root) or /docs

# Visit: https://username.github.io/repository-name
```

### Jekyll Site

```bash
# Install Jekyll
gem install jekyll bundler

# Create new site
jekyll new my-site
cd my-site

# Test locally
bundle exec jekyll serve
# Visit: http://localhost:4000

# Push to GitHub
git add .
git commit -m "Initial Jekyll site"
git push origin main

# Enable GitHub Pages in Settings
```

### Custom Domain

```bash
# 1. Add CNAME file to repository
echo "www.yourdomain.com" > CNAME
git add CNAME
git commit -m "Add custom domain"
git push origin main

# 2. Configure DNS:
# Add CNAME record: www → username.github.io
# Add A records:
# @ → 185.199.108.153
# @ → 185.199.109.153
# @ → 185.199.110.153
# @ → 185.199.111.153

# 3. In GitHub Settings → Pages:
# Enter custom domain
# Enable "Enforce HTTPS"
```

---

## Security Features

### Dependabot

```yaml
# Enable: Settings → Security → Dependabot

# Configure: Create .github/dependabot.yml
version: 2
updates:
  - package-ecosystem: "npm"
    directory: "/"
    schedule:
      interval: "weekly"
    open-pull-requests-limit: 10
    reviewers:
      - "username"
    assignees:
      - "username"
  
  - package-ecosystem: "docker"
    directory: "/"
    schedule:
      interval: "weekly"
```

### Security Advisories

```bash
# View security advisories
# Security tab → View security advisories

# Create security advisory
# Security tab → Advisories → New draft security advisory

# Report vulnerability privately
# Security tab → Report a vulnerability
```

### Code Scanning

```yaml
# Enable: Security → Code scanning → Set up this workflow

# .github/workflows/codeql.yml
name: "CodeQL"

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]
  schedule:
    - cron: '0 0 * * 1'

jobs:
  analyze:
    runs-on: ubuntu-latest
    
    strategy:
      matrix:
        language: [ 'javascript', 'python' ]
    
    steps:
    - uses: actions/checkout@v3
    
    - name: Initialize CodeQL
      uses: github/codeql-action/init@v2
      with:
        languages: ${{ matrix.language }}
    
    - name: Autobuild
      uses: github/codeql-action/autobuild@v2
    
    - name: Perform CodeQL Analysis
      uses: github/codeql-action/analyze@v2
```

### Secret Scanning

```bash
# Automatically enabled for public repos

# For private repos:
# Settings → Security → Secret scanning → Enable

# GitHub scans for:
# - API keys
# - Tokens
# - Credentials
# - Private keys
```

### Branch Protection Rules

```bash
# Settings → Branches → Add rule

# Recommended settings:
☑ Require a pull request before merging
  ☑ Require approvals (1-2)
  ☑ Dismiss stale reviews
  ☑ Require review from Code Owners

☑ Require status checks to pass before merging
  ☑ Require branches to be up to date
  ☑ Select specific checks

☑ Require conversation resolution before merging

☑ Require signed commits

☑ Require linear history

☑ Include administrators

☑ Restrict who can push to matching branches
```

---

## GitHub CLI

### Installation

```bash
# Windows (winget)
winget install --id GitHub.cli

# Windows (scoop)
scoop install gh

# macOS
brew install gh

# Linux (Debian/Ubuntu)
curl -fsSL https://cli.github.com/packages/githubcli-archive-keyring.gpg | sudo gpg --dearmor -o /usr/share/keyrings/githubcli-archive-keyring.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/githubcli-archive-keyring.gpg] https://cli.github.com/packages stable main" | sudo tee /etc/apt/sources.list.d/github-cli.list > /dev/null
sudo apt update
sudo apt install gh

# Verify installation
gh --version
```

### Authentication

```bash
# Login to GitHub
gh auth login

# Check authentication status
gh auth status

# Logout
gh auth logout

# Set up Git credential helper
gh auth setup-git
```

### Repository Operations

```bash
# Create repository
gh repo create my-project --public
gh repo create my-project --private

# Clone repository
gh repo clone owner/repo

# View repository
gh repo view owner/repo

# Fork repository
gh repo fork owner/repo

# List repositories
gh repo list
gh repo list owner

# Archive repository
gh repo archive owner/repo

# Delete repository
gh repo delete owner/repo
```

### Issue Operations

```bash
# Create issue
gh issue create
gh issue create --title "Bug" --body "Description"

# List issues
gh issue list
gh issue list --label bug
gh issue list --state closed

# View issue
gh issue view 123

# Close issue
gh issue close 123

# Reopen issue
gh issue reopen 123

# Comment on issue
gh issue comment 123 --body "Comment"
```

### Pull Request Operations

```bash
# Create PR
gh pr create
gh pr create --title "Feature" --body "Description"

# List PRs
gh pr list
gh pr list --state closed

# View PR
gh pr view 123

# Checkout PR
gh pr checkout 123

# Review PR
gh pr review 123 --approve
gh pr review 123 --request-changes
gh pr review 123 --comment

# Merge PR
gh pr merge 123
gh pr merge 123 --squash
gh pr merge 123 --rebase

# Close PR
gh pr close 123
```

### Workflow Operations

```bash
# List workflows
gh workflow list

# View workflow
gh workflow view ci.yml

# Run workflow
gh workflow run ci.yml

# View runs
gh run list

# View specific run
gh run view 123456

# Watch run
gh run watch

# Cancel run
gh run cancel 123456

# Download artifacts
gh run download 123456
```

### Gist Operations

```bash
# Create gist
gh gist create file.txt
gh gist create --public file.txt

# List gists
gh gist list

# View gist
gh gist view abc123

# Edit gist
gh gist edit abc123

# Delete gist
gh gist delete abc123
```

---

## Advanced Features

### GitHub Sponsors

```bash
# Set up sponsorship:
# 1. Go to Settings → Sponsors
# 2. Join waitlist (if needed)
# 3. Connect Stripe account
# 4. Set sponsorship tiers

# Add sponsor button to repo:
# Create .github/FUNDING.yml
github: [username]
patreon: username
custom: ["https://www.buymeacoffee.com/username"]
```

### GitHub Packages

```bash
# Authenticate
echo $GITHUB_TOKEN | docker login ghcr.io -u USERNAME --password-stdin

# Build and tag Docker image
docker build -t ghcr.io/username/repo:latest .

# Push to GitHub Packages
docker push ghcr.io/username/repo:latest

# Pull from GitHub Packages
docker pull ghcr.io/username/repo:latest
```

### GitHub Codespaces

```bash
# Create codespace (GitHub Web)
# Click "Code" → "Codespaces" → "New codespace"

# Or use CLI
gh codespace create

# List codespaces
gh codespace list

# Connect to codespace
gh codespace ssh

# Delete codespace
gh codespace delete
```

### GitHub Discussions

```bash
# Enable Discussions
# Settings → Features → Discussions

# Categories:
# - 📣 Announcements
# - 💡 Ideas
# - 🙏 Q&A
# - 💬 General
# - 🎉 Show and tell

# Manage via GitHub CLI
gh api repos/:owner/:repo/discussions
```

### GitHub Wiki

```bash
# Clone wiki
git clone https://github.com/username/repo.wiki.git

# Edit wiki locally
cd repo.wiki
# ... edit markdown files ...
git add .
git commit -m "Update wiki"
git push origin main

# Or edit on GitHub Web
# Wiki tab → New Page / Edit
```

### Code Owners

Create `.github/CODEOWNERS`:

```bash
# Default owners for everything
* @username

# Owners for specific directories
/docs/ @doc-team
/src/ @dev-team
/tests/ @qa-team

# Owners for specific file types
*.js @frontend-team
*.py @backend-team
*.md @doc-team

# Owners for specific files
README.md @username
package.json @lead-dev
```

### Templates

#### Pull Request Template

`.github/pull_request_template.md`:

```markdown
## Description
<!-- Describe your changes -->

## Type of change
- [ ] Bug fix
- [ ] New feature
- [ ] Breaking change

## Checklist
- [ ] Tests pass
- [ ] Documentation updated
- [ ] Code reviewed
```

#### Issue Templates

`.github/ISSUE_TEMPLATE/config.yml`:

```yaml
blank_issues_enabled: false
contact_links:
  - name: Community Support
    url: https://discord.gg/community
    about: Ask questions in Discord
```

### GitHub Insights

```bash
# View insights:
# Insights tab

# Available insights:
# - Pulse (recent activity)
# - Contributors (contribution stats)
# - Community Standards (health metrics)
# - Traffic (views, clones)
# - Commits (commit activity)
# - Code frequency (additions/deletions)
# - Dependency graph
# - Network (forks)
# - Forks (active forks)
```

---

## Best Practices

### Repository Organization

```bash
# Good repository structure:
project/
├── .github/
│   ├── workflows/          # GitHub Actions
│   ├── ISSUE_TEMPLATE/     # Issue templates
│   ├── pull_request_template.md
│   ├── CODEOWNERS
│   └── dependabot.yml
├── docs/                   # Documentation
├── src/                    # Source code
├── tests/                  # Tests
├── .gitignore
├── README.md
├── LICENSE
├── CONTRIBUTING.md
└── CODE_OF_CONDUCT.md
```

### README Best Practices

```markdown
# Project Name

Brief description

## Features
- Feature 1
- Feature 2

## Installation
```bash
npm install project
```

## Usage
```javascript
const project = require('project');
project.doSomething();
```

## Documentation
See [docs](./docs)

## Contributing
See [CONTRIBUTING.md](./CONTRIBUTING.md)

## License
MIT License - see [LICENSE](./LICENSE)

## Badges
![Build](https://github.com/user/repo/workflows/CI/badge.svg)
![Coverage](https://codecov.io/gh/user/repo/branch/main/graph/badge.svg)
![Version](https://img.shields.io/github/v/release/user/repo)
```

### Commit Message Conventions

```bash
# Format: <type>(<scope>): <subject>

# Types:
feat:     # New feature
fix:      # Bug fix
docs:     # Documentation
style:    # Formatting, missing semi colons, etc
refactor: # Code restructuring
perf:     # Performance improvement
test:     # Adding tests
chore:    # Maintenance
ci:       # CI/CD changes

# Examples:
feat(auth): add JWT authentication
fix(api): resolve null pointer exception
docs(readme): update installation steps
style(css): format according to prettier
refactor(utils): simplify date formatting
perf(db): optimize query performance
test(auth): add unit tests for login
chore(deps): update dependencies
ci(actions): add deployment workflow
```

### Branch Naming

```bash
# Format: <type>/<description>

# Types:
feature/  # New features
bugfix/   # Bug fixes
hotfix/   # Urgent fixes
release/  # Release branches
docs/     # Documentation
refactor/ # Code refactoring
test/     # Testing

# Examples:
feature/user-authentication
bugfix/login-timeout
hotfix/critical-security-issue
release/v1.2.0
docs/api-documentation
refactor/database-layer
test/integration-tests
```

### Security Best Practices

```bash
# 1. Never commit secrets
# Use .gitignore for sensitive files
# Use GitHub Secrets for CI/CD

# 2. Enable branch protection
# Require reviews, status checks

# 3. Use Dependabot
# Auto-update dependencies

# 4. Enable security scanning
# Code scanning, secret scanning

# 5. Use signed commits
git config --global commit.gpgsign true

# 6. Review permissions
# Limit access, use least privilege

# 7. Enable 2FA
# Settings → Account security → Two-factor authentication
```

### Collaboration Best Practices

```bash
# 1. Clear documentation
# README, CONTRIBUTING, CODE_OF_CONDUCT

# 2. Issue templates
# Make reporting bugs/features easy

# 3. PR guidelines
# Clear expectations for contributions

# 4. Code review culture
# Constructive feedback, timely reviews

# 5. CI/CD automation
# Automated testing, deployment

# 6. Labels and milestones
# Organize work effectively

# 7. Communication
# Use issues, PRs, discussions appropriately
```

---

## Troubleshooting

### Common Issues

#### Push Rejected

```bash
# Error: Updates were rejected

# Solution: Pull first
git pull origin main --rebase
git push origin main
```

#### Permission Denied

```bash
# Error: Permission denied (publickey)

# Solution: Check SSH setup
ssh -T git@github.com

# Add SSH key
cat ~/.ssh/id_ed25519.pub
# Copy to GitHub: Settings → SSH keys
```

#### Large Files

```bash
# Error: File exceeds GitHub's 100 MB limit

# Solution: Use Git LFS
git lfs install
git lfs track "*.zip"
git add .gitattributes
git add large-file.zip
git commit -m "Add large file"
git push origin main
```

#### Merge Conflicts

```bash
# Error: Automatic merge failed

# Solution: Resolve manually
git status  # See conflicted files
# Edit files, remove conflict markers
git add resolved-file.txt
git commit -m "Resolve merge conflicts"
git push origin main
```

---

## Quick Reference

### Essential Commands

```bash
# Repository
gh repo create name --public
gh repo clone owner/repo
gh repo fork owner/repo

# Issues
gh issue create
gh issue list
gh issue close 123

# Pull Requests
gh pr create
gh pr list
gh pr checkout 123
gh pr merge 123

# Actions
gh workflow list
gh run list
gh run watch

# Authentication
gh auth login
gh auth status
```

### Keyboard Shortcuts (GitHub Web)

```bash
?                 # Show all shortcuts
s or /            # Focus search bar
g n               # Go to notifications
g d               # Go to dashboard
c                 # Create issue
t                 # File finder
l                 # Jump to line
b                 # Open blame view
y                 # Expand URL to permalink
```

---

## Resources

### Official Documentation
- GitHub Docs: https://docs.github.com
- GitHub Skills: https://skills.github.com
- GitHub Blog: https://github.blog

### Learning Resources
- GitHub Learning Lab: https://lab.github.com
- GitHub Guides: https://guides.github.com
- GitHub Training: https://services.github.com

### Community
- GitHub Community: https://github.community
- GitHub Support: https://support.github.com
- GitHub Status: https://www.githubstatus.com

### Tools
- GitHub Desktop: https://desktop.github.com
- GitHub CLI: https://cli.github.com
- GitHub Mobile: iOS/Android app

---

*This cheat sheet covers essential GitHub features and workflows. For detailed information, visit [GitHub Documentation](https://docs.github.com)*

**Remember:** GitHub is more than just Git hosting - it's a complete platform for collaboration, automation, and community! 🚀
