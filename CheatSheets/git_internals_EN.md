# Git Internals - Deep Dive Guide

## 📋 Table of Contents
- [Introduction](#introduction)
- [Git Architecture Overview](#git-architecture-overview)
- [The Three States](#the-three-states)
- [Git Object Model](#git-object-model)
- [Branches and HEAD](#branches-and-head)
- [Commit History and DAG](#commit-history-and-dag)
- [Merging Strategies](#merging-strategies)
- [Rebasing Internals](#rebasing-internals)
- [Remote Repository Interaction](#remote-repository-interaction)
- [Understanding References](#understanding-references)
- [Advanced Concepts](#advanced-concepts)

---

## Introduction

This guide provides an in-depth look at **Git's internal workings** through visual diagrams. Understanding these internals will help you:

- 🎯 **Make better decisions** when resolving conflicts
- 🔍 **Debug issues** more effectively
- 💡 **Understand commands** at a deeper level
- 🚀 **Use Git confidently** in complex scenarios
- 📚 **Learn advanced workflows** with ease

Git is not just a version control system—it's a **content-addressable filesystem** with a version control interface built on top of it.

---

## Git Architecture Overview

### Step 1: Local vs Remote Repository Architecture

![Step-01](Diagrams/Step-01.png)

**What This Diagram Shows:**

This diagram illustrates the fundamental architecture of Git, showing the complete workflow from working directory to remote repository, including the crucial staging area.

**Key Components:**

1. **Working Directory** (Local Machine)
   - Your actual project files
   - Where you edit and create files
   - Contains modified files (unstaged)
   - Not managed by Git until you stage them

2. **Staging Area / Index** (Local Machine)
   - Intermediate layer between working directory and repository
   - Snapshot of what will be committed next
   - Allows selective commits
   - Located in `.git/index` file

3. **Local Repository** (.git folder)
   - Complete history of your project
   - All branches and commits
   - Stored on your computer
   - Contains the Git object database

4. **Remote Repository** (GitHub/GitLab/etc.)
   - Shared repository on a server
   - Central hub for collaboration
   - Backup of your work
   - Synchronized via push/pull operations

**Complete Git Command Flow:**

```
Working Directory  →  Staging Area  →  Local Repository  →  Remote Repository
    (edit)        →   (git add)    →   (git commit)     →   (git push)
                  ←                ←                    ←
              (git restore)    (git reset)        (git pull/fetch)
```

**Detailed Command Flow:**

```bash
# 1. EDIT - Modify files in working directory
echo "Hello Git" > file.txt
# Status: Modified (unstaged)

# 2. STAGE - Add to staging area
git add file.txt
# Status: Staged (ready to commit)

# 3. COMMIT - Save to local repository
git commit -m "Add greeting"
# Status: Committed (in local history)

# 4. PUSH - Upload to remote repository
git push origin main
# Status: Synchronized with remote

# REVERSE FLOW:
# Pull changes from remote
git pull origin main     # Remote → Local Repository + Working Directory
git fetch origin        # Remote → Local Repository only

# Unstage changes
git restore --staged file.txt  # Staging → Working Directory
git reset HEAD file.txt        # Staging → Working Directory (old way)

# Discard working changes
git restore file.txt           # Discard changes in working directory
git checkout -- file.txt       # Discard changes (old way)
```

**Visual Architecture:**

```
┌─────────────────────────────────────────────────────────────────┐
│                        LOCAL MACHINE                             │
│                                                                   │
│  ┌──────────────┐    ┌──────────────┐    ┌──────────────┐      │
│  │   Working    │ →  │   Staging    │ →  │    Local     │      │
│  │  Directory   │    │     Area     │    │  Repository  │      │
│  │              │    │   (Index)    │    │  (.git dir)  │      │
│  │  file.txt    │    │  file.txt    │    │  Commits     │      │
│  │  (modified)  │    │  (staged)    │    │  Branches    │      │
│  └──────────────┘    └──────────────┘    └──────┬───────┘      │
│                                                   │               │
└───────────────────────────────────────────────────┼──────────────┘
                                                    │
                                                    │ push/pull
                                                    │ fetch/clone
                                                    ▼
                                    ┌───────────────────────────┐
                                    │    REMOTE REPOSITORY      │
                                    │    (GitHub/GitLab)        │
                                    │                           │
                                    │    origin/main            │
                                    │    origin/feature         │
                                    │    All commits            │
                                    └───────────────────────────┘
```

**Workflow Examples:**

**Example 1: Complete Workflow**
```bash
# 1. Clone remote repository
git clone https://github.com/user/repo.git
# Downloads: Remote → Local Repository → Working Directory

# 2. Make changes
echo "new feature" >> app.js
# File modified in working directory

# 3. Check status
git status
# modified: app.js (not staged)

# 4. Stage changes
git add app.js
# Moved: Working Directory → Staging Area

# 5. Verify staged changes
git diff --staged
# Shows what will be committed

# 6. Commit to local repository
git commit -m "feat: add new feature"
# Moved: Staging Area → Local Repository

# 7. Push to remote
git push origin main
# Moved: Local Repository → Remote Repository
```

**Example 2: Working Offline**
```bash
# Your local repository is independent
# You can work offline completely

# Make changes (offline)
git add feature1.js
git commit -m "Add feature 1"

git add feature2.js
git commit -m "Add feature 2"

git add bugfix.js
git commit -m "Fix critical bug"

# All commits stored in LOCAL repository
# No internet needed!

# Later, when online, sync with remote
git push origin main
# All 3 commits uploaded at once
```

**Example 3: Selective Staging**
```bash
# Modified multiple files
touch file1.txt file2.txt file3.txt

# Stage selectively
git add file1.txt file2.txt
git commit -m "Add files 1 and 2"

# file3.txt still in working directory
git add file3.txt
git commit -m "Add file 3"
```

**Why This Architecture Matters:**

1. **Distributed Nature**
   - Git is **distributed** - every developer has a full copy
   - No single point of failure
   - Work without internet connection
   - Fast operations (everything is local)

2. **Three-Layer Flexibility**
   - **Working Directory:** Experiment freely
   - **Staging Area:** Prepare perfect commits
   - **Local Repository:** Full version history
   - **Remote Repository:** Collaboration and backup

3. **Offline Capabilities**
   ```bash
   # All these work offline:
   git log              # View history
   git diff             # Compare changes
   git branch           # Create branches
   git commit           # Save commits
   git merge            # Merge branches
   
   # Only these need internet:
   git push             # Upload to remote
   git pull             # Download from remote
   git fetch            # Fetch remote changes
   git clone            # Clone repository
   ```

4. **Safety Through Stages**
   ```bash
   # Layer 1: Working Directory (easily discarded)
   git restore file.txt
   
   # Layer 2: Staging Area (can unstage)
   git restore --staged file.txt
   
   # Layer 3: Local Repository (permanent, but local)
   git reset HEAD~1
   
   ```

**Common Patterns:**

```bash
# Pattern 1: Quick commit (skip staging view)
git commit -am "Quick fix"
# -a automatically stages tracked files
# -m provides commit message

# Pattern 2: Careful commit (review staging)
git add file1.js
git diff --staged           # Review what's staged
git add file2.js
git diff --staged           # Review again
git commit -m "Reviewed changes"

# Pattern 3: Fetch before pull
git fetch origin            # Download to local repository
git diff main origin/main   # Compare local vs remote
git merge origin/main       # Integrate if satisfied

# Pattern 4: Stash for context switching
git stash                   # Save working directory + staging
git checkout other-branch   # Switch context
git checkout main           # Return
git stash pop              # Restore working directory + staging
```

---

## The Three States

### Step 2: Working Directory, Staging Area, and Repository

![Step-02](Diagrams/Step-02.png)

**What This Diagram Shows:**

The three fundamental states in Git's workflow and how files move between them.

**The Three States:**

1. **Working Directory (Modified)**
   ```
   You edit files here
   Status: Modified (but not staged)
   ```

2. **Staging Area / Index (Staged)**
   ```
   Files ready to be committed
   Status: Staged (ready for commit)
   ```

3. **Git Repository (Committed)**
   ```
   Permanently stored in history
   Status: Committed (saved in database)
   ```

**File Lifecycle:**

```bash
# 1. Modify file (Working Directory)
echo "Hello World" > file.txt
git status
# modified: file.txt

# 2. Stage file (Staging Area)
git add file.txt
git status
# Changes to be committed:
#   modified: file.txt

# 3. Commit file (Repository)
git commit -m "Add hello world"
git status
# nothing to commit, working tree clean
```

**Visual Flow:**

```
Working Directory  →  Staging Area  →  Repository
    (edit)        →   (git add)    →  (git commit)
                  ←                ←
              (git restore)    (git reset)
```

**Why Three States?**

- **Flexibility:** Choose exactly what to commit
- **Atomic commits:** Group related changes
- **Review:** Check staged changes before committing
- **Partial commits:** Stage only parts of files

**Commands for Each State:**

```bash
# View each state
git diff              # Working vs Staging
git diff --staged     # Staging vs Repository
git diff HEAD         # Working vs Repository

# Move between states
git add file.txt      # Working → Staging
git commit            # Staging → Repository
git restore file.txt  # Unstage (new command)
git reset HEAD file   # Unstage (old command)
```

---

### Step 3: Understanding the Staging Area

![Step-03](Diagrams/Step-03.png)

**What This Diagram Shows:**

A detailed view of how the staging area (index) works as an intermediary layer between your working directory and the repository.

**The Staging Area Explained:**

The staging area is like a **preparation area** or **draft space** where you assemble your next commit.

**Why It Exists:**

1. **Selective Committing**
   ```bash
   # Modified 5 files, but only want to commit 2
   git add feature.js utils.js
   git commit -m "Add new feature"
   
   # Other 3 files remain in working directory
   ```

2. **Logical Grouping**
   ```bash
   # Group related changes
   git add auth/*.js
   git commit -m "Update authentication"
   
   git add api/*.js
   git commit -m "Update API endpoints"
   ```

3. **Partial File Staging**
   ```bash
   # Stage only specific changes in a file
   git add -p database.js
   # y = stage this hunk
   # n = don't stage this hunk
   # s = split into smaller hunks
   ```

**Staging Area as a Snapshot:**

```
Working Directory:        Staging Area:           Repository:
file1.txt (v3)    →      file1.txt (v2)    →    file1.txt (v1)
file2.txt (v2)    →      file2.txt (v2)    →    file2.txt (v1)
file3.txt (v1)    →      [not staged]      →    [doesn't exist]
```

**Practical Example:**

```bash
# Scenario: You fixed a bug and added a feature
# File: app.js has both changes

# Stage only the bug fix part
git add -p app.js
# Choose 'y' for bug fix hunks
# Choose 'n' for feature hunks

git commit -m "fix: resolve login timeout"

# Now stage the feature
git add app.js
git commit -m "feat: add user profile"
```

**Internal Representation:**

The staging area is actually a **binary file** at `.git/index` that contains:
- File paths
- File metadata (permissions, timestamps)
- SHA-1 hashes pointing to blob objects
- Staging flags

```bash
# Peek inside the index
git ls-files --stage
# 100644 a906cb2a4a904a152e80877d4088654daad0c859 0	README.md
# 100644 8f94139338f9404f26296befa88755fc2598c289 0	index.js
```

---

## Git Object Model

### Step 4: Blob Objects (File Contents)

![Step-04](Diagrams/Step-04.png)

**What This Diagram Shows:**

The fundamental building block of Git's object database - the **blob** (binary large object).

**What is a Blob?**

A blob stores the **contents** of a file. It's the most basic unit of storage in Git.

**Key Characteristics:**

- Stores **only file contents** (no filename, no metadata)
- Identified by **SHA-1 hash** of its contents
- Immutable (once created, never changes)
- Compressed with zlib

**How Blobs Work:**

```bash
# Create a file
echo "Hello, Git!" > greeting.txt

# Git computes SHA-1 hash
# SHA-1("Hello, Git!") = 8ab686eafeb1f44702738c8b0f24f2567c36da6d

# When you add the file
git add greeting.txt

# Git creates a blob object
# Stored in: .git/objects/8a/b686eafeb1f44702738c8b0f24f2567c36da6d
```

**Important: Content-Addressable Storage**

```bash
# Same content = Same hash = Same blob
echo "Hello, Git!" > file1.txt
echo "Hello, Git!" > file2.txt

git add file1.txt file2.txt
# Both files point to the SAME blob object!
# Git deduplicates automatically
```

**Viewing Blob Objects:**

```bash
# Get blob hash
git hash-object greeting.txt
# 8ab686eafeb1f44702738c8b0f24f2567c36da6d

# View blob contents
git cat-file -p 8ab686ea
# Hello, Git!

# View blob type
git cat-file -t 8ab686ea
# blob

# View blob size
git cat-file -s 8ab686ea
# 12
```

**Blob Limitations:**

Blobs store **only content**, NOT:
- ❌ Filename
- ❌ Directory structure
- ❌ Permissions
- ❌ Timestamps

**Example:**

```bash
# These create the same blob
echo "test" > a.txt
echo "test" > b.txt
echo "test" > dir/c.txt

git add .
# All three files share ONE blob object
# Filename/path stored in tree object (next step)
```

---

### Step 5: Tree Objects (Directory Structure)

![Step-05](Diagrams/Step-05.png)

**What This Diagram Shows:**

How Git stores directory structure using **tree objects**.

**What is a Tree?**

A tree object represents a **directory**. It contains:
- Pointers to blobs (files)
- Pointers to other trees (subdirectories)
- Metadata (filenames, permissions)

**Tree Structure:**

```
Tree (root directory)
├── blob: README.md (100644)
├── blob: index.js (100644)
└── tree: src/
    ├── blob: main.js (100644)
    └── blob: utils.js (100644)
```

**Real Example:**

```bash
# Project structure:
my-project/
├── README.md        "# My Project"
├── app.js           "console.log('Hello');"
└── lib/
    └── helper.js    "module.exports = {}"

# Git creates:
# - 3 blob objects (one for each file content)
# - 2 tree objects (root directory + lib directory)

# Root tree contains:
# - blob pointer to README.md
# - blob pointer to app.js
# - tree pointer to lib/

# lib/ tree contains:
# - blob pointer to helper.js
```

**Viewing Tree Objects:**

```bash
# Get tree of current commit
git cat-file -p HEAD^{tree}

# Output example:
# 100644 blob 8ab686ea    README.md
# 100644 blob 7c4a013d    app.js
# 040000 tree a1b2c3d4    lib

# View subdirectory tree
git cat-file -p a1b2c3d4
# 100644 blob e5f6g7h8    helper.js
```

**File Modes:**

```
100644 - Regular file
100755 - Executable file
040000 - Directory (tree)
120000 - Symbolic link
160000 - Gitlink (submodule)
```

**Trees are Immutable:**

```bash
# Change a file
echo "new content" > README.md
git add README.md

# Git creates:
# 1. New blob for new content
# 2. New tree for root directory
# 3. Old blob and old tree still exist!

# This is why Git is so fast at branching
```

**Efficiency:**

```bash
# If only one file changes:
my-project/
├── README.md        (changed - new blob)
├── app.js           (unchanged - same blob)
└── lib/             (unchanged - same tree!)
    └── helper.js    (unchanged - same blob)

# Git reuses:
# - Old blob for app.js
# - Old tree for lib/
# - Old blob for helper.js

# Creates only:
# - New blob for README.md
# - New root tree
```

---

### Step 6: Commit Objects (Snapshots)

![Step-06](Diagrams/Step-06.png)

**What This Diagram Shows:**

How **commit objects** tie everything together to create snapshots of your project.

**What is a Commit?**

A commit is a **snapshot** of your entire project at a specific point in time.

**Commit Object Contains:**

```
commit {
    tree:      sha1-of-root-tree
    parent:    sha1-of-parent-commit
    author:    name <email> timestamp
    committer: name <email> timestamp
    message:   "Commit message"
}
```

**Anatomy of a Commit:**

```bash
# View commit object
git cat-file -p HEAD

# Output:
tree 4b825dc642cb6eb9a060e54bf8d69288fbee4904
parent 1a410efbd13591db07496601ebc7a059dd55cfe9
author John Doe <john@example.com> 1699564800 +0000
committer John Doe <john@example.com> 1699564800 +0000

Add user authentication feature

# Detailed description here
```

**Commit Chain:**

```
         ┌─────────────┐
         │  Commit C   │
         │  tree: T3   │
         │  parent: B  │
         └──────┬──────┘
                │
         ┌──────▼──────┐
         │  Commit B   │
         │  tree: T2   │
         │  parent: A  │
         └──────┬──────┘
                │
         ┌──────▼──────┐
         │  Commit A   │
         │  tree: T1   │
         │  parent: -  │ (initial commit)
         └─────────────┘
```

**Complete Object Graph:**

```
Commit ──────► Tree (root)
  │              ├──► Blob (file1.txt)
  │              ├──► Blob (file2.js)
  │              └──► Tree (src/)
  │                     ├──► Blob (main.js)
  │                     └──► Blob (utils.js)
  │
  └────parent──► Previous Commit
```

**Real Example:**

```bash
# Make a commit
echo "Hello" > test.txt
git add test.txt
git commit -m "Add test file"

# Git creates:
# 1. Blob object (content: "Hello")
# 2. Tree object (points to blob, stores filename)
# 3. Commit object (points to tree, has metadata)

# View the chain
git log --oneline --graph
# * d3a5f21 (HEAD -> main) Add test file
# * 8c3f0b1 Previous commit
# * 1a2b3c4 Initial commit
```

**Why Commits Point to Trees:**

```bash
# Commit doesn't store individual files
# It stores ONE tree (root directory)
# Tree recursively contains everything

Commit
  └── Tree (project root)
      ├── README.md (blob)
      ├── src/ (tree)
      │   ├── index.js (blob)
      │   └── utils.js (blob)
      └── tests/ (tree)
          └── test.js (blob)

# ONE commit pointer gives you ENTIRE snapshot
```

**Commit SHA-1 Hash:**

```bash
# Hash includes:
# - Tree hash
# - Parent hash
# - Author info
# - Timestamp
# - Commit message

# Change ANY of these → Different commit hash
# Same content, different timestamp → Different hash!
```

**Special Commits:**

```bash
# Initial commit (no parent)
git cat-file -p <first-commit>
# tree abc123...
# author ...
# (no parent field)

# Merge commit (multiple parents)
git cat-file -p <merge-commit>
# tree def456...
# parent abc123...  (from main)
# parent xyz789...  (from feature)
# merge: Merge branch 'feature'
```

---

## Branches and HEAD

### Step 7: Branches are Just Pointers

![Step-07](Diagrams/Step-07.png)

**What This Diagram Shows:**

The crucial concept that **branches are simply movable pointers** to commits.

**What is a Branch?**

A branch is just a **41-byte file** containing a SHA-1 hash of a commit.

```bash
# Branch file content
cat .git/refs/heads/main
# d3a5f21b8c9e7a6f5d4c3b2a1e9f8d7c6b5a4e3d

# That's it! Just a commit hash.
```

**Branch as Pointer:**

```
Commits:  C1 ← C2 ← C3 ← C4 ← C5
                          ↑
                        main (branch pointer)
```

**Creating a Branch:**

```bash
# Create new branch
git branch feature

# Git simply creates a new file:
# .git/refs/heads/feature
# containing the same commit hash as current branch

Before:
    C1 ← C2 ← C3 ← C4 ← C5
                      ↑
                    main

After:
    C1 ← C2 ← C3 ← C4 ← C5
                      ↑
                    main, feature (both point to C5)
```

**Working with Branches:**

```bash
# Create branch
git branch feature
# .git/refs/heads/feature created

# Switch to branch
git checkout feature
# HEAD now points to feature

# Make a commit
git commit -m "New feature"
# feature pointer moves forward
# main stays where it was

Result:
    C1 ← C2 ← C3 ← C4 ← C5 ← C6
                      ↑     ↑
                    main  feature
```

**Why Branching is Fast:**

```bash
# Creating a branch in Git:
# 1. Create 41-byte file with commit hash
# 2. Done!

# In other VCS (like SVN):
# 1. Copy entire directory
# 2. Wait... and wait...

# Git branches are INSTANT regardless of project size!
```

---

### Step 8: HEAD Pointer

![Step-08](Diagrams/Step-08.png)

**What This Diagram Shows:**

How **HEAD** works as a pointer to the current branch.

**What is HEAD?**

HEAD is a **special pointer** that tells Git:
- Where you currently are
- What commit you're looking at
- What branch you're working on

**HEAD Points to Branch:**

```bash
# Normal state (HEAD → branch → commit)
cat .git/HEAD
# ref: refs/heads/main

# HEAD points to main branch
# main branch points to a commit
```

**Visual Representation:**

```
                    HEAD
                     ↓
                   main
                     ↓
C1 ← C2 ← C3 ← C4 ← C5
```

**HEAD Movement:**

```bash
# Switch branches
git checkout feature

Before:
    HEAD → main → C5

After:
    HEAD → feature → C5

# Now commits will move feature pointer
```

**Detached HEAD State:**

```bash
# Checkout a specific commit (not a branch)
git checkout d3a5f21

# HEAD now points directly to commit
cat .git/HEAD
# d3a5f21b8c9e7a6f5d4c3b2a1e9f8d7c6b5a4e3d

Detached HEAD:
    HEAD (not on any branch)
     ↓
C1 ← C2 ← C3
         ↑
       main
```

**Why Detached HEAD?**

```bash
# Useful for:
# 1. Exploring old commits
git checkout HEAD~5

# 2. Temporary experiments
git checkout abc123
# make changes, test
git checkout main  # abandon changes

# 3. Creating branch from old commit
git checkout abc123
git checkout -b hotfix
# Now HEAD → hotfix → abc123
```

**HEAD Shortcuts:**

```bash
# HEAD~ or HEAD~1 = parent of HEAD
git log HEAD~1

# HEAD~2 = grandparent
git log HEAD~2

# HEAD^ = parent (same as HEAD~1)
git log HEAD^

# HEAD^2 = second parent (for merge commits)
git log HEAD^2
```

**Practical Examples:**

```bash
# Where am I?
git branch
# * main
#   feature
#   develop

# What commit am I on?
git rev-parse HEAD
# d3a5f21b8c9e7a6f5d4c3b2a1e9f8d7c6b5a4e3d

# View HEAD history (where you've been)
git reflog
# d3a5f21 HEAD@{0}: commit: Add feature
# 8c3f0b1 HEAD@{1}: checkout: moving from main to feature
# 1a2b3c4 HEAD@{2}: commit: Initial commit
```

---

### Step 9: Multiple Branches

![Step-09](Diagrams/Step-09.png)

**What This Diagram Shows:**

How multiple branches can exist simultaneously, each pointing to different commits.

**Branch Divergence:**

```
        C4 ← C5      ← feature
       ↗
C1 ← C2 ← C3        ← main
       ↘
        C6 ← C7     ← develop
```

**Creating Divergent Branches:**

```bash
# Start: All branches at C3
C1 ← C2 ← C3
         ↑
      main, feature, develop

# Work on feature
git checkout feature
git commit -m "Feature work"

C1 ← C2 ← C3 ← C4
         ↑     ↑
        main  feature

# Work on develop
git checkout develop
git commit -m "Develop work"

C1 ← C2 ← C3 ← C4     ← feature
         ↑
         ↓
         C5 ← C6      ← develop
         ↑
        main
```

**Branch Listing:**

```bash
# List local branches
git branch
#   develop
#   feature
# * main         (* indicates current branch)

# List with commit info
git branch -v
#   develop  8c3f0b1 Develop work
#   feature  d3a5f21 Feature work
# * main     1a2b3c4 Main work

# List with upstream info
git branch -vv
#   develop  8c3f0b1 [origin/develop] Develop work
#   feature  d3a5f21 Feature work
# * main     1a2b3c4 [origin/main: ahead 2] Main work
```

**Branch Relationships:**

```bash
# See which commits are in feature but not in main
git log main..feature

# See which commits are in main but not in feature
git log feature..main

# See all commits in both (symmetric difference)
git log --left-right --graph main...feature
```

---

## Commit History and DAG

### Step 10: Directed Acyclic Graph (DAG)

![Step-10](Diagrams/Step-10.png)

**What This Diagram Shows:**

Git's commit history is a **Directed Acyclic Graph (DAG)**, not a simple linear history.

**What is a DAG?**

- **Directed:** Edges have direction (parent ← child)
- **Acyclic:** No cycles (can't have commit as its own ancestor)
- **Graph:** Nodes (commits) connected by edges (parent relationships)

**Linear History:**

```
C1 ← C2 ← C3 ← C4 ← C5
                    ↑
                  main
```

**Branched History:**

```
        ┌─ C4 ← C5      ← feature
       ↗
C1 ← C2 ← C3            ← main
       ↘
        └─ C6 ← C7      ← develop
```

**Merged History:**

```
C1 ← C2 ← C3 ← C4 ← C5 ← C8 (merge)  ← main
       ↘            ↗
        C6 ← C7 ←──┘
             ↑
           feature
```

**Understanding the Graph:**

```bash
# View graph
git log --oneline --graph --all

# Output:
# * 8c3f0b1 (HEAD -> main) Merge feature
# |\
# | * d3a5f21 (feature) Add feature
# | * 1a2b3c4 Feature work
# * | 9e8f7d6 Main work
# |/
# * 5c4b3a2 Common ancestor
```

**Why DAG Matters:**

1. **Multiple Parents (Merges)**
   ```
   Commit C8 has TWO parents:
   - C5 (from main)
   - C7 (from feature)
   ```

2. **Finding Common Ancestors**
   ```bash
   # Find merge base
   git merge-base main feature
   # Returns: 5c4b3a2 (common ancestor)
   ```

3. **Reachability**
   ```bash
   # Can we reach commit X from Y?
   git merge-base --is-ancestor X Y
   echo $?  # 0 = yes, 1 = no
   ```

**Complex DAG Example:**

```
     E ← F           ← feature2
    ↗
A ← B ← C ← D ← G   ← main
    ↘       ↗
     H ← I ← J      ← feature1
```

**Walking the DAG:**

```bash
# Topological order (parents before children)
git log --topo-order

# Date order
git log --date-order

# All reachable commits from HEAD
git log --all

# All reachable commits from multiple refs
git log main feature develop
```

---

## Merging Strategies

### Step 11: Fast-Forward Merge

![Step-11](Diagrams/Step-11.png)

**What This Diagram Shows:**

A **fast-forward merge** - the simplest type of merge in Git.

**What is Fast-Forward?**

When the target branch has no new commits since the feature branch was created, Git can simply move the pointer forward.

**Before Merge:**

```
C1 ← C2 ← C3 ← C4 ← C5
         ↑           ↑
       main       feature
```

**After Fast-Forward:**

```
C1 ← C2 ← C3 ← C4 ← C5
                    ↑
                main, feature
```

**How it Works:**

```bash
# On main branch
git checkout main

# Merge feature (fast-forward)
git merge feature

# Output:
# Updating c3..c5
# Fast-forward
#  file.txt | 2 ++
#  1 file changed, 2 insertions(+)

# main pointer simply moves to c5
# No new commit created!
```

**Why "Fast-Forward"?**

```
Think of it like fast-forwarding a video:
- You're at minute 3 (commit C3)
- The video is at minute 5 (commit C5)
- You fast-forward to minute 5
- No new content created, just moved pointer
```

**When Fast-Forward Happens:**

```bash
# Condition: Target branch must be ANCESTOR of source branch

main is at C3
feature is at C5
C3 is ancestor of C5 → Fast-forward possible

main is at C3, C4
feature is at C3, C5
C3, C4 and C3, C5 diverged → Fast-forward NOT possible
```

**Forcing Non-Fast-Forward:**

```bash
# Create merge commit even when fast-forward possible
git merge --no-ff feature

# Creates:
C1 ← C2 ← C3 ← C4 ← C5 ← C6 (merge commit)
                    ↑     ↑
                 feature main
```

**Why Force Non-Fast-Forward?**

```bash
# Preserve branch history
git merge --no-ff feature-login
# Creates merge commit showing feature was merged

# Without --no-ff (fast-forward)
# → Linear history, can't tell feature existed

# With --no-ff
# → Merge commit preserves feature branch existence
```

**Fast-Forward Only:**

```bash
# Abort if fast-forward not possible
git merge --ff-only feature

# Succeeds if fast-forward possible
# Fails if merge commit would be needed
```

---

### Step 12: Three-Way Merge

![Step-12](Diagrams/Step-12.png)

**What This Diagram Shows:**

A **three-way merge** - used when branches have diverged.

**What is Three-Way Merge?**

When both branches have new commits, Git creates a **merge commit** with two parents.

**Before Merge:**

```
        C4 ← C5      ← feature
       ↗
C1 ← C2 ← C3 ← C6    ← main
```

**After Three-Way Merge:**

```
        C4 ← C5      ← feature
       ↗         ↘
C1 ← C2 ← C3 ← C6 ← M    ← main
                     ↑
              (merge commit)
```

**The Three Commits:**

```
1. Common Ancestor (C2)
2. Our Changes (C6 on main)
3. Their Changes (C5 on feature)
```

**How it Works:**

```bash
# On main branch
git checkout main

# Merge feature
git merge feature

# Git creates merge commit M with:
# - Parent 1: C6 (main)
# - Parent 2: C5 (feature)
# - Tree: Combined changes from both
```

**Merge Commit Structure:**

```bash
# View merge commit
git cat-file -p M

tree abc123...
parent c6d5e4f3...  (main)
parent c5b4a3d2...  (feature)
author John Doe <john@example.com> ...
committer John Doe <john@example.com> ...

Merge branch 'feature' into main
```

**Three-Way Diff Algorithm:**

```
File in C2 (base):    File in C6 (main):    File in C5 (feature):
line 1                line 1                line 1
line 2                line 2 modified       line 2
line 3                line 3                line 3
                      line 4 added          line 5 added

Merge result:
line 1
line 2 modified       (from main)
line 3
line 4 added          (from main)
line 5 added          (from feature)
```

**Conflict Detection:**

```bash
# Conflict occurs when:
# - Same line modified in both branches
# - File deleted in one, modified in other
# - File added with same name in both

Example conflict:
# Base (C2):
def hello():
    print("Hello")

# Main (C6):
def hello():
    print("Hello World")

# Feature (C5):
def hello():
    print("Hello Git")

# Conflict! Git can't auto-merge
<<<<<<< HEAD
    print("Hello World")
=======
    print("Hello Git")
>>>>>>> feature
```

**Resolving Merge:**

```bash
# 1. Merge creates conflict
git merge feature
# CONFLICT (content): Merge conflict in app.py

# 2. View conflicted files
git status
# both modified: app.py

# 3. Edit file, resolve conflict
# (remove markers, keep desired code)

# 4. Stage resolved file
git add app.py

# 5. Complete merge
git commit
# (opens editor with default merge message)
```

---

### Step 13: Merge Strategies Comparison

![Step-13](Diagrams/Step-13.png)

**What This Diagram Shows:**

Comparison of different merge strategies and their outcomes.

**Merge Strategy Options:**

1. **Fast-Forward (Default)**
   ```bash
   git merge feature
   # Only if possible, otherwise three-way merge
   ```

2. **No Fast-Forward**
   ```bash
   git merge --no-ff feature
   # Always creates merge commit
   ```

3. **Fast-Forward Only**
   ```bash
   git merge --ff-only feature
   # Aborts if fast-forward not possible
   ```

4. **Squash**
   ```bash
   git merge --squash feature
   # Combines all feature commits into one
   ```

**Visual Comparison:**

**Fast-Forward:**
```
Before:  C1 ← C2 ← C3    ← main
              ↓
              C4 ← C5    ← feature

After:   C1 ← C2 ← C3 ← C4 ← C5    ← main, feature
```

**No Fast-Forward:**
```
Before:  C1 ← C2 ← C3    ← main
              ↓
              C4 ← C5    ← feature

After:   C1 ← C2 ← C3 ← C4 ← C5 ← M    ← main
                                  ↑
                            (merge commit)
```

**Squash Merge:**
```
Before:  C1 ← C2 ← C3    ← main
              ↓
              C4 ← C5 ← C6    ← feature

After:   C1 ← C2 ← C3 ← C7    ← main
                         ↑
              (C4, C5, C6 squashed)
         
         C4 ← C5 ← C6    ← feature (still exists)
```

**When to Use Each:**

```bash
# Fast-Forward (Default)
# ✓ Simple, linear history
# ✓ Feature was just sequential work
# ✗ Loses information about feature branch
git merge feature

# No Fast-Forward
# ✓ Preserves feature branch in history
# ✓ Clear indication of when feature was merged
# ✗ More complex history
git merge --no-ff feature

# Squash
# ✓ Clean, linear history
# ✓ Feature becomes one commit
# ✗ Loses individual commit history
git merge --squash feature
git commit -m "Add feature: description"

# Fast-Forward Only
# ✓ Ensures linear history
# ✗ Fails if branches diverged
git merge --ff-only feature
```

**Team Workflows:**

```bash
# GitHub Flow (Feature branches)
# Use --no-ff to preserve PR history
git merge --no-ff feature/user-login

# Trunk-Based Development
# Use --ff-only to keep linear history
git merge --ff-only feature

# GitFlow (Release management)
# Different strategies for different branches
git merge --no-ff develop          # into main
git merge --ff-only feature/xyz    # into develop
```

---

## Rebasing Internals

### Step 14: Rebase vs Merge

![Step-14](Diagrams/Step-14.png)

**What This Diagram Shows:**

The fundamental difference between **merge** and **rebase** for integrating changes.

**Merge Approach:**

```
Before:
        C4 ← C5           ← feature
       ↗
C1 ← C2 ← C3 ← C6 ← C7    ← main

After Merge:
        C4 ← C5 ←─────┐
       ↗              ↓
C1 ← C2 ← C3 ← C6 ← C7 ← M    ← main
                          ↑
                   (merge commit)
```

**Rebase Approach:**

```
Before:
        C4 ← C5           ← feature
       ↗
C1 ← C2 ← C3 ← C6 ← C7    ← main

After Rebase:
C1 ← C2 ← C3 ← C6 ← C7 ← C4' ← C5'    ← feature
                    ↑
                  main
```

**Key Differences:**

| Aspect | Merge | Rebase |
|--------|-------|--------|
| History | Preserves actual history | Rewrites history |
| Commits | Creates merge commit | No merge commit |
| Graph | Non-linear (branching) | Linear |
| Conflicts | Resolve once | May resolve multiple times |
| Safety | Safe for shared branches | Dangerous for shared branches |

**How Rebase Works:**

```bash
# Starting point
git checkout feature

# Rebase onto main
git rebase main

# Git does:
# 1. Find common ancestor (C2)
# 2. Collect commits from feature (C4, C5)
# 3. Temporarily save them
# 4. Reset feature to main (C7)
# 5. Replay commits one by one
#    - Apply C4 → creates C4'
#    - Apply C5 → creates C5'
# 6. Move feature pointer to C5'
```

**Why Commits Change (C4 → C4'):**

```bash
# Original C4:
tree: abc123
parent: C2      ← Based on C2
author: ...
message: "Feature work"

# Rebased C4':
tree: def456    ← Different tree (includes C3-C7 changes)
parent: C7      ← Based on C7 (different parent)
author: ...
message: "Feature work"

# Different parent + different tree = Different SHA-1 hash
# C4' is NOT the same as C4, even if code looks similar
```

**Interactive Rebase:**

```bash
# Rebase with editing options
git rebase -i HEAD~3

# Editor opens:
pick c4a8b2f Add feature A
pick 7f3e9d1 Add feature B
pick 2b5c8a9 Fix typo

# You can:
# pick   - use commit as-is
# reword - change commit message
# edit   - stop to amend commit
# squash - combine with previous
# fixup  - like squash, discard message
# drop   - remove commit
```

**Rebase Example:**

```bash
# Before:
git log --oneline feature
# 5c8a9b2 (feature) Third commit
# 2b5c7a1 Second commit
# 1a4b3c9 First commit
# 9e8f7d6 (main) Main work

# Interactive rebase
git rebase -i main

# Change to:
pick 1a4b3c9 First commit
squash 2b5c7a1 Second commit
reword 5c8a9b2 Third commit

# Result:
git log --oneline feature
# 8d9e2f1 (feature) Complete feature (squashed)
# 9e8f7d6 (main) Main work
```

**When to Use Rebase:**

**✅ DO use rebase:**
```bash
# 1. Clean up local commits before pushing
git rebase -i HEAD~5

# 2. Update feature branch with latest main
git checkout feature
git rebase main

# 3. Keep linear history
git pull --rebase
```

**❌ DON'T use rebase:**
```bash
# 1. On public/shared branches
git checkout main
git rebase feature  # ❌ DON'T! Others have this history

# 2. After pushing to remote
git push origin feature
git rebase main  # ❌ DON'T! Already pushed

# 3. On someone else's work
# ❌ DON'T rebase commits others are building on
```

**The Golden Rule of Rebase:**

> **Never rebase commits that have been pushed to a public repository and that others may have based work on.**

**Recovering from Bad Rebase:**

```bash
# Find previous state
git reflog
# 8d9e2f1 HEAD@{0}: rebase: Complete feature
# 5c8a9b2 HEAD@{1}: checkout: moving from main to feature

# Reset to before rebase
git reset --hard HEAD@{1}

# Or reset to specific commit
git reset --hard 5c8a9b2
```

**Rebase vs Merge Decision Tree:**

```bash
# Are you working alone on feature branch?
if [ "$solo" = true ]; then
    # Use rebase for clean history
    git rebase main
else
    # Use merge to preserve collaboration
    git merge main
fi

# Is the branch public/shared?
if [ "$public" = true ]; then
    # Use merge (safe)
    git merge main
else
    # Rebase is OK (local only)
    git rebase main
fi
```

---

## Remote Repository Interaction

### Understanding Remote References

**Remote Branches:**

Remote branches are references to the state of branches in remote repositories.

```bash
# Remote branches are stored as:
.git/refs/remotes/origin/main
.git/refs/remotes/origin/feature

# View remote branches
git branch -r
# origin/main
# origin/feature
# origin/develop

# View all branches (local + remote)
git branch -a
# * main
#   feature
#   remotes/origin/main
#   remotes/origin/feature
```

**Fetch vs Pull:**

```bash
# Fetch: Update remote references only
git fetch origin
# Downloads objects
# Updates origin/main
# Does NOT modify your local branches

# Pull: Fetch + Merge
git pull origin main
# Equivalent to:
# git fetch origin
# git merge origin/main
```

**Tracking Branches:**

```bash
# Create tracking branch
git checkout -b feature origin/feature
# Local feature tracks remote origin/feature

# View tracking relationships
git branch -vv
# * feature  abc1234 [origin/feature] Latest work
#   main     def5678 [origin/main: ahead 2] Local commits

# Set upstream after creation
git branch --set-upstream-to=origin/feature
```

**Push Variations:**

```bash
# Push to default remote
git push

# Push to specific remote and branch
git push origin feature

# Push and set upstream
git push -u origin feature

# Force push (dangerous!)
git push --force origin feature

# Force push (safer)
git push --force-with-lease origin feature
```

---

## Understanding References

### Types of References

**1. Branches (refs/heads/):**
```bash
# Local branches
.git/refs/heads/main
.git/refs/heads/feature

# Content: commit SHA-1
cat .git/refs/heads/main
# d3a5f21b8c9e7a6f5d4c3b2a1e9f8d7c6b5a4e3d
```

**2. Remote Branches (refs/remotes/):**
```bash
# Remote-tracking branches
.git/refs/remotes/origin/main
.git/refs/remotes/origin/feature

# These track state of remote repository
```

**3. Tags (refs/tags/):**
```bash
# Lightweight tags (just pointers)
.git/refs/tags/v1.0.0
# Contains: commit SHA-1

# Annotated tags (tag objects)
# Contains: SHA-1 of tag object
# Tag object contains: commit SHA-1, message, tagger, date
```

**4. Special References:**
```bash
# HEAD - current commit
.git/HEAD
# ref: refs/heads/main

# ORIG_HEAD - previous HEAD (after merge, rebase)
.git/ORIG_HEAD

# FETCH_HEAD - recently fetched branch
.git/FETCH_HEAD

# MERGE_HEAD - commit being merged
.git/MERGE_HEAD
```

**Reference Specifications (Refspecs):**

```bash
# Format: +<src>:<dst>

# Fetch refspec
fetch = +refs/heads/*:refs/remotes/origin/*
# Fetch all branches from remote
# Store in remotes/origin/

# Push refspec
push = refs/heads/main:refs/heads/main
# Push local main to remote main
```

---

## Advanced Concepts

### The .git Directory Structure

```
.git/
├── HEAD                  # Current branch pointer
├── config               # Repository configuration
├── description          # Repository description
├── hooks/               # Hook scripts
│   ├── pre-commit
│   ├── post-commit
│   └── ...
├── info/                # Global exclude file
│   └── exclude
├── objects/             # Object database
│   ├── 00/
│   ├── 01/
│   ├── ...
│   ├── ff/
│   ├── pack/           # Packed objects
│   └── info/
├── refs/                # References
│   ├── heads/          # Local branches
│   │   ├── main
│   │   └── feature
│   ├── remotes/        # Remote branches
│   │   └── origin/
│   │       ├── main
│   │       └── feature
│   └── tags/           # Tags
│       └── v1.0.0
├── logs/                # Reflog
│   ├── HEAD
│   └── refs/
└── index                # Staging area
```

### Object Storage and Packing

**Loose Objects:**
```bash
# Each object stored in separate file
echo "test" | git hash-object -w --stdin
# 9daeafb9864cf43055ae93beb0afd6c7d144bfa4

# Stored at:
.git/objects/9d/aeafb9864cf43055ae93beb0afd6c7d144bfa4

# First 2 chars = directory
# Remaining 38 chars = filename
```

**Packed Objects:**
```bash
# Git packs objects for efficiency
git gc

# Creates pack files:
.git/objects/pack/pack-abc123...pack  # Compressed objects
.git/objects/pack/pack-abc123...idx   # Index file

# Deltas are used for similar objects
# Only differences stored, not full content
```

### Garbage Collection

```bash
# Git garbage collection
git gc

# What it does:
# 1. Packs loose objects
# 2. Removes unreachable objects
# 3. Compresses pack files
# 4. Expires old reflog entries

# Aggressive GC
git gc --aggressive --prune=now
```

### Submodules Internals

```bash
# Submodule is stored as:
# 1. Gitlink (special tree entry)
# 2. .gitmodules file (configuration)
# 3. Submodule's .git directory

# View submodule commit
git ls-tree HEAD path/to/submodule
# 160000 commit abc123... path/to/submodule

# 160000 = gitlink type
```

### Hooks

```bash
# Client-side hooks:
pre-commit        # Before commit is created
prepare-commit-msg # Before commit message editor
commit-msg        # After commit message entered
post-commit       # After commit is created
pre-rebase        # Before rebase
post-checkout     # After checkout
post-merge        # After merge

# Server-side hooks:
pre-receive       # Before push is accepted
update            # Before branch is updated
post-receive      # After push is accepted
```

---

## Summary

### Key Takeaways

1. **Git is Content-Addressable**
   - Everything identified by SHA-1 hash
   - Same content = same hash = deduplicated

2. **Four Object Types**
   - Blob: File contents
   - Tree: Directory structure
   - Commit: Snapshot with metadata
   - Tag: Named reference

3. **Branches are Cheap**
   - Just 41-byte pointers
   - Creating/deleting is instant
   - No copying involved

4. **History is a DAG**
   - Commits point to parents
   - Branches can merge
   - No cycles allowed

5. **Rebase Rewrites History**
   - Changes commit SHAs
   - Creates new commits
   - Dangerous on public branches

6. **Understanding Internals Helps**
   - Better decisions during conflicts
   - Confident use of advanced features
   - Easier debugging
   - More efficient workflows

---

## Further Reading

- **Git Documentation:** https://git-scm.com/doc
- **Pro Git Book:** https://git-scm.com/book/en/v2
- **Git Internals:** https://git-scm.com/book/en/v2/Git-Internals-Plumbing-and-Porcelain
- **Git Objects:** https://git-scm.com/book/en/v2/Git-Internals-Git-Objects

---

*Understanding Git internals transforms you from a Git user to a Git master. The more you understand how it works underneath, the more powerful and confident you become with it!* 🚀
