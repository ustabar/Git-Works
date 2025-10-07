# 📚 Git Learning Materials - Complete Tutorial Series

A comprehensive collection of Git tutorials with visual diagrams and detailed explanations. This repository provides step-by-step guides for mastering Git, from basic workflows to advanced operations like rebasing and squashing.

## 📋 Prerequisites

Before starting with these tutorials, you'll need to install Git and optionally GitHub CLI for enhanced workflow.

### 🔧 Git Installation

#### Windows
```powershell
# Option 1: Download installer
# Visit: https://git-scm.com/download/win
# Download and run the installer

# Option 2: Using winget (Windows Package Manager)
winget install --id Git.Git -e --source winget

# Option 3: Using Chocolatey
choco install git

# Verify installation
git --version
```

#### macOS
```bash
# Option 1: Using Homebrew (recommended)
brew install git

# Option 2: Using Xcode Command Line Tools
xcode-select --install

# Option 3: Download installer
# Visit: https://git-scm.com/download/mac

# Verify installation
git --version
```

#### Linux

**Debian/Ubuntu:**
```bash
sudo apt update
sudo apt install git

# Verify installation
git --version
```

**Fedora/RHEL/CentOS:**
```bash
sudo dnf install git
# or for older versions
sudo yum install git

# Verify installation
git --version
```

**Arch Linux:**
```bash
sudo pacman -S git

# Verify installation
git --version
```

### 🚀 GitHub CLI Installation (Optional but Recommended)

GitHub CLI (`gh`) provides a faster, more efficient way to interact with GitHub from your terminal.

#### Windows
```powershell
# Option 1: Using winget (recommended)
winget install --id GitHub.cli

# Option 2: Using Chocolatey
choco install gh

# Option 3: Using Scoop
scoop install gh

# Verify installation
gh --version
```

#### macOS
```bash
# Using Homebrew
brew install gh

# Verify installation
gh --version
```

#### Linux

**Debian/Ubuntu:**
```bash
# Add GitHub CLI repository
type -p curl >/dev/null || (sudo apt update && sudo apt install curl -y)
curl -fsSL https://cli.github.com/packages/githubcli-archive-keyring.gpg | sudo dd of=/usr/share/keyrings/githubcli-archive-keyring.gpg
sudo chmod go+r /usr/share/keyrings/githubcli-archive-keyring.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/githubcli-archive-keyring.gpg] https://cli.github.com/packages stable main" | sudo tee /etc/apt/sources.list.d/github-cli.list > /dev/null

# Install
sudo apt update
sudo apt install gh

# Verify installation
gh --version
```

**Fedora/RHEL/CentOS:**
```bash
sudo dnf install gh

# Verify installation
gh --version
```

**Arch Linux:**
```bash
sudo pacman -S github-cli

# Verify installation
gh --version
```

### ⚙️ Initial Configuration

After installing Git, configure your identity:

```bash
# Set your name
git config --global user.name "Your Name"

# Set your email (use your GitHub email)
git config --global user.email "your-email@example.com"

# Set default branch name to main
git config --global init.defaultBranch main

# Enable color output
git config --global color.ui auto

# Verify configuration
git config --list
```

### 🔐 GitHub CLI Authentication (If Installed)

```bash
# Authenticate with GitHub
gh auth login

# Follow the prompts:
# 1. Choose "GitHub.com"
# 2. Select "HTTPS" or "SSH"
# 3. Authenticate via web browser (recommended)
# 4. Complete authentication in browser

# Verify authentication
gh auth status
```

### ✅ Verification Checklist

Before proceeding with the tutorials, ensure:
- ✅ Git is installed (`git --version` shows version 2.x or higher)
- ✅ Git user name is configured (`git config user.name`)
- ✅ Git user email is configured (`git config user.email`)
- ✅ (Optional) GitHub CLI is installed (`gh --version`)
- ✅ (Optional) GitHub CLI is authenticated (`gh auth status`)

### 🆘 Troubleshooting Installation

#### Git not found after installation
```bash
# Windows: Restart terminal/PowerShell
# Close and reopen your terminal

# Verify PATH includes Git
# Windows PowerShell:
$env:PATH -split ';' | Select-String git

# macOS/Linux:
echo $PATH | grep git
```

#### GitHub CLI authentication issues
```bash
# Re-authenticate
gh auth logout
gh auth login

# Or use token authentication
gh auth login --with-token < token.txt
```

---

## 🎯 Overview

This project is designed as a complete learning resource for Git version control system. Each tutorial step includes:
- 🖼️ **Visual diagrams** illustrating Git operations
- 📝 **Detailed markdown documentation** with explanations
- 💻 **Command examples** with real-world scenarios
- ✅ **Best practices** and common pitfalls
- 🔍 **Troubleshooting guides** for common issues

## 📖 Tutorial Contents

### Basic Git Workflow
- **[Step 01](Steps/Step-01.md)**: Four-Stage Git Workflow (Working Directory → Staging → Local → Remote)
- **[Step 02](Steps/Step-02.md)**: Git Branching and Merging Fundamentals
- **[Step 03](Steps/Step-03.md)**: Clone and Branch Workflow
- **[Step 04](Steps/Step-04.md)**: Change, Commit, and Push Workflow
- **[Step 05](Steps/Step-05.md)**: Complete Git Workflow with All Four Areas
- **[Step 06](Steps/Step-06.md)**: Pull Workflow and Bidirectional Synchronization

### Essential Git Commands
- **[Step 07](Steps/Step-07.md)**: Eight Essential Git Commands Overview
- **[Step 08](Steps/Step-08.md)**: Pull vs Fetch - Understanding the Difference

### Branching Strategies
- **[Step 09](Steps/Step-09.md)**: Main and Develop Branch Strategy
- **[Step 10](Steps/Step-10.md)**: Complete Git Flow with Feature Branches

### Advanced Git Operations
- **[Step 11](Steps/Step-11.md)**: Merge Strategies (--no-ff vs Fast-Forward)
- **[Step 12](Steps/Step-12.md)**: Git Merge Visualization and Mechanics
- **[Step 13](Steps/Step-13.md)**: Git Rebase - Rewriting History for Clean Integration
- **[Step 14](Steps/Step-14.md)**: Git Squash Commit - Combining Multiple Commits

## 📂 Project Structure

```
Git-Works/
│
├── README.md                 # This file
├── .gitignore               # Git ignore rules
│
├── Steps/                   # Tutorial documentation files
│   ├── Step-01.md           # Basic Git workflow
│   ├── Step-02.md           # Branching and merging
│   ├── Step-03.md           # Clone and branch
│   ├── Step-04.md           # Change, commit, push
│   ├── Step-05.md           # Complete workflow
│   ├── Step-06.md           # Pull workflow
│   ├── Step-07.md           # Essential commands
│   ├── Step-08.md           # Pull vs Fetch
│   ├── Step-09.md           # Main and Develop strategy
│   ├── Step-10.md           # Complete Git Flow
│   ├── Step-11.md           # Merge strategies
│   ├── Step-12.md           # Merge visualization
│   ├── Step-13.md           # Git Rebase
│   └── Step-14.md           # Squash commits
│
├── Diagrams/                # Visual diagrams for each step
│   ├── Step-01.png
│   ├── Step-02.png
│   ├── Step-03.png
│   ├── Step-04.png
│   ├── Step-05.png
│   ├── Step-06.png
│   ├── Step-07.png
│   ├── Step-08.png
│   ├── Step-09.png
│   ├── Step-10.png
│   ├── Step-11.png
│   ├── Step-12.png
│   ├── Step-13.png
│   └── Step-14.png
│
└── CheatSheets/             # Quick reference guides
    ├── git_cheatsheet_EN.md      # Git commands (English)
    ├── git_cheatsheet_TR.md      # Git commands (Turkish)
    ├── git_internals_EN.md       # Git internals guide
    ├── git_stash_EN.md           # Git stash operations
    └── github_cheatsheet_EN.md   # GitHub-specific commands
 
```

## 🚀 Key Features

### Comprehensive Coverage
- ✅ Complete Git workflow from basics to advanced
- ✅ Visual learning with detailed diagrams
- ✅ Both English and Turkish documentation available
- ✅ Real-world examples and use cases

### Learning Path
- 📊 Progressive difficulty from beginner to advanced
- 🎯 Each step builds upon previous knowledge
- 💡 Practical scenarios and problem-solving
- ⚠️ Common mistakes and how to avoid them

### Documentation Quality
- 📝 Detailed explanations with code examples
- 🔍 Troubleshooting sections for each topic
- ✅ Best practices and do's/don'ts
- 🎨 ASCII diagrams for better visualization

## 🎓 Learning Path

### For Beginners
1. Start with **Step 01** (Basic Workflow)
2. Continue to **Step 02** (Branching)
3. Practice with **Step 03** (Clone and Branch)
4. Review **Step 07** (Essential Commands)
5. Understand **Step 08** (Pull vs Fetch)

### For Intermediate Users
1. Study **Step 09** (Branching Strategy)
2. Learn **Step 10** (Complete Git Flow)
3. Master **Step 11** (Merge Strategies)
4. Visualize **Step 12** (Merge Mechanics)

### For Advanced Users
1. Deep dive into **Step 13** (Rebase)
2. Master **Step 14** (Squash Commits)
3. Review **CheatSheets/git_internals_EN.md** (Git Internals)
4. Explore **CheatSheets/git_stash_EN.md** (Stash Operations)

## 📚 Additional Resources

- **[git_cheatsheet_EN.md](CheatSheets/git_cheatsheet_EN.md)** - Quick reference for common Git commands
- **[git_cheatsheet_TR.md](CheatSheets/git_cheatsheet_TR.md)** - Türkçe Git komut referansı
- **[git_internals_EN.md](CheatSheets/git_internals_EN.md)** - Deep dive into how Git works internally
- **[git_stash_EN.md](CheatSheets/git_stash_EN.md)** - Complete guide to Git stash operations
- **[github_cheatsheet_EN.md](CheatSheets/github_cheatsheet_EN.md)** - GitHub-specific commands and workflows

## 💡 How to Use This Repository

### Reading the Tutorials
Each Step-XX.md file is self-contained and includes:
- Overview section explaining the concept
- Visual diagram reference
- Detailed explanation with examples
- Command-line instructions
- Best practices and tips
- Troubleshooting guide
- Key takeaways and summary

### Following Along
```bash
# Clone this repository
git clone https://github.com/ustabar/Git-Works.git

# Navigate to the project
cd Git-Works

# Open any tutorial
# Recommended: Start with Step-01.md
```

### Practice Projects
Use the knowledge from these tutorials to:
- Set up your own Git repositories
- Practice branching and merging
- Experiment with rebase and squash
- Contribute to open-source projects
- Manage team collaboration workflows

## 🛠️ Technologies Covered

- **Git** - Version control system
- **GitHub** - Remote repository hosting
- **GitLab** - Alternative platform (mentioned)
- **Bitbucket** - Alternative platform (mentioned)
- **Markdown** - Documentation format
- **Command Line** - Git CLI operations

## 🎯 Target Audience

- 🆕 **Beginners** learning Git for the first time
- 📈 **Intermediate users** wanting to improve their Git skills
- 🚀 **Advanced users** looking for reference materials
- 👥 **Teams** establishing Git workflows
- 🎓 **Educators** teaching version control
- 📖 **Self-learners** preferring visual documentation

## 📊 Tutorial Statistics

- **14 Step-by-step tutorials** with visual diagrams
- **14 Detailed diagrams** illustrating Git operations
- **5 Additional reference documents** (cheat sheets and internals)
- **Comprehensive coverage** from basic to advanced topics
- **Multiple languages** (English and Turkish)

## ⚠️ Important Notes

### Golden Rules
- ❌ **Never rebase public/shared branches** (covered in Step 13)
- ✅ **Always pull before push** to avoid conflicts
- ✅ **Write meaningful commit messages** for better history
- ✅ **Use branches for features** to keep main stable
- ✅ **Test before merging** to avoid breaking changes

### Best Practices
- 🔄 Commit frequently with small, logical changes
- 📝 Use clear and descriptive commit messages
- 🌿 Create feature branches for new work
- 🧪 Test your code before committing
- 📊 Review changes before pushing
- 🔍 Use git status often to check repository state

## 🤝 Contributing

This is a learning resource repository. If you find errors or have suggestions:
1. Fork the repository
2. Create a feature branch (`git checkout -b feature/improvement`)
3. Make your changes
4. Commit with clear messages (`git commit -m "Fix typo in Step-05"`)
5. Push to your fork (`git push origin feature/improvement`)
6. Open a Pull Request

## 📝 License

This project is created for educational purposes. Feel free to use, share, and modify for learning.

## 🌟 Acknowledgments

- Git documentation and community
- Visual diagram inspirations from various Git tutorials
- Contributors to Git learning resources worldwide

## 📧 Contact & Support

If you have questions or suggestions about these tutorials:
- Open an issue in this repository
- Review the troubleshooting sections in each tutorial
- Consult the official Git documentation at [git-scm.com](https://git-scm.com)

---

## 🚀 Git Deployment Steps

### Prerequisites
Before deploying this project to Git and GitHub, ensure you have:
- ✅ Git installed on your system (`git --version`)
- ✅ GitHub account created
- ✅ Git configured with your user information

### Step 1: Configure Git (First Time Setup)

```bash
# Set your username
git config --global user.name "Your Name"

# Set your email (use your GitHub email)
git config --global user.email "your-email@example.com"

# Verify configuration
git config --list
```

### Step 2: Initialize Local Git Repository

```bash
# Navigate to project directory
cd c:\Codes\Git-Works

# Initialize Git repository
git init

# Check status
git status
```

### Step 3: Add Files to Git

```bash
# Add all files to staging area
git add .

# Or add specific files
git add README.md
git add .gitignore
git add Step-*.md

# Check what will be committed
git status
```

### Step 4: Create Initial Commit

```bash
# Commit with descriptive message
git commit -m "Initial commit: Complete Git tutorial series with 14 steps

- Add comprehensive tutorials (Step-01 through Step-14)
- Include visual diagrams for each concept
- Add Git and GitHub cheat sheets
- Include documentation in English and Turkish
- Add .gitignore and README.md"

# Verify commit
git log --oneline
```

### Step 5: Rename Branch to Main (If Needed)

```bash
# Rename master to main (modern convention)
git branch -M main

# Verify branch name
git branch
```

### Step 6: Create GitHub Repository

**Option A: Using GitHub Website**
1. Go to [https://github.com](https://github.com)
2. Click the **"+"** icon in the top-right corner
3. Select **"New repository"**
4. Fill in repository details:
   - **Repository name:** `Git-Works`
   - **Description:** "Complete Git tutorial series with step-by-step guides and visual diagrams"
   - **Visibility:** Choose Public or Private
   - ⚠️ **DO NOT** initialize with README, .gitignore, or license (you already have them)
5. Click **"Create repository"**

**Option B: Using GitHub CLI (Faster & Easier)**

```bash
# Install GitHub CLI from: https://cli.github.com/
# Or via winget on Windows:
winget install --id GitHub.cli

# Authenticate (first time only)
gh auth login
# Follow prompts to authenticate via browser

# Create repository (public)
gh repo create Git-Works --public --source=. --remote=origin --description "Complete Git tutorial series with step-by-step guides and visual diagrams"

# Or create repository (private)
gh repo create Git-Works --private --source=. --remote=origin --description "Complete Git tutorial series with step-by-step guides and visual diagrams"

# This single command:
# ✅ Creates the repository on GitHub
# ✅ Sets up the remote 'origin'
# ✅ Links your local repository to GitHub
# ✅ No need for Step 7 if using this method!

# Push to GitHub
git push -u origin main
```

**GitHub CLI Advantages:**
- ⚡ Faster - one command instead of multiple steps
- 🔒 Secure - uses OAuth authentication
- 🤖 Automated - no manual clicking
- 💻 Terminal-friendly - perfect for developers

### Step 7: Connect Local Repository to GitHub (If Using Option A)

⚠️ **Note:** Skip this step if you used GitHub CLI in Step 6 (Option B) - it already set up the remote!

```bash
# Add remote repository (replace YOUR_USERNAME with your GitHub username)
git remote add origin https://github.com/ustabar/Git-Works.git

# Verify remote was added
git remote -v

# Should show:
# origin  https://github.com/YOUR_USERNAME/Git-Works.git (fetch)
# origin  https://github.com/YOUR_USERNAME/Git-Works.git (push)
```

### Step 8: Push to GitHub

```bash
# Push to GitHub (first time)
git push -u origin main

# Enter credentials when prompted
# Username: your-github-username
# Password: your-personal-access-token (NOT your GitHub password)
```

### Step 9: Authentication Setup

#### Option A: Personal Access Token (Recommended)

```bash
# Create a Personal Access Token on GitHub:
# 1. GitHub → Settings → Developer settings
# 2. Personal access tokens → Tokens (classic)
# 3. Generate new token (classic)
# 4. Set expiration (90 days recommended)
# 5. Select scopes: check 'repo' (full control of private repositories)
# 6. Click "Generate token"
# 7. Copy the token immediately (you won't see it again!)

# When pushing, use token as password:
# Username: your-github-username
# Password: ghp_xxxxxxxxxxxxxxxxxxxx (your token)
```

#### Option B: SSH Key (Most Secure)

```bash
# Generate SSH key
ssh-keygen -t ed25519 -C "your-email@example.com"

# Start SSH agent
eval "$(ssh-agent -s)"

# Add SSH key to agent
ssh-add ~/.ssh/id_ed25519

# Copy public key to clipboard (Windows PowerShell)
Get-Content ~/.ssh/id_ed25519.pub | Set-Clipboard

# Add to GitHub:
# 1. GitHub → Settings → SSH and GPG keys
# 2. Click "New SSH key"
# 3. Paste your key
# 4. Click "Add SSH key"

# Change remote URL to SSH
git remote set-url origin git@github.com:ustabar/Git-Works.git

# Push using SSH
git push -u origin main
```

#### Option C: GitHub CLI (Easiest)

```bash
# Install GitHub CLI from: https://cli.github.com/

# Authenticate
gh auth login

# Follow the prompts to authenticate via browser

# Push
git push -u origin main
```

### Step 10: Verify Deployment

```bash
# Check remote status
git remote -v

# Check branch tracking
git branch -vv

# Verify everything is pushed
git status
# Should show: "Your branch is up to date with 'origin/main'"
```

**On GitHub:**
1. Visit your repository: `https://github.com/ustabar/Git-Works`
2. Verify all files are present
3. Check that README.md is displayed on the main page
4. Review the commit history

### Step 11: Future Updates

```bash
# After making changes to any file:

# 1. Check what changed
git status

# 2. Add modified files
git add .
# Or specific files: git add Step-15.md

# 3. Commit with descriptive message
git commit -m "Add Step-15: Advanced Git techniques"

# 4. Push to GitHub
git push
# No need for -u origin main after the first push

# 5. Verify on GitHub
# Check your repository to see the updates
```

### Common Commands Reference

```bash
# View commit history
git log --oneline --graph --all

# Check repository status
git status

# See what changed
git diff

# Undo last commit (keep changes)
git reset --soft HEAD~1

# Undo last commit (discard changes)
git reset --hard HEAD~1

# Pull latest changes from GitHub
git pull origin main

# Clone this repository elsewhere
git clone https://github.com/ustabar/Git-Works.git
```

### Troubleshooting Deployment Issues

#### Issue: "fatal: remote origin already exists"
```bash
# Remove existing remote
git remote remove origin

# Add correct remote
git remote add origin https://github.com/ustabar/Git-Works.git
```

#### Issue: "failed to push some refs"
```bash
# Pull and merge first
git pull origin main --allow-unrelated-histories

# Then push
git push -u origin main
```

#### Issue: "Authentication failed"
```bash
# Ensure you're using a Personal Access Token, not your password
# GitHub no longer accepts password authentication

# Or set up SSH authentication (see Option B above)
```

#### Issue: "Permission denied (publickey)"
```bash
# If using SSH, verify your SSH key is added to GitHub
ssh -T git@github.com

# Should respond: "Hi USERNAME! You've successfully authenticated"
```

### Post-Deployment Checklist

- ✅ All files visible on GitHub
- ✅ README.md displays correctly on repository homepage
- ✅ Diagrams folder contains all images
- ✅ All markdown files (Step-01 through Step-14) are present
- ✅ Commit history is clean and organized
- ✅ Repository description is set
- ✅ .gitignore is working (no unwanted files)
- ✅ Can clone repository from another location

---

## 🔄 Undoing Changes in Git

You may need to undo changes you've made in Git. Here are the most common scenarios and their solutions:

### Scenario 1: Undo Both Staging and Commit

**Situation:** You staged changes with `git add .` and committed with `git commit`, but you want to undo both the commit and staging.

#### Solution 1: Completely Undo Commit and Staging (Keep Changes)

```bash
# Undo the last commit, keep changes in working directory
git reset --soft HEAD~1

# Files are still in staging area
# To unstage them:
git restore --staged .

# Now files are in working directory, commit and staging are cancelled
git status
# Changes not staged for commit: (shown in red)
```

**Step-by-step what happened:**
1. `git reset --soft HEAD~1` → Commit undone, files remain in staging
2. `git restore --staged .` → Files moved from staging to working directory
3. Changes still exist in files, you can make your corrections

#### Solution 2: Undo Commit, Keep in Staging

```bash
# Only undo commit, keep files in staging
git reset --soft HEAD~1

# Files are still staged, you can commit again
git status
# Changes to be committed: (shown in green)
```

#### Solution 3: Completely Undo Everything (DELETE Changes)

```bash
# ⚠️ WARNING: This command permanently deletes all changes!
git reset --hard HEAD~1

# Commit undone + Staging cleared + File changes deleted
git status
# nothing to commit, working tree clean
```

### Scenario 2: Undo Only Staging (Without Commit)

**Situation:** You ran `git add .` but haven't committed yet, and you want to unstage.

```bash
# Remove all files from staging
git restore --staged .

# Or the old command (still works):
git reset HEAD .

# To unstage a specific file:
git restore --staged filename.txt

# Check status
git status
# Changes not staged for commit: (in red)
```

### Scenario 3: Undo File Changes

**Situation:** You modified a file but haven't run `git add` yet, and you want to cancel the changes.

```bash
# Cancel all changes (return to last commit state)
git restore .

# Or the old command:
git checkout -- .

# Cancel changes in a specific file
git restore filename.txt

# ⚠️ WARNING: These commands permanently delete changes!
```

### Scenario 4: Amend Last Commit

**Situation:** You made a commit but want to change the commit message or files.

```bash
# To change commit message:
git commit --amend -m "New commit message"

# To add a forgotten file to the same commit:
git add forgotten_file.txt
git commit --amend --no-edit

# To add file and change message:
git add forgotten_file.txt
git commit --amend -m "Updated commit message"
```

### Scenario 5: Undo Multiple Commits

```bash
# Undo last 3 commits (keep changes)
git reset --soft HEAD~3

# Undo last 3 commits (keep in staging)
git reset --mixed HEAD~3

# Undo last 3 commits (delete everything)
git reset --hard HEAD~3

# Return to a specific commit (with commit hash)
git reset --soft abc1234
```

### Scenario 6: Undo Pushed Commit

**Situation:** You pushed a commit to GitHub and want to undo it.

#### Method 1: Revert (Recommended - Safe)

```bash
# Create a new "undo" commit
git revert HEAD

# Revert multiple commits
git revert HEAD~3..HEAD

# Push
git push origin main
```

**Advantages:**
- ✅ Doesn't delete history (safe)
- ✅ No issues in team workflows
- ✅ Audit trail preserved

#### Method 2: Reset + Force Push (Risky)

```bash
# ⚠️ WARNING: Only use on your own branch!

# Undo locally
git reset --hard HEAD~1

# Force push
git push --force origin main
# Or safer:
git push --force-with-lease origin main
```

**Risks:**
- ❌ Can break others' work
- ❌ Rewrites history
- ❌ Don't use in team workflows

### Git Reset Options Comparison

| Command | Commit | Staging | Working Directory | Use Case |
|---------|--------|---------|-------------------|----------|
| `git reset --soft HEAD~1` | ✅ Undoes | ❌ Keeps | ❌ Keeps | To fix commit |
| `git reset --mixed HEAD~1` | ✅ Undoes | ✅ Undoes | ❌ Keeps | To clear staging too |
| `git reset --hard HEAD~1` | ✅ Undoes | ✅ Undoes | ✅ Undoes | To delete everything (CAREFUL!) |

### Practical Examples

#### Example 1: "I committed the wrong file"

```bash
# Undo last commit
git reset --soft HEAD~1

# Unstage wrong file
git restore --staged wrong_file.txt

# Commit again with correct files
git commit -m "Correct commit"
```

#### Example 2: "My commit message is wrong"

```bash
# Change commit message
git commit --amend -m "Correct commit message"

# If you already pushed:
git push --force-with-lease origin main
```

#### Example 3: "I added too many files"

```bash
# Remove specific files from staging
git restore --staged unnecessary_file1.txt unnecessary_file2.txt

# Or remove all and select again
git restore --staged .
git add needed_file1.txt needed_file2.txt
git commit -m "Only needed files"
```

#### Example 4: "I committed test code"

```bash
# Undo last commit but keep changes
git reset --soft HEAD~1

# Delete test code
git restore test_file.txt

# Commit actual code
git add .
git commit -m "Production code"
```

### Solution for Your Specific Scenario

**Your Situation:**
1. ✅ You ran `git add .` (staged files)
2. ✅ You ran `git commit -m "new changes"`
3. ❌ You noticed an error
4. 🎯 You want to undo both commit and staging

**Solution:**

```bash
# Step 1: Undo last commit (keep changes in working directory)
git reset --soft HEAD~1

# Step 2: Clear staging
git restore --staged .

# Check status
git status
# Now files are under "Changes not staged for commit" (red)

# Step 3: Fix the error
# Edit your files...

# Step 4: Commit again with corrected version
git add .
git commit -m "Corrected changes"
```

### Important Warnings

⚠️ **BE CAREFUL when using `git reset --hard`:**
- Permanently deletes all your changes
- Cannot be recovered (recoverable from reflog but complex)
- Make a backup if unsure

⚠️ **BE CAREFUL when using force push:**
- Causes issues in team workflows
- Can break others' work
- Only use on your own branch

✅ **Safe Methods:**
- `git reset --soft` → Safest, keeps everything
- `git revert` → Ideal for public branches
- `git commit --amend` → To fix last commit
- `git restore --staged` → Safely clears staging

### Undoing Changes Flow Diagram

```
Made Changes
    ↓
Did You Stage? (git add)
    ├─ NO → git restore file.txt (delete changes)
    │
    └─ YES → Did You Commit?
           ├─ NO → git restore --staged . (unstage)
           │
           └─ YES → Did You Push?
                  ├─ NO → git reset --soft HEAD~1 (undo commit)
                  │
                  └─ YES → git revert HEAD (safe)
                         or
                         git reset + force push (risky)
```

### Recovery

If you accidentally deleted something, you can recover it with reflog:

```bash
# View history of all operations
git reflog

# Example output:
# abc1234 HEAD@{0}: reset: moving to HEAD~1
# def5678 HEAD@{1}: commit: new changes
# ghi9012 HEAD@{2}: commit: previous commit

# Return to a specific point
git reset --hard HEAD@{1}

# Or with commit hash:
git reset --hard def5678
```

---

## 🎯 Quick Reference: Undoing Changes

| What to Undo | Command | Are Changes Preserved? |
|--------------|---------|------------------------|
| File changes (unstaged) | `git restore file.txt` | ❌ No |
| Staging (git add) | `git restore --staged .` | ✅ Yes (in working directory) |
| Last commit (soft) | `git reset --soft HEAD~1` | ✅ Yes (in staging) |
| Last commit (mixed) | `git reset --mixed HEAD~1` | ✅ Yes (in working directory) |
| Last commit (hard) | `git reset --hard HEAD~1` | ❌ No (everything deleted) |
| Pushed commit | `git revert HEAD` | ✅ Yes (with new commit) |
| Commit message | `git commit --amend -m "..."` | ✅ Yes |

---

## 🎉 Success!

Your Git tutorial project is now live on GitHub! Share the repository URL with others who want to learn Git.

**Repository URL:** `https://github.com/ustabar/Git-Works`

Happy learning and teaching! 🚀


