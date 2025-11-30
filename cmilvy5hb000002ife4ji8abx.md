---
title: "Git Internals, Branching, and Diff – A Practical Deep Dive"
seoTitle: "Exploring Git: Branches and Diffs Simplified"
seoDescription: "Learn Git internals: basic commands, branching, object models, and `git diff` for change comparison. Advance your Git skills"
datePublished: Sun Nov 30 2025 15:39:03 GMT+0000 (Coordinated Universal Time)
cuid: cmilvy5hb000002ife4ji8abx
slug: git-internals-branching-and-diff-a-practical-deep-dive
tags: github, git, devops, github-actions-1

---

If you are comfortable using `git add`, `git commit`, and `git push` but want to understand what really happens behind the scenes, this guide is for you.

We will go from “What is Git?” all the way to Git’s internal object model, branches as pointers, and how `git diff` actually compares changes.

---

## 1\. What Is Git?

Git is a **Distributed Version Control System (DVCS)**. That means every developer has:

* A full copy of the repository (the `.git` directory)
    
* Complete history including all branches, commits, and tags
    
* The ability to work offline, commit, branch, merge, and later push to a remote
    

Unlike older centralized systems like CVS or SVN, Git does not depend on a single central repository for day-to-day work. The “server” is mostly used as a synchronization point for collaboration.

### Centralized vs Distributed

| Feature | Centralized VCS (CVS, SVN) | Git (DVCS) |
| --- | --- | --- |
| Server dependency | Mandatory | Not required for local work |
| Offline work | Generally no | Yes |
| Performance | Slower (network bound) | Faster (most operations are local) |
| History stored | On server | Full history stored locally |
| Branching model | Limited or heavy | Lightweight branching and rebasing |

### Why Git Changed the Game

Git is powerful because of how it stores and tracks data:

* Uses **snapshots** instead of just line-by-line diffs
    
* Keeps **immutable commit history**
    
* Performs operations locally which makes them **fast**
    
* Supports **advanced branching and merging** strategies
    
* Uses **SHA-1 hashes** to identify content and ensure integrity
    
* Stores data efficiently with **content deduplication**
    

---

## 2\. Core Git Terminology (With Internals)

| Term | Purpose | Behind the Scenes |
| --- | --- | --- |
| `git init` | Creates a new repo | Makes a `.git/` directory with metadata and references |
| Working directory | Where you edit files | Files can be tracked or untracked |
| Staging area | Prepares content for commit | Stored in `.git/index` as metadata about staged files |
| Commit | Saves a snapshot of project state | New commit object with a SHA-1 that points to a tree and parent |
| Push | Sends commits to remote | Updates refs on the remote such as `.git/refs/heads/main` |
| `HEAD` | Current branch pointer | Content of `.git/HEAD` points to a branch ref |
| Refs | Names for commits (branches, tags) | Files in `.git/refs/` and `packed-refs` |

---

## 3\. Git’s Core Object Model

Everything Git stores lives under `.git/objects` as one of four object types.

| Object Type | Purpose |
| --- | --- |
| **Blob** | File content (no filename, just data) |
| **Tree** | Directory structure: filenames, modes, and hashes |
| **Commit** | Snapshot reference plus metadata and parent links |
| **Tag** | Human-friendly named reference to a commit or object |

### Blob Objects

* Represent raw file content
    
* Compressed and stored by hash (content addressable)
    
* Identified by a SHA-1 hash
    

```bash
echo "Hello World" > file.txt
git hash-object file.txt
```

Git creates a blob and stores it under `.git/objects/<first2>/<restOfHash>`.

---

### Tree Objects

Trees describe directories and their contents. They map:

* Filenames to blob or tree hashes
    
* File modes and types (file, directory, executable)
    

You can inspect a tree with:

```bash
git ls-tree HEAD
```

This prints the tree for the current commit.

---

### Commit Objects

A commit ties everything together:

* Points to a **tree** (the root directory for that snapshot)
    
* Stores **author**, **committer**, **date**, and **message**
    
* Points to one or more **parent** commits
    

```bash
git cat-file -p HEAD
```

Example output:

```text
tree 48ab23...
parent a1c9e7...
author Aditya <adi@devops.com>
committer Aditya <adi@devops.com>
date   Tue Apr 9 20:31:54 2024 +0530

Initial commit
```

---

## 4\. The Git Commit DAG

Git history is **not** just a straight line. It is a **Directed Acyclic Graph (DAG)** of commits.

```text
A --- B --- C --- D  (main)
       \
        E --- F      (feature)
```

* Each commit points to its parent or parents
    
* Merge commits have multiple parents
    
* “Directed acyclic” means the graph has direction (parent to child) and no cycles
    
* Git resolves branches and history using these parent relationships
    

This structure allows branching and merging without copying entire directories.

---

## 5\. SHA-1 Hashing in Git

Git uses SHA-1 hashes like `9fceb02c...` to uniquely identify:

* Blobs
    
* Trees
    
* Commits
    
* Tags
    

Properties:

* Even a one-character change creates a completely different hash
    
* The hash is based on the content, so Git is **content addressable**
    
* Ensures data integrity and enables deduplication
    

---

## 6\. What Actually Happens During Common Git Commands

### `git add file.txt`

Behind the scenes:

1. Git hashes the file content.
    
2. Creates a blob object if that content does not already exist.
    
3. Updates the `.git/index` (staging area) to reference this blob.
    

### `git commit -m "msg"`

Git will:

1. Write a **tree object** that represents the directory structure and staged blobs.
    
2. Create a **commit object** that points to that tree and the previous commit.
    
3. Move `HEAD` (and the current branch ref) to this new commit.
    

### `git push`

* Sends new commits and updated refs to the remote repository over HTTP/SSH.
    
* Remote refs like `refs/heads/main` are updated to point to the latest commit.
    

---

## 7\. Inside the `.git` Directory

A typical `.git` directory looks like this:

```text
.git/
├── HEAD          # Pointer to the current branch
├── config        # Repo configuration
├── index         # Staging area (binary)
├── objects/      # All Git objects (blobs, trees, commits, tags)
└── refs/         # Branch and tag references
    ├── heads/    # Local branches
    └── tags/     # Tags
```

---

## 8\. Explore Git Internals Yourself

Try this small lab in any folder:

```bash
# Create repo and commit
git init
echo "Hello Git" > hello.txt
git add hello.txt
git commit -m "Initial commit"

# Inspect internals
ls .git/objects
git cat-file -p HEAD
git cat-file -p <commit-hash>
git cat-file -p <tree-hash>
```

This makes the theory very concrete.

---

## 9\. Git Branch Management (Concepts + Internals)

### What Is a Git Branch?

A branch is a **lightweight movable pointer** to a commit. It is not a separate copy of all files. Think of it like a bookmark in the commit DAG.

* You can move the branch by committing new changes.
    
* You can create new branches without duplicating data.
    

### How Branches Work Under the Hood

* `HEAD` points to your current branch.
    
* Each branch is a file under `.git/refs/heads/<branch-name>` that contains one SHA-1 commit hash.
    
* Creating a branch is just writing a new file with a hash.
    

Example:

```bash
git branch feature-x
cat .git/refs/heads/feature-x
```

You will see a commit hash like:

```text
3adf23d5d61e2e6aee43b00e93e93e1e9e32a012
```

When you make new commits on `feature-x`, Git updates this file with the new commit hash.

---

### Essential Branch Commands with Practical Meaning

| Task | Command | Scenario |
| --- | --- | --- |
| Create branch | `git branch feature-x` | Start a new login feature |
| Switch to branch | `git switch feature-x` or `git checkout feature-x` | Begin working on that feature |
| Create and switch | `git checkout -b feature-x` | One shot: create then move to new branch |
| List local branches | `git branch` | See which branches you have locally |
| Delete merged branch | `git branch -d feature-x` | Cleanup after merging |
| Force delete branch | `git branch -D feature-x` | Delete even if not merged |
| Show remote branches | `git branch -r` | See branches on origin / remote |
| Show all (local + remote) | `git branch -a` | Full list |

### Small Branching Assignment

```bash
git init
echo "Hello World" > hello.txt
git add . && git commit -m "Initial commit"

# Create and use a feature branch
git branch feature-login
git checkout feature-login
echo "Login Page" > login.html
git add . && git commit -m "Add login page"
```

Now go back to `main`:

```bash
git checkout main
ls
```

You will notice `login.html` is not there on `main`. That is branch isolation in action.

---

### Branch Naming Conventions for Teams

Use meaningful, consistent names:

| Branch Type | Example | Purpose |
| --- | --- | --- |
| Mainline | `main` or `master` | Production ready code |
| Integration | `develop` | Integration and testing |
| Feature | `feature/payment-integration` | New features |
| Bugfix | `bugfix/login-crash` | Non critical bug fixes |
| Hotfix | `hotfix/critical-vuln` | Urgent fixes for production |
| Release | `release/v1.2.0` | Stabilization before release |

---

### Popular Corporate Branching Strategies

#### 1\. Git Flow (Vincent Driessen Model)

Good for large, release based projects.

Branches:

* `main` for production
    
* `develop` for integration
    
* `feature/*`, `release/*`, `hotfix/*`
    

Example flow:

```bash
# Start from develop
git checkout develop
git checkout -b feature/cart-page

# After work
git checkout develop
git merge feature/cart-page

# Prepare a release
git checkout -b release/v1.0

# After testing
git checkout main
git merge release/v1.0
git tag v1.0

# Sync back to develop
git checkout develop
git merge release/v1.0
```

#### 2\. GitHub Flow

Ideal for continuous deployment and simpler workflows:

* Only `main` as the long lived branch
    
* Short lived feature branches and Pull Requests
    

Example:

```bash
git checkout -b feature/signup-form main
# Work, commit, push, open PR, review, merge
```

#### 3\. Trunk Based Development

Used by many large scale teams:

* Everyone commits frequently to `main` (or `trunk`)
    
* Uses **feature flags** to hide incomplete features
    
* Requires strong CI and automated testing
    

---

### Best Practices for Branch Management

* Use descriptive names like `feature/signup-form`
    
* Delete stale branches regularly
    
* Merge or rebase frequently so branches do not drift too far
    
* Prefer **rebase locally**, **merge on remote** (via PR) for cleaner history
    
* Use `git fetch --prune` to clean up stale remote tracking branches
    
* Use PR templates for consistent reviews
    

### Common Branching Issues and Fixes

| Issue | Possible Fix |
| --- | --- |
| Branch started from wrong base | `git rebase correct-base` |
| Extra unwanted commits in a branch | `git reset` or `git cherry-pick` |
| Merge conflict | Resolve manually, then `git add` and `git commit` |
| Old remote refs after deletion on server | `git fetch --prune` |

### Practice Ideas

1. Simulate a full Git Flow cycle from `develop` to `main`.
    
2. Intentionally create a merge conflict and resolve it.
    
3. Create and delete a few dummy branches both locally and on remote.
    
4. Create a PR in GitHub or GitLab from a feature branch.
    

---

## 10\. `git diff` in Depth

### What Is `git diff`?

`git diff` shows **line by line changes** between two versions of your project. It can compare:

* Working directory vs staging area
    
* Staging area vs last commit
    
* Any two commits
    
* Any two branches
    

### Common `git diff` Commands

| Command | Compares | Use Case |
| --- | --- | --- |
| `git diff` | Working directory vs staging | See unstaged changes |
| `git diff --cached` | Staging vs last commit | See what will be committed |
| `git diff HEAD` | Working directory vs last commit | All uncommitted changes |
| `git diff A B` | Commit A vs Commit B | Compare two points in history |
| `git diff branch1..branch2` | branch1 vs branch2 | See what changed between branches |

### Practical Examples

```bash
# Before staging
git diff

# After staging
git add file.txt
git diff --cached

# Between two commits
git diff 4e9f5d1 29c3dd2

# Between branches
git diff develop..feature-xyz
```

### Understanding `git diff` Output

Example snippet:

```text
diff --git a/index.html b/index.html
index 83db48f..bf2692e 100644
--- a/index.html
+++ b/index.html
@@ -5,7 +5,7 @@
<title>Homepage</title>
-<h1>Hello World</h1>
+<h1>Welcome to Git Course</h1>
```

Key parts:

* `---` and `+++` show the file before and after.
    
* `@@ -5,7 +5,7 @@` shows the affected line ranges.
    
* Lines starting with `-` are removed.
    
* Lines starting with `+` are added.
    

### Advanced Diff Usage

* Last 3 commits vs current:
    
    ```bash
    git diff HEAD~3 HEAD
    ```
    
* Staged vs last commit:
    
    ```bash
    git diff --cached
    ```
    
* Word by word diff:
    
    ```bash
    git diff --word-diff
    ```
    
* Summary statistics:
    
    ```bash
    git diff --stat
    ```
    

Tips:

* Use `git difftool` to launch graphical comparison tools.
    
* Example alias:
    
    ```bash
    git config --global alias.diffs "diff --stat --cached"
    ```
    

---

## 11\. Git in One Line: Snapshots + DAG + Content Hashing

Git is essentially:

| Core Concept | Benefit |
| --- | --- |
| Snapshot based | Each commit stores the full project state |
| DAG structure | Powerful branching, merging, and rebasing |
| SHA-1 hashing | Strong data integrity and automatic deduplication |
| Local repository | Fast operations and full offline capability |
| Immutable commits | Clean audit trail and trustworthy history |

---

## 12\. Final Takeaways

* Git is not just “a VCS”. It is a **snapshot engine** with a **content addressed filesystem**.
    
* Every commit records a complete **tree** snapshot plus metadata.
    
* **Blobs** store content, **trees** organize it, **commits** record history, **refs** name points in that history.
    
* Git’s internals (SHA-1 hashes, DAG of commits, and local objects) are what make it fast, robust, and scalable for teams of any size.