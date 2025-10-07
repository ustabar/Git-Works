# Git Stash - Complete Guide

## 📚 Table of Contents
- [What is Git Stash?](#what-is-git-stash)
- [Why Use Stash?](#why-use-stash)
- [How Stash Works](#how-stash-works)
- [Common Scenarios](#common-scenarios)
- [Basic Commands](#basic-commands)
- [Advanced Usage](#advanced-usage)
- [Best Practices](#best-practices)
- [Troubleshooting](#troubleshooting)

---

## What is Git Stash?

**Git Stash** is a powerful feature that allows you to temporarily save (or "stash") your uncommitted changes without committing them. Think of it as a **clipboard for your work in progress**.

### The Problem It Solves

Imagine you're working on a feature, but suddenly you need to:
- Switch to another branch to fix an urgent bug
- Pull latest changes from remote
- Work on a different task
- Test something in a clean state

**Without stash:** You'd be forced to either commit incomplete work or lose your changes.

**With stash:** You can save your changes temporarily, switch contexts, and restore them later.

---

## Why Use Stash?

### 1. **Context Switching**
Switch between tasks without losing work or creating messy commits.

```bash
# Working on feature-A
# Urgent bug needs fixing!

git stash                    # Save current work
git checkout main            # Switch branch
# Fix the bug
git checkout feature-A       # Return to feature
git stash pop               # Restore your work
```

### 2. **Clean Working Directory**
Get a clean slate when needed without losing changes.

```bash
# Your working directory has changes
git status
# modified: file1.js
# modified: file2.js

git stash                    # Now working directory is clean
# Test something or pull updates
git stash pop               # Get your changes back
```

### 3. **Experiment Safely**
Try different approaches without commitment.

```bash
# You have a working solution
git stash                    # Save current approach

# Try alternative solution
# ... make changes ...

# Doesn't work? No problem!
git checkout .              # Discard experiment
git stash pop              # Restore original work
```

### 4. **Pull with Uncommitted Changes**
Pull remote changes when you have local modifications.

```bash
# You have uncommitted changes
# Need to pull from remote

git stash                    # Save your changes
git pull                     # Pull updates
git stash pop               # Reapply your changes
```

---

## How Stash Works

### The Stash Stack

Stashes are stored in a **stack** (last-in, first-out):

```
┌─────────────────────────┐
│  stash@{0} (newest)     │  ← Top of stack
├─────────────────────────┤
│  stash@{1}              │
├─────────────────────────┤
│  stash@{2}              │
├─────────────────────────┤
│  stash@{3} (oldest)     │  ← Bottom of stack
└─────────────────────────┘
```

### What Gets Stashed?

**By default:**
- ✅ Modified tracked files
- ✅ Staged changes

**Not included by default:**
- ❌ Untracked files (new files)
- ❌ Ignored files (.gitignore)

**You can include them with flags:**
```bash
git stash -u              # Include untracked files
git stash -a              # Include all files (even ignored)
```

---

## Common Scenarios

### Scenario 1: Emergency Bug Fix

```bash
# You're working on feature-login branch
# Making changes to login.js and styles.css

# 🚨 Emergency: Critical bug in production!

# Save your work
git stash save "WIP: login form validation"

# Switch to main and fix bug
git checkout main
git checkout -b hotfix/critical-bug
# ... fix the bug ...
git commit -m "fix: critical security issue"
git push origin hotfix/critical-bug

# Return to your feature
git checkout feature-login
git stash pop

# Continue working where you left off
```

### Scenario 2: Switching Tasks

```bash
# Working on Task A
git stash save "Task A: halfway through implementation"

# Work on Task B
git checkout task-b-branch
# ... complete Task B ...
git commit -m "Complete Task B"

# Back to Task A
git checkout task-a-branch
git stash pop
# Continue Task A
```

### Scenario 3: Pull Latest Changes

```bash
# You have uncommitted changes
git status
# modified: config.js

# Want to pull latest from remote
git stash                    # Save changes
git pull origin main         # Pull updates
git stash pop               # Restore your changes

# If conflicts occur, resolve them
# Your changes are preserved!
```

### Scenario 4: Testing Clean State

```bash
# Made changes, want to test without them
git stash

# Run tests in clean state
npm test

# Changes didn't break anything?
git stash pop               # Restore and continue

# Or changes broke something?
git stash drop             # Discard changes
```

---

## Basic Commands

### Creating Stashes

```bash
# Stash all changes (simplest)
git stash

# Stash with descriptive message
git stash save "Feature: add user authentication"

# Stash including untracked files
git stash -u
git stash --include-untracked

# Stash everything (including ignored files)
git stash -a
git stash --all

# Stash only staged changes
git stash --staged

# Stash with keep index (keep staged files)
git stash --keep-index
```

### Viewing Stashes

```bash
# List all stashes
git stash list
# Output:
# stash@{0}: WIP on main: abc1234 Last commit
# stash@{1}: On feature: def5678 Another stash
# stash@{2}: WIP on develop: ghi9012 Old stash

# Show stash contents (summary)
git stash show
git stash show stash@{1}

# Show stash contents (full diff)
git stash show -p
git stash show -p stash@{2}

# Show stash in patch format
git stash show --patch stash@{0}
```

### Applying Stashes

```bash
# Apply last stash (keeps it in stash list)
git stash apply

# Apply specific stash
git stash apply stash@{2}

# Apply last stash and remove it from list
git stash pop

# Apply specific stash and remove it
git stash pop stash@{1}
```

### Deleting Stashes

```bash
# Delete last stash
git stash drop

# Delete specific stash
git stash drop stash@{2}

# Delete all stashes (CAREFUL!)
git stash clear
```

---

## Advanced Usage

### Creating Branch from Stash

When stash conflicts with current branch, create a new branch:

```bash
# Stash conflicts when applying?
git stash branch new-branch-name

# Or from specific stash
git stash branch new-branch-name stash@{1}

# This:
# 1. Creates new branch
# 2. Checks out the commit where stash was created
# 3. Applies stash
# 4. Drops stash if successful
```

### Partial Stashing (Interactive)

Stash only specific changes:

```bash
# Interactive stash
git stash -p

# For each change, Git asks:
# y - stash this hunk
# n - don't stash this hunk
# q - quit (don't stash remaining hunks)
# a - stash this and all remaining hunks
# d - don't stash this or remaining hunks
# s - split into smaller hunks
# e - manually edit hunk
```

### Stashing Untracked Files Selectively

```bash
# Add specific untracked files first
git add new-file.js

# Then stash only staged files
git stash --staged

# Or stash everything including untracked
git stash -u
```

### Viewing Stash as Diff

```bash
# Compare stash with current working directory
git diff stash@{0}

# Compare stash with specific commit
git diff abc1234 stash@{0}

# Show what's in stash compared to parent
git stash show -p stash@{1}
```

### Applying Stash to Different Branch

```bash
# Create stash on branch-A
git checkout branch-A
git stash save "Changes from branch-A"

# Apply to branch-B
git checkout branch-B
git stash apply stash@{0}

# Stash is branch-independent!
```

---

## Best Practices

### 1. **Use Descriptive Messages**

❌ Bad:
```bash
git stash
git stash save "stuff"
```

✅ Good:
```bash
git stash save "WIP: user authentication - login form validation"
git stash save "Bug fix: cart calculation issue - needs testing"
```

### 2. **Don't Let Stashes Accumulate**

```bash
# Check stash list regularly
git stash list

# Clean old stashes
git stash drop stash@{5}
git stash clear  # If starting fresh
```

### 3. **Use Pop vs Apply Appropriately**

**Use `pop`** when:
- You're sure you want to apply changes
- You won't need the stash again
- Keeping stash list clean

**Use `apply`** when:
- You want to try changes on multiple branches
- You're not sure if changes will work
- You want to keep stash as backup

```bash
git stash apply  # Keep stash for later
# Test changes
# If good:
git stash drop   # Manually remove

# Or just use pop
git stash pop    # Apply and remove in one step
```

### 4. **Stash Before Pulling**

```bash
# Good workflow
git stash
git pull
git stash pop

# Or use rebase
git stash
git pull --rebase
git stash pop
```

### 5. **Use Stash for Quick Backups**

```bash
# Before risky operation
git stash save "Backup before major refactor"

# Do risky changes
# ...

# If something goes wrong
git stash pop    # Restore backup
```

### 6. **Don't Use Stash as Long-term Storage**

❌ Don't do this:
```bash
# Storing work for days/weeks in stash
git stash save "Feature from 2 months ago"
```

✅ Do this instead:
```bash
# Create a proper branch
git checkout -b feature/temporary-work
git add .
git commit -m "WIP: temporary work"
```

---

## Troubleshooting

### Problem 1: Conflicts When Applying Stash

```bash
# Stash pop causes conflicts
git stash pop
# CONFLICT (content): Merge conflict in file.js

# Option 1: Resolve conflicts manually
# Edit conflicted files
git add file.js
# Stash is automatically dropped after resolution

# Option 2: Abort and keep stash
git reset --merge
# Your stash is still in the list
git stash list
```

### Problem 2: Accidentally Dropped Stash

```bash
# Find lost stash in dangling commits
git fsck --unreachable | grep commit

# Or use reflog
git reflog

# Recover stash
git stash apply <commit-hash>

# Create branch from lost stash
git branch recover-stash <commit-hash>
```

### Problem 3: Stash Not Including All Files

```bash
# Untracked files not stashed?
git stash -u              # Include untracked

# Ignored files not stashed?
git stash -a              # Include everything

# Staged files removed from stage?
git stash --keep-index    # Keep staged files
```

### Problem 4: Can't Switch Branches

```bash
# Error: Your local changes would be overwritten
error: Your local changes to the following files would be overwritten by checkout:
        file.js
Please commit your changes or stash them before you switch branches.

# Solution
git stash
git checkout other-branch
git stash pop
```

### Problem 5: Stash List Too Long

```bash
# View all stashes
git stash list

# Delete old stashes one by one
git stash drop stash@{5}
git stash drop stash@{6}

# Or clear all (CAREFUL!)
git stash clear
```

---

## Stash vs Other Approaches

### Stash vs Commit

| Feature | Stash | Commit |
|---------|-------|--------|
| Temporary | ✅ Yes | ❌ No |
| In history | ❌ No | ✅ Yes |
| Easy to discard | ✅ Yes | ❌ Harder |
| Best for | Quick saves | Permanent saves |

**When to use:**
- **Stash:** Temporary, incomplete work, context switching
- **Commit:** Complete, working changes you want to keep

### Stash vs WIP Commit

Some developers use "WIP" commits instead of stash:

```bash
# WIP commit approach
git add .
git commit -m "WIP: incomplete feature"

# Later
git reset HEAD~1  # Undo commit
```

**Pros of WIP commits:**
- Part of history
- Can push to remote
- Can share with team

**Pros of stash:**
- Cleaner history
- Designed for this purpose
- Easier to manage

**Recommendation:** Use stash for local temporary saves, use WIP commits if you need to share or backup to remote.

---

## Real-World Examples

### Example 1: Multi-tasking Developer

```bash
# Morning: Start feature A
git checkout -b feature/user-profile
# ... write code ...

# 10 AM: Manager asks to review PR
git stash save "Profile feature: 40% complete"
git checkout develop
# ... review PR ...

# 11 AM: Back to feature A
git checkout feature/user-profile
git stash pop

# 2 PM: Need to demo another feature
git stash save "Profile feature: 70% complete"
git checkout feature/other-feature
# ... demo ...

# 3 PM: Back to feature A
git checkout feature/user-profile
git stash pop
# ... finish feature ...
git add .
git commit -m "feat: add user profile page"
```

### Example 2: Experimental Development

```bash
# Current approach works but is slow
git stash save "Working solution - performance issue"

# Try optimization
# ... make changes ...
# Run benchmarks

# Slower? Revert
git reset --hard HEAD
git stash pop

# Or faster? Keep it
git stash drop
git commit -m "perf: optimize database queries"
```

### Example 3: Team Collaboration

```bash
# Working on feature, teammate needs help
git stash save "Feature X: implementing validation"

# Help teammate
git checkout teammate-branch
# ... help debug ...

# Quick sync with remote
git checkout main
git pull

# Back to your work
git checkout your-branch
git stash pop
```

---

## Keyboard Shortcuts & Aliases

### Useful Aliases

Add these to your `.gitconfig`:

```bash
[alias]
    # Stash shortcuts
    st = stash
    sta = stash save
    stl = stash list
    stp = stash pop
    sts = stash show -p
    
    # Advanced stash
    stash-all = stash save --include-untracked
    stash-staged = stash --staged
    stash-keep = stash --keep-index
    
    # Stash management
    stash-clear-old = "!git stash list | tail -n +6 | cut -d: -f1 | xargs -n1 git stash drop"
```

Usage:
```bash
git sta "My work in progress"
git stl
git stp
```

---

## Cheat Sheet

### Quick Reference

```bash
# Create
git stash                         # Stash changes
git stash -u                      # Include untracked
git stash save "message"          # With message

# View
git stash list                    # List all
git stash show                    # Show last
git stash show -p                 # Show with diff

# Apply
git stash apply                   # Apply (keep in list)
git stash pop                     # Apply (remove from list)
git stash apply stash@{2}         # Apply specific

# Delete
git stash drop                    # Delete last
git stash drop stash@{1}          # Delete specific
git stash clear                   # Delete all

# Advanced
git stash branch name             # Create branch from stash
git stash -p                      # Interactive stash
```

---

## Summary

**Git Stash** is your temporary workspace clipboard. Use it to:

✅ Switch contexts quickly  
✅ Keep working directory clean  
✅ Experiment safely  
✅ Handle interruptions gracefully  
✅ Pull changes when you have uncommitted work  

**Remember:**
- Stash is temporary, not for long-term storage
- Use descriptive messages
- Clean up old stashes regularly
- Prefer `pop` over `apply` + `drop`
- When in doubt, stash it out!

**Pro tip:** Master stash, and you'll handle interruptions and context switches like a pro! 🚀

---

*For more information, see: `git help stash` or visit [Git Documentation](https://git-scm.com/docs/git-stash)*
