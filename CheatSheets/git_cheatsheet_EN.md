# Git Commands - Comprehensive Cheat Sheet

## 📋 Table of Contents
- [Installation and Configuration](#installation-and-configuration)
- [Repository Creation](#repository-creation)
- [Basic Commands](#basic-commands)
- [Branch Operations](#branch-operations)
- [Commit Operations](#commit-operations)
- [Remote Operations](#remote-operations)
- [Merge and Rebase](#merge-and-rebase)
- [Undoing Changes](#undoing-changes)
- [Stash Operations](#stash-operations)
- [Log and History](#log-and-history)
- [Tag Operations](#tag-operations)
- [Advanced Commands](#advanced-commands)

---

## Installation and Configuration

### Initial Setup
```bash
# Set user name
git config --global user.name "Your Name"

# Set email
git config --global user.email "email@example.com"

# Set default editor
git config --global core.editor "code --wait"  # VS Code
git config --global core.editor "vim"           # Vim

# Set default branch name (main)
git config --global init.defaultBranch main

# View configuration
git config --list
git config user.name
git config --global --list

# Edit configuration
git config --global --edit
```

### Useful Config Settings
```bash
# Colored output
git config --global color.ui auto

# Line ending characters (Windows)
git config --global core.autocrlf true

# Line ending characters (Mac/Linux)
git config --global core.autocrlf input

# Create aliases
git config --global alias.st status
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.ci commit
git config --global alias.unstage 'reset HEAD --'
git config --global alias.last 'log -1 HEAD'
git config --global alias.visual 'log --oneline --graph --all'
```

---

## Repository Creation

### Creating New Repository
```bash
# Turn current directory into Git repo
git init

# Create new directory and make it a Git repo
git init project-name

# Create bare repository (for server)
git init --bare
```

### Cloning Existing Repository
```bash
# Clone with HTTPS
git clone https://github.com/user/repo.git

# Clone with SSH
git clone git@github.com:user/repo.git

# Clone with different folder name
git clone https://github.com/user/repo.git new-folder

# Clone specific branch
git clone -b develop https://github.com/user/repo.git

# Shallow clone (only last commit)
git clone --depth 1 https://github.com/user/repo.git

# Clone single branch
git clone --single-branch --branch main https://github.com/user/repo.git
```

---

## Basic Commands

### Status Check
```bash
# Working directory status
git status

# Short format
git status -s
git status --short

# With branch information
git status -sb
```

### Adding Files (Staging)
```bash
# Add single file
git add file.txt

# Add multiple files
git add file1.txt file2.txt

# Add all files
git add .
git add -A
git add --all

# Add files with specific extension
git add *.js

# Interactive adding
git add -i

# Patch mode (add part of file)
git add -p file.txt

# Add new and modified files (excluding deleted)
git add .

# Add all changes (including deleted)
git add -A
```

### Unstaging Files
```bash
# Remove file from staging (keep changes)
git reset HEAD file.txt
git restore --staged file.txt

# Remove all files from staging
git reset HEAD
```

### Deleting Files
```bash
# Delete file and stage it
git rm file.txt

# Remove from Git only (keep file)
git rm --cached file.txt

# Delete folder
git rm -r folder/

# Force delete
git rm -f file.txt
```

### Moving/Renaming Files
```bash
# Move or rename file
git mv old-name.txt new-name.txt
git mv file.txt folder/file.txt
```

### Viewing Changes
```bash
# Working directory vs Staging area
git diff

# Staging area vs Last commit
git diff --staged
git diff --cached

# Working directory vs Last commit
git diff HEAD

# Difference between two commits
git diff commit1 commit2

# Difference for specific file
git diff file.txt

# Difference between branches
git diff main..feature-branch

# Show only changed file names
git diff --name-only

# Statistical summary
git diff --stat
```

---

## Branch Operations

### Creating Branches
```bash
# Create new branch (don't switch)
git branch feature-login

# Create new branch and switch
git checkout -b feature-login
git switch -c feature-login      # New syntax

# Create branch from specific commit
git branch bugfix abc1234
git checkout -b bugfix abc1234

# Create local branch from remote branch
git checkout -b feature origin/feature
git checkout --track origin/feature

# Create branch from another branch
git checkout -b new-feature develop
```

### Switching Between Branches
```bash
# Switch to branch (old)
git checkout main

# Switch to branch (new)
git switch main

# Return to previous branch
git checkout -
git switch -

# Go to specific commit in detached HEAD mode
git checkout abc1234
```

### Listing Branches
```bash
# List local branches
git branch

# List remote branches
git branch -r

# List all branches
git branch -a

# List with last commit
git branch -v
git branch -vv  # With upstream information

# Merged branches
git branch --merged
git branch --merged main

# Unmerged branches
git branch --no-merged

# Filter with pattern
git branch --list "feature/*"
```

### Deleting Branches
```bash
# Delete local branch (safe)
git branch -d feature-login

# Delete local branch (force)
git branch -D feature-login

# Delete remote branch
git push origin --delete feature-login
git push origin :feature-login     # Old syntax

# Delete multiple branches
git branch -d branch1 branch2 branch3

# Delete all merged branches
git branch --merged | grep -v "\*" | grep -v "main" | xargs -n 1 git branch -d
```

### Renaming Branches
```bash
# Rename current branch
git branch -m new-name

# Rename another branch
git branch -m old-name new-name

# Update remote as well
git push origin --delete old-name
git push origin -u new-name
```

---

## Commit Operations

### Creating Commits
```bash
# Simple commit
git commit -m "Commit message"

# Detailed commit message (opens editor)
git commit

# Add and commit together (for tracked files)
git commit -am "Commit message"
git commit -a -m "Commit message"

# Empty commit (without changes)
git commit --allow-empty -m "Empty commit"

# Amend last commit (change message)
git commit --amend -m "New message"

# Add files to last commit (message unchanged)
git commit --amend --no-edit

# Change commit date
git commit --date="2024-01-01 12:00:00" -m "Message"

# Commit with different author
git commit --author="Name <email@example.com>" -m "Message"
```

### Commit History
```bash
# Commit history
git log

# One line
git log --oneline

# With graph
git log --graph
git log --oneline --graph --all

# Last N commits
git log -n 5
git log -5

# Specific date range
git log --since="2024-01-01"
git log --after="2 weeks ago"
git log --until="2024-12-31"
git log --before="yesterday"

# Commits by specific author
git log --author="Name"

# Search in commit messages
git log --grep="bugfix"

# History of specific file
git log -- file.txt
git log -p file.txt  # With changes

# With statistics
git log --stat

# Short summary
git log --oneline --graph --decorate --all

# Pretty format
git log --pretty=format:"%h - %an, %ar : %s"
```

### Commit Details
```bash
# Last commit details
git show

# Specific commit details
git show abc1234

# Specific file at specific commit
git show abc1234:file.txt

# Show changes in commit
git show --stat abc1234
```

---

## Remote Operations

### Remote Repository Management
```bash
# List remotes
git remote
git remote -v        # With URLs

# Add remote
git remote add origin https://github.com/user/repo.git

# Change remote URL
git remote set-url origin https://github.com/user/new-repo.git

# Rename remote
git remote rename origin upstream

# Remove remote
git remote remove origin
git remote rm origin

# Show remote details
git remote show origin
```

### Push (Sending)
```bash
# Push current branch
git push

# Push for first time and set upstream
git push -u origin main
git push --set-upstream origin main

# Push specific branch
git push origin feature-login

# Push all branches
git push --all

# Push tags
git push --tags

# Force push (CAREFUL!)
git push -f
git push --force

# Safe force push
git push --force-with-lease

# Delete remote branch
git push origin --delete feature-login

# Dry run (show what would happen, don't push)
git push --dry-run
```

### Fetch (Pulling - Without Merge)
```bash
# Fetch all remote changes
git fetch

# Fetch from specific remote
git fetch origin

# Fetch specific branch
git fetch origin main

# Fetch all remotes
git fetch --all

# Prune deleted remote branches
git fetch --prune
git fetch -p

# Fetch tags as well
git fetch --tags
```

### Pull (Fetch and Merge)
```bash
# Fetch + Merge
git pull

# Pull from specific remote and branch
git pull origin main

# Pull with rebase
git pull --rebase
git pull -r

# No automatic commit (if not fast-forward)
git pull --no-commit

# Pull only if fast-forward
git pull --ff-only

# Pull from all remotes
git pull --all
```

---

## Merge and Rebase

### Merge
```bash
# Merge branch into current branch
git merge feature-branch

# Fast-forward merge (if possible)
git merge feature-branch

# Always create merge commit
git merge --no-ff feature-branch

# Merge only if fast-forward possible
git merge --ff-only feature-branch

# Merge but don't commit
git merge --no-commit feature-branch

# Abort merge
git merge --abort

# Squash merge (compress into single commit)
git merge --squash feature-branch

# Specify merge strategy
git merge -s recursive feature-branch
git merge -X theirs feature-branch    # Take their version on conflict
git merge -X ours feature-branch      # Take our version on conflict
```

### Rebase
```bash
# Rebase current branch onto another branch
git rebase main

# Interactive rebase (last 3 commits)
git rebase -i HEAD~3

# Rebase from specific commit
git rebase -i abc1234

# Continue rebase (after conflict)
git rebase --continue

# Abort rebase
git rebase --abort

# Skip current commit
git rebase --skip

# Rebase using onto
git rebase --onto main develop feature
```

### Interactive Rebase Commands
```bash
# Commands used in editor:
# pick   = use commit
# reword = use commit but edit message
# edit   = use commit, stop for amending
# squash = use commit, merge with previous
# fixup  = like squash but discard commit message
# drop   = remove commit
# exec   = run shell command

# Example:
git rebase -i HEAD~3
# pick abc123 First commit
# squash def456 Second commit
# reword ghi789 Third commit
```

### Cherry-Pick
```bash
# Copy specific commit to current branch
git cherry-pick abc1234

# Multiple commits
git cherry-pick abc1234 def5678

# Commit range
git cherry-pick abc1234..def5678

# Cherry-pick but don't commit
git cherry-pick -n abc1234
git cherry-pick --no-commit abc1234

# Abort
git cherry-pick --abort

# Continue
git cherry-pick --continue
```

---

## Undoing Changes

### Undo Changes in Working Directory
```bash
# Undo changes in file
git checkout -- file.txt
git restore file.txt        # New syntax

# Undo all changes
git checkout -- .
git restore .

# Restore to specific commit
git checkout abc1234 -- file.txt
git restore --source=abc1234 file.txt
```

### Undo Staged Changes
```bash
# Unstage file
git reset HEAD file.txt
git restore --staged file.txt

# Unstage all files
git reset HEAD
```

### Undoing Commits

#### Reset (Changes History)
```bash
# Soft reset (undo commit, keep changes staged)
git reset --soft HEAD~1

# Mixed reset (default - undo commit, keep changes in working dir)
git reset HEAD~1
git reset --mixed HEAD~1

# Hard reset (completely remove commit and changes)
git reset --hard HEAD~1

# Reset to specific commit
git reset --hard abc1234

# Sync with remote
git reset --hard origin/main
```

#### Revert (Creates New Commit)
```bash
# Create new commit that undoes a commit
git revert abc1234

# Revert multiple commits
git revert abc1234 def5678

# Revert merge commit
git revert -m 1 abc1234

# Revert but don't commit
git revert -n abc1234
git revert --no-commit abc1234
```

### Clean (Delete Untracked Files)
```bash
# Show untracked files
git clean -n
git clean --dry-run

# Delete untracked files
git clean -f
git clean --force

# Delete untracked files and folders
git clean -fd

# Delete .gitignore files as well
git clean -fx

# Interactive clean
git clean -i
```

---

## Stash Operations

### Creating Stash
```bash
# Stash changes
git stash
git stash save

# Named stash
git stash save "Work in progress"

# Include untracked files
git stash -u
git stash --include-untracked

# Stash all files (including ignored)
git stash -a
git stash --all

# Stash only staged changes
git stash --staged
```

### Listing and Viewing Stash
```bash
# Stash list
git stash list

# Stash details
git stash show
git stash show -p           # Patch format
git stash show stash@{0}

# Content of specific stash
git stash show -p stash@{2}
```

### Applying Stash
```bash
# Apply last stash (keeps in stash)
git stash apply

# Apply specific stash
git stash apply stash@{2}

# Apply last stash and remove from stash
git stash pop

# Apply and remove specific stash
git stash pop stash@{2}

# Create branch and apply stash
git stash branch new-branch
git stash branch new-branch stash@{1}
```

### Deleting Stash
```bash
# Delete last stash
git stash drop

# Delete specific stash
git stash drop stash@{2}

# Delete all stashes
git stash clear
```

---

## Log and History

### Viewing Log
```bash
# Standard log
git log

# One line
git log --oneline

# Graph view
git log --graph
git log --oneline --graph --all --decorate

# Last N commits
git log -5
git log -n 5

# With detailed diffs
git log -p
git log --patch

# With statistics
git log --stat

# Short summary
git log --shortstat

# Date range
git log --since="2 weeks ago"
git log --until="2024-12-31"
git log --after="2024-01-01" --before="2024-06-01"

# Filter by author
git log --author="Name"
git log --author="email@example.com"

# Search in commit messages
git log --grep="bugfix"
git log --grep="feature" --grep="hotfix" --all-match

# Search in file changes
git log -S"function_name"
git log -G"regex_pattern"

# History of specific file
git log -- file.txt
git log --follow -- file.txt  # Follow renames

# Branch comparison
git log main..feature           # In feature but not in main
git log feature..main           # In main but not in feature
git log main...feature          # Not in both

# Custom format
git log --pretty=format:"%h %an %ar %s"
git log --pretty=format:"%C(yellow)%h%Creset %s %C(cyan)(%cr)%Creset"
```

### Reflog (Reference History)
```bash
# Show all HEAD movements
git reflog

# Show last 10 movements
git reflog -10

# Branch-specific reflog
git reflog show feature-branch

# Recover lost commit
git reflog
git checkout abc1234  # commit found from reflog
```

### Blame (Line-by-Line History)
```bash
# Show last change for each line of file
git blame file.txt

# Specific line range
git blame -L 10,20 file.txt

# Show with email
git blame -e file.txt

# Shorter format
git blame -s file.txt
```

### Bisect (Binary Search for Bug)
```bash
# Start bisect
git bisect start

# Mark current commit as bad
git bisect bad

# Mark working commit as good
git bisect good abc1234

# Git automatically jumps to middle commit
# Test and mark:
git bisect good   # or
git bisect bad

# Finish
git bisect reset
```

---

## Tag Operations

### Creating Tags
```bash
# Lightweight tag
git tag v1.0.0

# Annotated tag (recommended)
git tag -a v1.0.0 -m "Version 1.0.0 release"

# Tag specific commit
git tag -a v0.9.0 abc1234 -m "Version 0.9.0"

# Signed tag (with GPG)
git tag -s v1.0.0 -m "Signed version 1.0.0"
```

### Listing Tags
```bash
# List all tags
git tag

# Filter with pattern
git tag -l "v1.*"
git tag --list "v1.*"

# Show tag details
git show v1.0.0
```

### Pushing Tags
```bash
# Push single tag
git push origin v1.0.0

# Push all tags
git push origin --tags
git push --tags

# Push annotated tags
git push --follow-tags
```

### Deleting Tags
```bash
# Delete local tag
git tag -d v1.0.0
git tag --delete v1.0.0

# Delete remote tag
git push origin --delete v1.0.0
git push origin :refs/tags/v1.0.0
```

### Checkout Tag
```bash
# Checkout tag (detached HEAD)
git checkout v1.0.0

# Create branch from tag
git checkout -b version-1.0 v1.0.0
```

---

## Advanced Commands

### Submodule
```bash
# Add submodule
git submodule add https://github.com/user/repo.git path/to/submodule

# Initialize and update submodules
git submodule init
git submodule update

# Or in single command
git submodule update --init --recursive

# Update all submodules
git submodule update --remote

# Show submodule status
git submodule status

# Remove submodule
git submodule deinit path/to/submodule
git rm path/to/submodule
```

### Worktree (Multiple Working Directories)
```bash
# Create new worktree
git worktree add ../new-folder feature-branch

# List worktrees
git worktree list

# Remove worktree
git worktree remove ../new-folder

# Prune worktree
git worktree prune
```

### Archive (Creating Archives)
```bash
# Create ZIP archive
git archive --format=zip --output=project.zip HEAD

# Create TAR archive
git archive --format=tar HEAD | gzip > project.tar.gz

# Archive from specific branch
git archive --format=zip --output=project.zip feature-branch

# Archive specific folder
git archive --format=zip --output=docs.zip HEAD:docs/
```

### Patch (Patch Files)
```bash
# Save last commit as patch file
git format-patch -1 HEAD

# Save last 3 commits as patches
git format-patch -3

# Patches between two commits
git format-patch abc1234..def5678

# Apply patch
git apply patch-file.patch

# Apply patch as commit
git am patch-file.patch
```

### Bundle (Offline Transfer)
```bash
# Create repository bundle
git bundle create repo.bundle --all

# Clone from bundle
git clone repo.bundle new-folder

# Verify bundle contents
git bundle verify repo.bundle

# Pull from bundle
git pull repo.bundle main
```

### Filter-Branch / Filter-Repo (History Cleanup)
```bash
# Remove file from entire history (old method - not recommended)
git filter-branch --tree-filter 'rm -f sensitive-file.txt' HEAD

# Better alternative: git-filter-repo (external tool)
git filter-repo --path file.txt --invert-paths

# Change author email
git filter-repo --email-callback 'return email.replace(b"old@email.com", b"new@email.com")'
```

### Grep (Code Search)
```bash
# Search in all files
git grep "search_term"

# With line numbers
git grep -n "search_term"

# Count occurrences
git grep -c "search_term"

# Search in specific file type
git grep "search_term" -- "*.js"

# Search with regex
git grep -E "pattern[0-9]+"

# Case-insensitive search
git grep -i "search_term"
```

### Sparse Checkout (Partial Checkout)
```bash
# Enable sparse checkout
git sparse-checkout init --cone

# Select specific folders
git sparse-checkout set src/ docs/

# Show list
git sparse-checkout list

# Disable
git sparse-checkout disable
```

### Notes (Commit Notes)
```bash
# Add note to commit
git notes add -m "Additional info" abc1234

# Show notes
git log --show-notes

# Edit note
git notes edit abc1234

# Delete note
git notes remove abc1234
```

---

## Useful Combinations

### Quick Workflow
```bash
# Quick add-commit
git commit -am "Message"

# Amend last commit and push
git commit --amend --no-edit && git push -f

# Create new branch, move changes
git stash
git checkout -b new-feature
git stash pop

# Merge and delete branch
git checkout main
git merge feature-branch
git branch -d feature-branch
git push origin --delete feature-branch

# Track remote branch
git checkout --track origin/feature-branch
```

### Cleanup and Maintenance
```bash
# Clean all merged branches
git branch --merged main | grep -v "^\*\|main" | xargs git branch -d

# Clean branches deleted on remote
git fetch --prune

# Git garbage collection
git gc

# Aggressive GC (more optimization)
git gc --aggressive --prune=now

# Check repository size
git count-objects -vH

# Reflog expire
git reflog expire --expire=now --all
```

### Comparison and Analysis
```bash
# Show differences between two branches
git diff main..feature-branch

# Which commits were merged
git log --merges

# Which commits were not merged
git log --no-merges

# Contributor statistics
git shortlog -sn

# Commit count by branch
git rev-list --count main
```

---

## .gitignore Templates

```bash
# Node.js
node_modules/
npm-debug.log
.env

# Python
__pycache__/
*.py[cod]
venv/
.env

# Java
*.class
target/
*.jar

# IDE
.vscode/
.idea/
*.swp
*.swo
.DS_Store

# Logs
*.log
logs/

# Temporary files
*.tmp
*.bak
*~

# OS
Thumbs.db
.DS_Store
```

---

## Git Alias Examples

### Aliases to Add to Configuration
```bash
# Short commands
git config --global alias.st status
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.ci commit
git config --global alias.unstage 'reset HEAD --'

# Log aliases
git config --global alias.lg "log --oneline --graph --all --decorate"
git config --global alias.last 'log -1 HEAD'
git config --global alias.hist "log --pretty=format:'%h %ad | %s%d [%an]' --graph --date=short"
git config --global alias.today "log --since='midnight' --author='$(git config user.name)' --oneline"

# Diff aliases
git config --global alias.df diff
git config --global alias.dc 'diff --cached'

# Branch management
git config --global alias.branches 'branch -a'
git config --global alias.remotes 'remote -v'
git config --global alias.tags 'tag -l'

# Stash aliases
git config --global alias.sl 'stash list'
git config --global alias.sa 'stash apply'
git config --global alias.ss 'stash save'

# Cleanup
git config --global alias.cleanup "!git branch --merged | grep -v '\\*\\|main\\|develop' | xargs -n 1 git branch -d"
git config --global alias.undo 'reset HEAD~1 --mixed'

# Advanced
git config --global alias.sync '!git fetch --all && git pull --rebase'
git config --global alias.amend 'commit --amend --no-edit'
git config --global alias.contributors 'shortlog -sn --all'
```

---

## Security and Best Practices

### Protecting Sensitive Information
```bash
# Completely remove a file from history
git filter-repo --path sensitive-file.txt --invert-paths

# Ignore .env file
echo ".env" >> .gitignore
git rm --cached .env
git commit -m "Remove .env from tracking"

# Git secrets setup (for AWS keys etc.)
git secrets --install
git secrets --register-aws
```

### GPG Signing
```bash
# Generate GPG key
gpg --gen-key

# Configure GPG key in Git
git config --global user.signingkey YOUR_GPG_KEY_ID

# Automatic signing
git config --global commit.gpgsign true

# Signed commit
git commit -S -m "Signed commit"

# Signed tag
git tag -s v1.0.0 -m "Signed tag"

# Verify signatures
git verify-commit abc1234
git verify-tag v1.0.0
```

### SSH Setup
```bash
# Generate SSH key
ssh-keygen -t ed25519 -C "email@example.com"

# Add to SSH agent
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519

# Add public key to GitHub
cat ~/.ssh/id_ed25519.pub

# Test SSH connection
ssh -T git@github.com
```

---

## Troubleshooting

### Common Problems and Solutions

#### Resolving Merge Conflicts
```bash
# Show conflicted files
git status

# Edit conflict markers
# <<<<<<< HEAD
# Your version
# =======
# Their version
# >>>>>>> branch-name

# Stage resolved file
git add conflicted-file.txt

# Complete merge
git commit

# Abort merge if needed
git merge --abort
```

#### Undo Pushed Commit
```bash
# Undo last commit (create revert commit)
git revert HEAD
git push

# Force reset (DANGEROUS - affects others)
git reset --hard HEAD~1
git push --force-with-lease
```

#### Recover Deleted Branch
```bash
# Find deleted branch in reflog
git reflog

# Restore branch
git checkout -b recovered-branch abc1234
```

#### Fix Detached HEAD
```bash
# Create branch from detached HEAD
git checkout -b new-branch

# Or return to branch
git checkout main
```

#### Large File Issues
```bash
# Remove large file from history
git filter-repo --path large-file.zip --invert-paths

# Use Git LFS for large files
git lfs install
git lfs track "*.zip"
git add .gitattributes
```

#### Permission Denied Errors
```bash
# Check SSH key
ssh -T git@github.com

# Regenerate SSH key if needed
ssh-keygen -t ed25519 -C "email@example.com"

# Change remote URL to SSH
git remote set-url origin git@github.com:user/repo.git
```

---

## Performance Tips

### Optimize Repository
```bash
# Garbage collection
git gc --aggressive --prune=now

# Repack repository
git repack -a -d --depth=250 --window=250

# Check repository size
git count-objects -vH

# Find large files
git rev-list --objects --all | git cat-file --batch-check='%(objecttype) %(objectname) %(objectsize) %(rest)' | sort -k3 -n | tail -10
```

### Shallow Clone for Large Repos
```bash
# Clone with limited history
git clone --depth 1 https://github.com/user/repo.git

# Deepen history later
git fetch --deepen=100

# Convert to full clone
git fetch --unshallow
```

---

## Collaboration Best Practices

### Commit Messages
```bash
# Good commit message format:
# <type>(<scope>): <subject>
#
# <body>
#
# <footer>

# Examples:
git commit -m "feat(auth): add login functionality"
git commit -m "fix(api): resolve null pointer exception"
git commit -m "docs(readme): update installation instructions"
git commit -m "refactor(utils): simplify date formatting"
git commit -m "test(auth): add unit tests for login"
```

### Pull Request Workflow
```bash
# Create feature branch
git checkout -b feature/new-feature

# Make changes and commit
git add .
git commit -m "feat: add new feature"

# Update with main branch
git fetch origin
git rebase origin/main

# Push to remote
git push -u origin feature/new-feature

# After PR approval, clean up
git checkout main
git pull
git branch -d feature/new-feature
```

### Code Review Tips
```bash
# View changes before pushing
git diff origin/main

# Review specific commit
git show abc1234

# Check what would be pushed
git log origin/main..HEAD

# Interactive rebase to clean commits
git rebase -i origin/main
```

---

## Emergency Commands

### Save Work Quickly
```bash
# Quick stash everything
git stash save "WIP: emergency fix"

# Create backup branch
git branch backup-$(date +%Y%m%d-%H%M%S)

# Create patch file
git diff > emergency-patch.diff
```

### Recover Lost Work
```bash
# Find lost commits
git reflog

# Recover deleted branch
git checkout -b recovered-branch abc1234

# Recover from dangling commits
git fsck --lost-found
```

### Fix Broken Repository
```bash
# Verify repository integrity
git fsck

# Reset to known good state
git reset --hard origin/main

# Clean everything
git clean -fdx

# Re-clone if necessary
cd ..
mv repo repo-backup
git clone <url> repo
```

---

## Git Hooks Examples

### Pre-commit Hook
```bash
# .git/hooks/pre-commit
#!/bin/sh
# Run tests before commit
npm test
if [ $? -ne 0 ]; then
    echo "Tests failed. Commit aborted."
    exit 1
fi
```

### Commit-msg Hook
```bash
# .git/hooks/commit-msg
#!/bin/sh
# Validate commit message format
commit_msg=$(cat "$1")
pattern="^(feat|fix|docs|style|refactor|test|chore)(\(.+\))?: .+"

if ! echo "$commit_msg" | grep -qE "$pattern"; then
    echo "Invalid commit message format"
    echo "Format: type(scope): subject"
    exit 1
fi
```

---

## Resources and Learning

### Official Documentation
- Git Official Documentation: https://git-scm.com/doc
- Git Book (Pro Git): https://git-scm.com/book/en/v2
- Git Reference: https://git-scm.com/docs

### Interactive Learning
- Learn Git Branching: https://learngitbranching.js.org/
- Git Immersion: https://gitimmersion.com/
- GitHub Learning Lab: https://lab.github.com/

### Useful Tools
- GitKraken: GUI client
- Sourcetree: Visual Git tool
- Git Extensions: Windows Git GUI
- Lazygit: Terminal UI for Git

---

## Quick Reference Card

### Most Used Commands
```bash
# Status and information
git status
git log --oneline --graph
git diff

# Basic workflow
git add .
git commit -m "message"
git push

# Branching
git checkout -b new-branch
git merge feature-branch
git branch -d old-branch

# Remote operations
git fetch
git pull
git push origin branch-name

# Undo changes
git restore file.txt
git reset HEAD~1
git revert abc1234

# Stashing
git stash
git stash pop
git stash list
```

### Keyboard Shortcuts (in terminals)
```bash
# Git command line shortcuts
Ctrl + R        # Search command history
Ctrl + A        # Go to line start
Ctrl + E        # Go to line end
Ctrl + U        # Clear line
Ctrl + W        # Delete word
Ctrl + C        # Cancel command
```

---

*This cheat sheet covers most Git commands and workflows. For detailed information, always refer to `git help <command>` or the official Git documentation.*

**Remember:** Always commit often, write meaningful commit messages, and pull before you push!
