# Git Branching & Merging: A Comprehensive Guide

## Table of Contents
- [Introduction to Branching](#introduction-to-branching)
- [Understanding Branches](#understanding-branches)
- [Creating and Managing Branches](#creating-and-managing-branches)
- [Merging Fundamentals](#merging-fundamentals)
- [Types of Merges](#types-of-merges)
- [Branching Strategies](#branching-strategies)
- [Conflict Resolution](#conflict-resolution)
- [Advanced Branching Concepts](#advanced-branching-concepts)
- [Best Practices](#best-practices)

---

## Introduction to Branching

Branching is one of Git's most powerful features, allowing developers to diverge from the main line of development and work independently without affecting the main codebase. Think of branches as parallel universes where you can experiment, develop features, or fix bugs in isolation.

### Why Branching Matters

- **Isolation**: Work on features without affecting production code
- **Collaboration**: Multiple developers can work simultaneously
- **Experimentation**: Try new ideas safely
- **Organization**: Keep different types of work separated
- **Release Management**: Maintain multiple versions of your software

---

## Understanding Branches

### What is a Branch?

A branch in Git is simply a **lightweight movable pointer** to a commit. The default branch is typically called `main` (or `master` in older repositories).

```
┌──────────────────────────────────────────────────────┐
│                   Commit Object                      │
├──────────────────────────────────────────────────────┤
│  Commit Hash:  a3f5b21...                           │
│  Tree:         Snapshot of all files                │
│  Parent:       Pointer to previous commit(s)        │
│  Author:       Name <email> (timestamp)             │
│  Committer:    Name <email> (timestamp)             │
│  Message:      "Descriptive commit message"         │
└──────────────────────────────────────────────────────┘
```

### How Branches Work Internally

```
┌─────────────────────────────────────────────────────────┐
│  INITIAL STATE (main branch)                            │
├─────────────────────────────────────────────────────────┤
│                                                         │
│     ●───────●───────●                                   │
│     A       B       C                                   │
│                     ↑                                   │
│                     │                                   │
│                  ┌──┴───┐                               │
│                  │ main │  ← branch pointer             │
│                  └──┬───┘                               │
│                     ↑                                   │
│                  ┌──┴───┐                               │
│                  │ HEAD │  ← you are here               │
│                  └──────┘                               │
└─────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────┐
│  CREATING NEW BRANCH (feature)                          │
├─────────────────────────────────────────────────────────┤
│                                                         │
│     ●───────●───────●                                   │
│     A       B       C                                   │
│                     ↑                                   │
│                     │                                   │
│              ┌──────┼──────┐                            │
│              ↓             ↓                            │
│          ┌──────┐     ┌─────────┐                       │
│          │ main │     │ feature │  ← new branch         │
│          └──────┘     └────┬────┘                       │
│                            ↑                            │
│                        ┌───┴────┐                       │
│                        │  HEAD  │  ← switched here      │
│                        └────────┘                       │
└─────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────┐
│  AFTER COMMITTING ON FEATURE BRANCH                     │
├─────────────────────────────────────────────────────────┤
│                                                         │
│     ●───────●───────●                                   │
│     A       B       C                                   │
│                     ↑                                   │
│                     │                                   │
│                 ┌───┴────┐                              │
│                 │  main  │                              │
│                 └────────┘                              │
│                     │                                   │
│                     └─────●───────●                     │
│                           D       E                     │
│                                   ↑                     │
│                            ┌──────┴──────┐              │
│                            │   feature   │              │
│                            └──────┬──────┘              │
│                                   ↑                     │
│                              ┌────┴─────┐               │
│                              │   HEAD   │               │
│                              └──────────┘               │
└─────────────────────────────────────────────────────────┘
```

### The HEAD Pointer

`HEAD` is a special pointer that indicates your current working position (which branch you're on). It typically points to a branch reference, which in turn points to a commit.

```
┌─────────────────────────────────────────────────────────┐
│  DETACHED HEAD (directly pointing to commit)            │
├─────────────────────────────────────────────────────────┤
│                                                         │
│     ●───────●───────●───────●                           │
│     A       B       C       D                           │
│                     ↑                                   │
│                     │                                   │
│                ┌────┴─────┐                             │
│                │   HEAD   │  ⚠ Detached!                │
│                └──────────┘                             │
│                                                         │
│     Changes made here won't be associated with          │
│     any branch and may be lost.                         │
└─────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────┐
│  NORMAL HEAD (pointing to branch reference)             │
├─────────────────────────────────────────────────────────┤
│                                                         │
│     ●───────●───────●───────●                           │
│     A       B       C       D                           │
│                             ↑                           │
│                             │                           │
│                        ┌────┴─────┐                     │
│                        │   main   │                     │
│                        └────┬─────┘                     │
│                             ↑                           │
│                        ┌────┴─────┐                     │
│                        │   HEAD   │  ✓ Normal           │
│                        └──────────┘                     │
└─────────────────────────────────────────────────────────┘
```

---

## Creating and Managing Branches

### Basic Branch Commands

```bash
# Create a new branch
git branch <branch-name>

# Create and switch to new branch
git checkout -b <branch-name>
# or (modern syntax)
git switch -c <branch-name>

# List all branches
git branch                  # local branches
git branch -r              # remote branches
git branch -a              # all branches

# Switch branches
git checkout <branch-name>
# or (modern syntax)
git switch <branch-name>

# Rename a branch
git branch -m <old-name> <new-name>

# Delete a branch
git branch -d <branch-name>    # safe delete (only if merged)
git branch -D <branch-name>    # force delete

# Delete remote branch
git push origin --delete <branch-name>
```

### Branch Lifecycle Visualization

```
┌──────────────────────────────────────────────────────────────────────┐
│  STEP 1: CREATE BRANCH                                               │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│   main:       ●───────●───────●                                      │
│               A       B       C                                      │
│                               ↑                                      │
│                            [main]                                    │
│                                                                      │
│   feature:                    ● (branch created, points to C)       │
│                               ↑                                      │
│                           [feature]                                  │
│                                                                      │
│   Command: git checkout -b feature                                   │
└──────────────────────────────────────────────────────────────────────┘

┌──────────────────────────────────────────────────────────────────────┐
│  STEP 2: DEVELOP ON BRANCH                                           │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│   main:       ●───────●───────●                                      │
│               A       B       C                                      │
│                               ↑                                      │
│                            [main]                                    │
│                               │                                      │
│                               └───●───────●───────●                  │
│   feature:                        D       E       F                  │
│                                                   ↑                  │
│                                               [feature]              │
│                                                   ↑                  │
│                                                 [HEAD]               │
│                                                                      │
│   Commands: git commit -m "..." (multiple times)                     │
└──────────────────────────────────────────────────────────────────────┘

┌──────────────────────────────────────────────────────────────────────┐
│  STEP 3: MERGE BACK TO MAIN                                          │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│   main:       ●───────●───────●───────────────────●                  │
│               A       B       C                   M  (merge commit)  │
│                               ↑                 ╱ ↑                  │
│                            [main]             ╱ [HEAD]               │
│                               │             ╱                        │
│                               └───●───────●───────●                  │
│   feature:                        D       E       F                  │
│                                                   ↑                  │
│                                               [feature]              │
│                                                                      │
│   Commands: git checkout main                                        │
│             git merge feature                                        │
└──────────────────────────────────────────────────────────────────────┘

┌──────────────────────────────────────────────────────────────────────┐
│  STEP 4: DELETE BRANCH (CLEANUP)                                     │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│   main:       ●───────●───────●───────────────────●                  │
│               A       B       C                   M                  │
│                               ↑                 ╱ ↑                  │
│                               │               ╱ [main]               │
│                               │             ╱  [HEAD]                │
│                               └───●───────●───────●                  │
│                                   D       E       F                  │
│                                                                      │
│   feature: (deleted - commits preserved in history)                 │
│                                                                      │
│   Command: git branch -d feature                                     │
└──────────────────────────────────────────────────────────────────────┘
```

---

## Merging Fundamentals

Merging is the process of integrating changes from one branch into another. Git uses several strategies depending on the history structure.

### Basic Merge Command

```bash
# Switch to the branch you want to merge INTO
git checkout main

# Merge the feature branch INTO main
git merge feature-branch
```

### Merge Direction Matters

```
┌──────────────────────────────────────────────────────────────────────┐
│  BEFORE MERGE (you are on main)                                      │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│   main:       ●───────●───────●                                      │
│               A       B       C ← YOU ARE HERE                       │
│                               ↑                                      │
│                            [main*]  * = current branch               │
│                                                                      │
│   feature:    ●───────●───────●───────●                              │
│               A       B       D       E                              │
│                               ↑                                      │
│                           [feature]                                  │
│                                                                      │
│   Command to run: git merge feature                                  │
│   (merges feature INTO main)                                         │
└──────────────────────────────────────────────────────────────────────┘

┌──────────────────────────────────────────────────────────────────────┐
│  AFTER: git merge feature                                            │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│   main:       ●───────●───────●───────────────────●                  │
│               A       B       C                   M                  │
│                               │                 ╱ ↑                  │
│                               │               ╱ [main*]              │
│                               │             ╱                        │
│   feature:                    └───●───────●                          │
│                                   D       E                          │
│                                           ↑                          │
│                                       [feature]                      │
│                                                                      │
│   Result: Merge commit M combines C and E                            │
└──────────────────────────────────────────────────────────────────────┘
```

---

## Types of Merges

### 1. Fast-Forward Merge

**When it happens**: When there are no new commits on the target branch since the source branch was created.

**What happens**: Git simply moves the branch pointer forward.

```
┌──────────────────────────────────────────────────────────────────────┐
│  BEFORE FAST-FORWARD MERGE                                           │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│   Timeline:   ●───────●───────●───────●───────●                      │
│               A       B       C       D       E                      │
│                               ↑                                      │
│                            [main*]                                   │
│                                                                      │
│                                               ↑                      │
│                                           [feature]                  │
│                                                                      │
│   Note: main has not moved since feature was created                 │
│         (both share commits A, B, C)                                 │
│                                                                      │
│   Command: git merge feature                                         │
└──────────────────────────────────────────────────────────────────────┘

┌──────────────────────────────────────────────────────────────────────┐
│  AFTER FAST-FORWARD MERGE                                            │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│   Timeline:   ●───────●───────●───────●───────●                      │
│               A       B       C       D       E                      │
│                                               ↑                      │
│                                          ┌────┴────┐                 │
│                                          │  main*  │                 │
│                                          │ feature │                 │
│                                          └─────────┘                 │
│                                                                      │
│   Result: main pointer simply moved forward to E                     │
│           No merge commit created (linear history)                   │
│           "Fast-forward" because main caught up                      │
└──────────────────────────────────────────────────────────────────────┘
```

**Command**:
```bash
git checkout main
git merge feature  # Fast-forward by default
```

**Characteristics**:
- ✅ Clean, linear history
- ✅ No merge commit created
- ❌ Loses information about feature branch
- ❌ Can't track which commits were part of a feature

---

### 2. Non-Fast-Forward Merge (--no-ff)

**When to use**: When you want to preserve the existence of a feature branch in history.

**What happens**: Git creates a merge commit even when a fast-forward is possible.

```
┌──────────────────────────────────────────────────────────────────────┐
│  BEFORE NON-FAST-FORWARD MERGE                                       │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│   Timeline:   ●───────●───────●───────●───────●                      │
│               A       B       C       D       E                      │
│                               ↑                                      │
│                            [main*]                                   │
│                                                                      │
│                                               ↑                      │
│                                           [feature]                  │
│                                                                      │
│   Note: Fast-forward is possible, but we want merge commit           │
│                                                                      │
│   Command: git merge --no-ff feature -m "Merge feature branch"       │
└──────────────────────────────────────────────────────────────────────┘

┌──────────────────────────────────────────────────────────────────────┐
│  AFTER NON-FAST-FORWARD MERGE                                        │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│                               ┌───────────────────┐                  │
│   Timeline:   ●───────●───────●───────●───────●───M                  │
│               A       B       C       D       E   │                  │
│                               │                   │                  │
│                               │                   ↑                  │
│                               └───────────────[main*]                │
│                                                                      │
│                                               ↑                      │
│                                           [feature]                  │
│                                                                      │
│   Result: Merge commit M created with 2 parents: C and E             │
│           Feature branch history preserved                           │
│           Clear boundary showing "this was a feature"                │
│                                                                      │
│   Benefits: Easy to revert (git revert M), clear feature tracking   │
└──────────────────────────────────────────────────────────────────────┘
```

**Command**:
```bash
git checkout main
git merge --no-ff feature -m "Merge feature branch"
```

**Characteristics**:
- ✅ Preserves feature branch history
- ✅ Clear visualization of feature boundaries
- ✅ Easy to revert entire feature
- ❌ More complex history graph

---

### 3. Three-Way Merge

**When it happens**: When both branches have diverged (new commits on both branches).

**What happens**: Git creates a merge commit with two parents, combining changes from both branches.

```
┌──────────────────────────────────────────────────────────────────────┐
│  BEFORE THREE-WAY MERGE (DIVERGED BRANCHES)                          │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│                                   ┌───────●───────●                  │
│                                   │       F       G                  │
│                                   │               ↑                  │
│   Timeline:   ●───────●───────●───┤            [main*]               │
│               A       B       C   │                                  │
│                               ↑   │                                  │
│                         Common    │                                  │
│                         Ancestor  │                                  │
│                                   │                                  │
│                                   └───────●───────●                  │
│                                           D       E                  │
│                                                   ↑                  │
│                                               [feature]              │
│                                                                      │
│   Note: Both branches have moved since C                             │
│         main: C → F → G                                              │
│         feature: C → D → E                                           │
│                                                                      │
│   Command: git merge feature                                         │
└──────────────────────────────────────────────────────────────────────┘

┌──────────────────────────────────────────────────────────────────────┐
│  AFTER THREE-WAY MERGE                                               │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│                                   ┌───────●───────●─────────┐        │
│                                   │       F       G         │        │
│                                   │                         │        │
│   Timeline:   ●───────●───────●───┤                         M        │
│               A       B       C   │                       ╱ ↑        │
│                                   │                     ╱ [main*]    │
│                                   │                   ╱              │
│                                   └───────●───────●─────              │
│                                           D       E                  │
│                                                   ↑                  │
│                                               [feature]              │
│                                                                      │
│   Result: Merge commit M with TWO parents (G and E)                  │
│           Combines changes from both branches                        │
│           Common ancestor C used to determine conflicts              │
└──────────────────────────────────────────────────────────────────────┘

┌──────────────────────────────────────────────────────────────────────┐
│  HOW THREE-WAY MERGE ALGORITHM WORKS                                 │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│                     ┌────────────────────┐                           │
│                     │  Common Ancestor   │                           │
│                     │   Commit C         │                           │
│                     │  (Base version)    │                           │
│                     └─────────┬──────────┘                           │
│                               │                                      │
│                   ┌───────────┴───────────┐                          │
│                   │                       │                          │
│                   ▼                       ▼                          │
│        ┌──────────────────┐    ┌──────────────────┐                 │
│        │   Your Changes   │    │  Their Changes   │                 │
│        │   Commit G       │    │   Commit E       │                 │
│        │   (main branch)  │    │ (feature branch) │                 │
│        └────────┬─────────┘    └─────────┬────────┘                 │
│                 │                        │                          │
│                 └───────────┬────────────┘                          │
│                             │                                       │
│                             ▼                                       │
│                  ┌──────────────────────┐                           │
│                  │   Merge Algorithm    │                           │
│                  │   Compares:          │                           │
│                  │   • C vs G (diff1)   │                           │
│                  │   • C vs E (diff2)   │                           │
│                  │   • Combines diffs   │                           │
│                  └──────────┬───────────┘                           │
│                             │                                       │
│                             ▼                                       │
│                  ┌──────────────────────┐                           │
│                  │  Merge Result M      │                           │
│                  │  (Auto if no conflict│                           │
│                  │   Manual if conflict)│                           │
│                  └──────────────────────┘                           │
└──────────────────────────────────────────────────────────────────────┘
```

**Command**:
```bash
git checkout main
git merge feature  # Automatically does three-way merge if needed
```

---

### 4. Squash Merge

**When to use**: When you want to condense all feature branch commits into a single commit.

**What happens**: All changes from the feature branch are combined into one commit on the target branch.

```
┌──────────────────────────────────────────────────────────────────────┐
│  BEFORE SQUASH MERGE                                                 │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│   main:       ●───────●───────●                                      │
│               A       B       C                                      │
│                               ↑                                      │
│                            [main*]                                   │
│                               │                                      │
│                               │                                      │
│                               └───────●───────●───────●              │
│   feature:                            D       E       F              │
│                                       │       │       │              │
│                                       │       │       ↑              │
│                                       │       │   [feature]          │
│                                       │       │                      │
│                               Commit messages:                       │
│                               D: "WIP: Start feature"                │
│                               E: "Fix typo"                          │
│                               F: "Complete feature"                  │
│                                                                      │
│   Command: git merge --squash feature                                │
│            git commit -m "Add complete feature X"                    │
└──────────────────────────────────────────────────────────────────────┘

┌──────────────────────────────────────────────────────────────────────┐
│  AFTER SQUASH MERGE                                                  │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│   main:       ●───────●───────●───────●                              │
│               A       B       C       S                              │
│                                       ↑                              │
│                                    [main*]                           │
│                                       │                              │
│   S contains:                         │                              │
│   • All changes from D+E+F            │                              │
│   • Single commit message             │                              │
│   • No link to feature branch         │                              │
│   • Clean history                     │                              │
│                                       │                              │
│                                       │                              │
│                               ┌───────●───────●───────●              │
│   feature:                    │       D       E       F              │
│                               │                       ↑              │
│                               │                   [feature]          │
│                               │   (unchanged, can be deleted)        │
│                               C                                      │
│                                                                      │
│   Result: One clean commit on main                                   │
│           Feature branch commits not in main history                 │
│           Ideal for PRs with messy commit history                    │
└──────────────────────────────────────────────────────────────────────┘
```

**Command**:
```bash
git checkout main
git merge --squash feature
git commit -m "Add complete feature X"
```

**Characteristics**:
- ✅ Clean history (one commit per feature)
- ✅ Easy to review in history
- ❌ Loses granular commit history
- ❌ Feature branch history is discarded

---

### Merge Comparison Table

| Merge Type | Creates Merge Commit? | Linear History? | Preserves Branch Info? | Use Case |
|------------|----------------------|-----------------|----------------------|----------|
| Fast-Forward | ❌ | ✅ | ❌ | Simple linear development |
| Non-FF (`--no-ff`) | ✅ | ❌ | ✅ | Feature branch workflow |
| Three-Way | ✅ | ❌ | ✅ | Divergent branches |
| Squash | ❌* | ✅ | ❌ | Clean history, single commit per feature |

*Squash creates a regular commit, not a merge commit

---

## Branching Strategies

### 1. Git Flow

**Structure**: Long-lived branches with specific purposes.

```
┌──────────────────────────────────────────────────────────────────────┐
│  GIT FLOW BRANCH HIERARCHY                                           │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│                  ┌─────────────────────────┐                         │
│                  │         main            │  ← Production           │
│                  │  (production releases)  │    Only tested code     │
│                  └───────────┬─────────────┘    Protected branch     │
│                              │                                       │
│                              │ merges from release/hotfix            │
│                              │                                       │
│                  ┌───────────┴─────────────┐                         │
│                  │        develop          │  ← Integration          │
│                  │  (integration branch)   │    Latest features      │
│                  └───────────┬─────────────┘    Nightly builds       │
│                              │                                       │
│             ┌────────────────┼────────────────┐                      │
│             │                │                │                      │
│             ▼                ▼                ▼                      │
│    ┌────────────────┐ ┌────────────┐ ┌────────────────┐             │
│    │ feature/login  │ │feature/pay │ │feature/profile │             │
│    │  (temporary)   │ │(temporary) │ │  (temporary)   │             │
│    └────────────────┘ └────────────┘ └────────────────┘             │
│                                                                      │
│                              │                                       │
│                 ┌────────────┴──────────┐                            │
│                 │                       │                            │
│                 ▼                       ▼                            │
│         ┌───────────────┐      ┌──────────────┐                     │
│         │  release/1.0  │      │ hotfix/bug   │                     │
│         │  (temporary)  │      │ (emergency)  │                     │
│         └───────────────┘      └──────────────┘                     │
│                 │                       │                            │
│                 └───────────┬───────────┘                            │
│                             │                                        │
│                             ▼                                        │
│                         merge to                                     │
│                      main & develop                                  │
└──────────────────────────────────────────────────────────────────────┘

┌──────────────────────────────────────────────────────────────────────┐
│  GIT FLOW TIMELINE VISUALIZATION                                     │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  main:       ●───────────────●───────────────●───────●               │
│              A              v1.0            v1.1    v1.1.1           │
│              │               ↑               ↑       ↑               │
│              │               │               │       │               │
│              │               │               │       │               │
│  develop:    ●───●───●───●───●───●───●───●───●───●───●               │
│              A   │   │   │   │   │   │   │   │   │   │               │
│                  │   │   │   │   │   │   │   │   │   │               │
│  feature/        │   │   │   │   │   │   │   │   │   │               │
│  login:      ────●───●───┘   │   │   │   │   │   │   │               │
│                              │   │   │   │   │   │   │               │
│  feature/                    │   │   │   │   │   │   │               │
│  payment:    ────────────────●───●───┘   │   │   │   │               │
│                                          │   │   │   │               │
│  release/                                │   │   │   │               │
│  1.1:        ────────────────────────────●───┘   │   │               │
│                                                  │   │               │
│  hotfix/                                         │   │               │
│  bug:        ────────────────────────────────────────●───┘           │
│                                                                      │
│  Legend:  ● = commit    ─ = branch flow    ↑ = merge to main        │
└──────────────────────────────────────────────────────────────────────┘
```

**Branch Types**:

1. **Main Branch** (`main`/`master`)
   - Production-ready code only
   - Every commit is a release
   - Protected branch

2. **Develop Branch** (`develop`)
   - Integration branch for features
   - Reflects latest development changes
   - Source for feature branches

3. **Feature Branches** (`feature/*`)
   ```
   Branch from: develop
   Merge to: develop
   Naming: feature/user-authentication
   Lifespan: Until feature complete
   ```

4. **Release Branches** (`release/*`)
   ```
   Branch from: develop
   Merge to: main AND develop
   Naming: release/1.2.0
   Lifespan: Until release deployed
   ```

5. **Hotfix Branches** (`hotfix/*`)
   ```
   Branch from: main
   Merge to: main AND develop
   Naming: hotfix/security-patch
   Lifespan: Until fix deployed
   ```

**Complete Git Flow Example**:

```
┌──────────────────────────────────────────────────────────────────────┐
│  COMPLETE GIT FLOW LIFECYCLE (Time flows left → right)               │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  main:                                                               │
│  ●────────────────────────────────●──────────────────●               │
│  │                                │                  │               │
│  A                              v1.1              v1.1.1             │
│  │                           (release)           (hotfix)            │
│  │                                ↑                  ↑               │
│  │                                │                  │               │
│  develop:                         │                  │               │
│  ●────●────●─────────●────●────●──┴──●────●──────●───┘               │
│  A    B    C         D    E    F      G    H      I                  │
│  │    │    │         ↑    │    ↑      ↑    ↑      ↑                  │
│  │    │    │         │    │    │      │    │      │                  │
│  │    └────┴─────────┘    │    │      │    │      │                  │
│  │  feature/login         │    │      │    │      │                  │
│  │  (B: start → C: finish)│    │      │    │      │                  │
│  │                         │    │      │    │      │                  │
│  │                         └────┴──────┘    │      │                  │
│  │                     feature/payment      │      │                  │
│  │                     (D: start → E,F: complete)  │                  │
│  │                                          │      │                  │
│  │                                   ┌──────┴──────┘                  │
│  │                                   │   release/1.1                  │
│  │                                   │   (G: prep → H: deploy)        │
│  │                                   │                                │
│  │                                   └────────────┐                   │
│  │                                     hotfix/security-patch          │
│  │                                     (I: urgent fix)                │
│  A (initial commit)                                                  │
│                                                                      │
│  ┌────────────────────────────────────────────────────────────────┐  │
│  │ WORKFLOW STEPS:                                                │  │
│  │                                                                │  │
│  │ 1. Feature branches (login, payment) branch from develop      │  │
│  │ 2. Features merge back to develop when complete               │  │
│  │ 3. Release branch created from develop for v1.1               │  │
│  │ 4. Release tested, bug fixes added, merged to main & develop  │  │
│  │ 5. Tag created on main (v1.1)                                 │  │
│  │ 6. Hotfix branches from main for critical production bugs     │  │
│  │ 7. Hotfix merged to both main (v1.1.1) and develop            │  │
│  └────────────────────────────────────────────────────────────────┘  │
│                                                                      │
│  Legend:  ● = commit   ─ = progression   ↑ = merge   │ = branch     │
└──────────────────────────────────────────────────────────────────────┘
```

**Commands**:
```bash
# Start a feature
git checkout develop
git checkout -b feature/new-login

# Finish a feature
git checkout develop
git merge --no-ff feature/new-login
git branch -d feature/new-login

# Start a release
git checkout develop
git checkout -b release/1.2.0
# ... version bump, final fixes

# Finish a release
git checkout main
git merge --no-ff release/1.2.0
git tag -a v1.2.0
git checkout develop
git merge --no-ff release/1.2.0
git branch -d release/1.2.0

# Hotfix
git checkout main
git checkout -b hotfix/security-fix
# ... make fix
git checkout main
git merge --no-ff hotfix/security-fix
git tag -a v1.2.1
git checkout develop
git merge --no-ff hotfix/security-fix
git branch -d hotfix/security-fix
```

---

### 2. GitHub Flow

**Philosophy**: Simplified workflow with only one main branch and feature branches.

```
┌──────────────────────────────────────────────────────────────────────┐
│  GITHUB FLOW STRUCTURE (Simple & Continuous)                         │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  main:      ●─────●─────────────●─────────────●─────────────●        │
│             A     B             C             D             E        │
│             │     ↑             ↑             ↑             ↑        │
│             │   [main]       [main]       [main]       [main]       │
│             │     │             │             │             │        │
│             │     │             │             │             │        │
│  feature1:  └─────●─────●───────┘             │             │        │
│                   X     Y                     │             │        │
│                   │     └─────────────┐       │             │        │
│                   └─ feature/login    │       │             │        │
│                      (PR #42)         │       │             │        │
│                      ✓ Tests pass     │       │             │        │
│                      ✓ Reviewed       │       │             │        │
│                      ✓ Deployed       │       │             │        │
│                      ✓ Merged ────────┘       │             │        │
│                                               │             │        │
│  feature2:                          ●─────●───┘             │        │
│                                     W     V                 │        │
│                                     │     └──────────┐      │        │
│                                     └─ feature/api   │      │        │
│                                        (PR #43)      │      │        │
│                                        ✓ Merged ─────┘      │        │
│                                                             │        │
│  feature3:                                        ●─────●───┘        │
│                                                   Q     R            │
│                                                   │     └────────┐   │
│                                                   └─ feature/ui  │   │
│                                                      (PR #44)    │   │
│                                                      ✓ Merged ───┘   │
│                                                                      │
│  ┌────────────────────────────────────────────────────────────────┐  │
│  │ KEY PRINCIPLES:                                                │  │
│  │ • main is always deployable                                    │  │
│  │ • Feature branches created from main                           │  │
│  │ • Pull Requests for code review                                │  │
│  │ • Deploy to staging before merging                             │  │
│  │ • Merge triggers automatic production deployment               │  │
│  │ • Short-lived branches (days, not weeks)                       │  │
│  └────────────────────────────────────────────────────────────────┘  │
└──────────────────────────────────────────────────────────────────────┘
```

**Characteristics**:
- ✅ Simple and easy to understand
- ✅ Always deployable main branch
- ✅ Continuous deployment friendly
- ✅ Great for web applications
- ❌ Not suitable for versioned releases
- ❌ No support for production hotfixes on old versions

**Complete Workflow**:

```
┌──────────────────────────────────────────────────────────────────────┐
│  STEP 1: CREATE FEATURE BRANCH                                       │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│   main:       ●───────●                                              │
│               A       B                                              │
│                       ↑                                              │
│                    [main*]                                           │
│                       ↑                                              │
│                   [feature]  ← branch created                        │
│                                                                      │
│   $ git checkout -b feature/add-user-profile                         │
└──────────────────────────────────────────────────────────────────────┘

┌──────────────────────────────────────────────────────────────────────┐
│  STEP 2: MAKE COMMITS                                                │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│   main:       ●───────●                                              │
│               A       B                                              │
│                       ↑                                              │
│                    [main]                                            │
│                       │                                              │
│                       └───●───────●───────●                          │
│   feature:                C       D       E                          │
│                           │       │       ↑                          │
│                           │       │   [feature*]                     │
│                           │       │                                  │
│   Commits:                │       └─ "Add profile view"              │
│                           └───────── "Add profile API"               │
│                                                                      │
│   $ git commit -m "..." (multiple times)                             │
│   $ git push -u origin feature/add-user-profile                      │
└──────────────────────────────────────────────────────────────────────┘

┌──────────────────────────────────────────────────────────────────────┐
│  STEP 3: OPEN PULL REQUEST                                           │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│   ┌────────────────────────────────────────────────────┐             │
│   │  Pull Request #42                                  │             │
│   │  feature/add-user-profile → main                   │             │
│   ├────────────────────────────────────────────────────┤             │
│   │  Status Checks:                                    │             │
│   │  ✓ Tests passed (24/24)                            │             │
│   │  ✓ Code coverage 95%                               │             │
│   │  ✓ Linting passed                                  │             │
│   │  ✓ Security scan clean                             │             │
│   │                                                    │             │
│   │  Reviews:                                          │             │
│   │  ✓ @reviewer1 approved                             │             │
│   │  ✓ @reviewer2 approved                             │             │
│   │                                                    │             │
│   │  [Deploy to Preview] [Merge PR]                    │             │
│   └────────────────────────────────────────────────────┘             │
└──────────────────────────────────────────────────────────────────────┘

┌──────────────────────────────────────────────────────────────────────┐
│  STEP 4: DEPLOY TO STAGING/PREVIEW                                   │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│   Preview Environment: https://pr-42-preview.example.com             │
│                                                                      │
│   ┌─────────────────────────────┐                                    │
│   │  🚀 Deployment Successful   │                                    │
│   │                             │                                    │
│   │  Preview your changes:      │                                    │
│   │  • Manual QA testing        │                                    │
│   │  • Stakeholder review       │                                    │
│   │  • Production-like env      │                                    │
│   └─────────────────────────────┘                                    │
└──────────────────────────────────────────────────────────────────────┘

┌──────────────────────────────────────────────────────────────────────┐
│  STEP 5: MERGE TO MAIN                                               │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│   main:       ●───────●───────────────────●                          │
│               A       B                   M                          │
│                       │                 ╱ ↑                          │
│                       │               ╱ [main*]                      │
│                       │             ╱                                │
│   feature:            └───●───────●───────●                          │
│                           C       D       E                          │
│                                           ↑                          │
│                                       [feature]                      │
│                                                                      │
│   $ git checkout main                                                │
│   $ git merge --no-ff feature/add-user-profile                       │
│   or: Merge via GitHub UI (creates merge commit M)                   │
└──────────────────────────────────────────────────────────────────────┘

┌──────────────────────────────────────────────────────────────────────┐
│  STEP 6: AUTO-DEPLOY TO PRODUCTION                                   │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│   ┌─────────────────────────────────────┐                            │
│   │  🚀 CI/CD Pipeline Triggered        │                            │
│   │                                     │                            │
│   │  ✓ Build successful                 │                            │
│   │  ✓ Tests passed                     │                            │
│   │  ✓ Deploying to production...       │                            │
│   │  ✓ Deployment successful            │                            │
│   │                                     │                            │
│   │  Production: https://example.com    │                            │
│   │  Deployed commit: M (abc1234)       │                            │
│   └─────────────────────────────────────┘                            │
│                                                                      │
│   main branch → always reflects production                           │
└──────────────────────────────────────────────────────────────────────┘
```

**Commands**:
```bash
# Create feature branch
git checkout main
git pull origin main
git checkout -b feature/add-user-avatar

# Make changes and commit
git add .
git commit -m "Add user avatar functionality"

# Push and create Pull Request
git push -u origin feature/add-user-avatar
# Open PR on GitHub

# After approval, merge via GitHub UI or:
git checkout main
git merge --no-ff feature/add-user-avatar
git push origin main

# Cleanup
git branch -d feature/add-user-avatar
git push origin --delete feature/add-user-avatar
```

---

### 3. GitLab Flow

**Philosophy**: Combines Git Flow and GitHub Flow with environment branches.

```
┌──────────────────────────────────────────────────────────────────────┐
│  GITLAB FLOW: ENVIRONMENT BRANCHES                                   │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  Development → Staging → Production (Promotion workflow)             │
│                                                                      │
│  main          ●───●───●───●───●───●───●───●                         │
│  (develop)     A   B   C   D   E   F   G   H                         │
│                │   │   │   ↓       ↓       ↓                         │
│                │   │   │   │       │       │                         │
│  pre-          │   │   │   ●───────●───────┐                         │
│  production    │   │   │   D'      F'      │                         │
│  (staging)     │   │   │   ↓               ↓                         │
│                │   │   │   │               │                         │
│  production    │   │   │   │               ●                         │
│  (live)        │   │   │   │               F''                       │
│                │   │   │   │                                         │
│  feature/1  ───┴───●───●───┘                                         │
│                    B   C (merged to main)                            │
│                                                                      │
│  feature/2  ───────────┴───●───●                                     │
│                            E   F (merged to main)                    │
│                                                                      │
│  ┌────────────────────────────────────────────────────────────────┐  │
│  │ WORKFLOW:                                                      │  │
│  │ 1. Develop features on main                                    │  │
│  │ 2. Merge main → pre-production (staging tests)                 │  │
│  │ 3. Merge pre-production → production (after QA approval)       │  │
│  │ 4. Each environment can have its own commits (hotfixes)        │  │
│  └────────────────────────────────────────────────────────────────┘  │
└──────────────────────────────────────────────────────────────────────┘

┌──────────────────────────────────────────────────────────────────────┐
│  GITLAB FLOW: RELEASE BRANCHES                                       │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  Supports multiple versions in production simultaneously             │
│                                                                      │
│  main:        ●───●───●───●───●───●───●───●───●                      │
│               A   B   C   D   E   F   G   H   I                      │
│               │       │       │       │                              │
│               │       │       │       │                              │
│               ↓       │       │       │                              │
│  2.1-stable:  ●───●───●───●───●                                      │
│               A   B   C   C1  C2                                     │
│                       ↑   ↑   ↑                                      │
│                       │   │   └─ Backport security fix               │
│                       │   └───── Backport bug fix                    │
│                       │                                              │
│                       ↓                                              │
│  2.2-stable:          ●───●───●───●───●                              │
│                       C   D   E   E1  E2                             │
│                               ↑   ↑   ↑                              │
│                               │   │   └─ Backport fix                │
│                               │   └───── Critical patch               │
│                               │                                      │
│                               ↓                                      │
│  2.3-stable:                  ●───●───●───●                          │
│                               E   F   G   G1                         │
│                                       ↑   ↑                          │
│                                       │   └─ Latest stable           │
│                                       │                              │
│                                       main (latest development)      │
│                                                                      │
│  ┌────────────────────────────────────────────────────────────────┐  │
│  │ USE CASE:                                                      │  │
│  │ • Enterprise customers on different versions                   │  │
│  │ • Mobile apps that can't force updates                         │  │
│  │ • Security fixes need to apply to all supported versions       │  │
│  │ • Cherry-pick or backport critical fixes to old versions       │  │
│  └────────────────────────────────────────────────────────────────┘  │
└──────────────────────────────────────────────────────────────────────┘
```

**Workflow Types**:

**A. Environment Branches**:
```bash
# Develop on main
git checkout main
git checkout -b feature/new-dashboard

# Merge to main
git checkout main
git merge feature/new-dashboard

# Promote to pre-production
git checkout pre-production
git merge main

# After testing, promote to production
git checkout production
git merge pre-production
```

**B. Release Branches**:
```bash
# Create release branch
git checkout -b 2.3-stable main

# Hotfix on old version
git checkout 2.2-stable
git checkout -b hotfix/security-fix
# ... make fix
git checkout 2.2-stable
git merge hotfix/security-fix

# Cherry-pick to other versions if needed
git checkout 2.3-stable
git cherry-pick <commit-hash>
```

---

### 4. Trunk-Based Development

**Philosophy**: Everyone commits to main (trunk), short-lived feature branches.

```
┌──────────────────────────────────────────────────────────────────────┐
│  TRUNK-BASED DEVELOPMENT: MINIMAL BRANCHING                          │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  main:    ●───●───●───●───●───●───●───●───●───●───●───●              │
│  (trunk)  A   B   C   D   E   F   G   H   I   J   K   L              │
│           │   ↑   │   ↑   │   ↑   │   ↑   │   ↑       ↑              │
│           │   │   │   │   │   │   │   │   │   │       │              │
│           │   │   │   │   │   │   │   │   │   │       │              │
│  Direct   └───┘   │   │   │   │   │   │   │   │       │              │
│  commits          │   │   │   │   │   │   │   │       │              │
│  (small changes)  │   │   │   │   │   │   │   │       │              │
│                   │   │   │   │   │   │   │   │       │              │
│  tiny/feature1 ───┴───●───┘   │   │   │   │   │       │              │
│  (< 1 day)            D'       │   │   │   │   │       │              │
│                                │   │   │   │   │       │              │
│  tiny/feature2 ────────────────┴───●───┘   │   │       │              │
│  (< 1 day)                         F'       │   │       │              │
│                                             │   │       │              │
│  tiny/bugfix ───────────────────────────────┴───●───┘   │              │
│  (< 4 hours)                                    H'       │              │
│                                                          │              │
│  tiny/hotfix ────────────────────────────────────────────●              │
│  (urgent, < 2 hours)                                     L'             │
│                                                                      │
│  ┌────────────────────────────────────────────────────────────────┐  │
│  │ KEY PRINCIPLES:                                                │  │
│  │ • Branches live < 24 hours (ideally < 4 hours)                 │  │
│  │ • Integrate to main multiple times per day                     │  │
│  │ • Small, atomic commits                                        │  │
│  │ • CI/CD runs on every commit                                   │  │
│  │ • Feature flags for incomplete work                            │  │
│  │ • High test coverage required                                  │  │
│  └────────────────────────────────────────────────────────────────┘  │
└──────────────────────────────────────────────────────────────────────┘

┌──────────────────────────────────────────────────────────────────────┐
│  FEATURE FLAGS FOR TRUNK-BASED DEVELOPMENT                           │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  main:    ●───●───●───●───●───●───●───●                              │
│           A   B   C   D   E   F   G   H                              │
│           │   │   │   │   │   │   │   │                              │
│           │   │   │   │   │   │   │   └─ Flag enabled (release!)    │
│           │   │   │   │   │   │   └───── Add checkout UI             │
│           │   │   │   │   │   └───────── Add payment API             │
│           │   │   │   │   └───────────── Initial flag (disabled)     │
│           │   │   │   └───────────────── Other feature               │
│           │   │   └───────────────────── Refactoring                 │
│           │   └───────────────────────── Bug fix                     │
│           └───────────────────────────── Base code                   │
│                                                                      │
│  ┌──────────────────────────────────────────────────────────────┐    │
│  │ CODE EXAMPLE:                                                │    │
│  │                                                              │    │
│  │ // Feature flag check                                       │    │
│  │ if (featureFlags.isEnabled('newCheckout')) {                │    │
│  │     return <NewCheckoutFlow />  // Incomplete, in progress  │    │
│  │ } else {                                                     │    │
│  │     return <OldCheckoutFlow />  // Stable, production       │    │
│  │ }                                                            │    │
│  │                                                              │    │
│  │ BENEFITS:                                                    │    │
│  │ • Incomplete features deployed but hidden                    │    │
│  │ • No long-lived branches                                     │    │
│  │ • Code integrated continuously                               │    │
│  │ • Enable/disable features without deployment                 │    │
│  │ • A/B testing and gradual rollouts                           │    │
│  └──────────────────────────────────────────────────────────────┘    │
└──────────────────────────────────────────────────────────────────────┘
```

**Key Principles**:
1. **Short-lived branches** (< 24 hours)
2. **Frequent integration** (multiple times per day)
3. **Feature flags** for incomplete features
4. **Automated testing** (CI/CD essential)
5. **Small commits** (easy to review)

**Commands**:
```bash
# Create very short-lived branch
git checkout main
git pull
git checkout -b tiny/update-button

# Quick change (< 1 day work)
# ... make small change
git commit -am "Update button styling"
git push

# Create PR, get quick review, merge same day
# Delete branch immediately after merge
```

---

### Strategy Comparison

| Strategy | Complexity | Branch Count | Release Model | Team Size | Best For |
|----------|-----------|--------------|---------------|-----------|----------|
| **Git Flow** | High | Many (5 types) | Versioned releases | Large | Traditional software, scheduled releases |
| **GitHub Flow** | Low | Few (main + features) | Continuous | Small-Medium | Web apps, continuous deployment |
| **GitLab Flow** | Medium | Moderate | Flexible | Medium-Large | Multiple environments, staged releases |
| **Trunk-Based** | Low | Minimal | Continuous | Any | High-performing teams, CI/CD mature |

---

## Conflict Resolution

Conflicts occur when Git cannot automatically merge changes because both branches modified the same lines of code.

### When Conflicts Occur

```
┌──────────────────────────────────────────────────────────────────────┐
│  CONFLICT SCENARIO: DIVERGENT CHANGES TO SAME FILE                   │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  INITIAL STATE (Common Ancestor):                                    │
│                                                                      │
│     ●───────●───────●                                                │
│     A       B       C                                                │
│                     │                                                │
│                     │  file.txt: "Hello World"                       │
│                     │                                                │
│                     ├─────────────────────────┐                      │
│                     │                         │                      │
│                     ▼                         ▼                      │
│                                                                      │
│  BRANCH 1 (main):                  BRANCH 2 (feature):               │
│                                                                      │
│     ●───────●───────●───────●         ●───────●───────●───────●      │
│     A       B       C       D         A       B       C       E      │
│                             ↑                                 ↑      │
│                          [main*]                          [feature]  │
│                             │                                 │      │
│     file.txt:               │         file.txt:               │      │
│     "Hello Universe"        │         "Hello Galaxy"          │      │
│     (changed line 1)        │         (changed line 1)        │      │
│                                                                      │
│  ⚠ CONFLICT: Both branches modified the same line!                   │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘

┌──────────────────────────────────────────────────────────────────────┐
│  ATTEMPTING MERGE                                                    │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  $ git checkout main                                                 │
│  $ git merge feature                                                 │
│                                                                      │
│  Output:                                                             │
│  ┌────────────────────────────────────────────────────────────────┐  │
│  │ Auto-merging file.txt                                          │  │
│  │ CONFLICT (content): Merge conflict in file.txt                 │  │
│  │ Automatic merge failed; fix conflicts and then commit result.  │  │
│  └────────────────────────────────────────────────────────────────┘  │
│                                                                      │
│  Git stopped and left conflict markers in file.txt                   │
│  You must resolve manually before completing the merge               │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

### Conflict Markers

```javascript
// file.txt after conflict

<<<<<<< HEAD (Current Change - main branch)
console.log("Hello Universe");
=======
console.log("Hello Galaxy");
>>>>>>> feature (Incoming Change - feature branch)
```

**Anatomy of Conflict Markers**:
```
<<<<<<< HEAD
[Your current branch changes]
=======
[Incoming branch changes]
>>>>>>> branch-name


Example with context:

function greet() {
<<<<<<< HEAD
    return "Good morning!";
=======
    return "Hello there!";
>>>>>>> feature/greeting
}
```

### Conflict Resolution Workflow

```
┌──────────────────────────────────────────────────────────────────────┐
│  STEP 1: ATTEMPT MERGE                                               │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│                       ┌───────●───────●                              │
│                       │       C       D                              │
│                       │               ↑                              │
│   Timeline:   ●───────●            [main*]                           │
│               A       B               │                              │
│                       │               │                              │
│                       └───────●───────●                              │
│                               C       E                              │
│                                       ↑                              │
│                                   [feature]                          │
│                                                                      │
│   $ git checkout main                                                │
│   $ git merge feature                                                │
│                                                                      │
│   ⚠ CONFLICT! Git cannot automatically merge                         │
└──────────────────────────────────────────────────────────────────────┘

┌──────────────────────────────────────────────────────────────────────┐
│  STEP 2: CHECK STATUS                                                │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│   $ git status                                                       │
│                                                                      │
│   Output:                                                            │
│   ┌────────────────────────────────────────────────────────────────┐ │
│   │ On branch main                                                 │ │
│   │ You have unmerged paths.                                       │ │
│   │   (fix conflicts and run "git commit")                         │ │
│   │   (use "git merge --abort" to abort the merge)                 │ │
│   │                                                                │ │
│   │ Unmerged paths:                                                │ │
│   │   (use "git add <file>..." to mark resolution)                 │ │
│   │                                                                │ │
│   │         both modified:   src/app.js                            │ │
│   │         both modified:   config/settings.json                  │ │
│   │                                                                │ │
│   │ no changes added to commit (use "git add")                     │ │
│   └────────────────────────────────────────────────────────────────┘ │
│                                                                      │
│   📋 2 files have conflicts that need manual resolution              │
└──────────────────────────────────────────────────────────────────────┘

┌──────────────────────────────────────────────────────────────────────┐
│  STEP 3: VIEW CONFLICTS                                              │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│   $ git diff                                                         │
│   $ cat src/app.js                                                   │
│                                                                      │
│   File contents with conflict markers:                               │
│   ┌────────────────────────────────────────────────────────────────┐ │
│   │ // config/settings.json                                        │ │
│   │                                                                │ │
│   │ {                                                              │ │
│   │   "appName": "MyApp",                                          │ │
│   │ <<<<<<< HEAD                                                   │ │
│   │   "version": "2.0",        ← Your changes (main)              │ │
│   │ =======                                                        │ │
│   │   "version": "1.5",        ← Their changes (feature)          │ │
│   │ >>>>>>> feature                                                │ │
│   │   "port": 3000                                                 │ │
│   │ }                                                              │ │
│   └────────────────────────────────────────────────────────────────┘ │
│                                                                      │
│   Conflict markers explained:                                        │
│   <<<<<<< HEAD          = Start of your changes (current branch)     │
│   =======               = Separator between changes                  │
│   >>>>>>> feature       = End of their changes (merging branch)      │
└──────────────────────────────────────────────────────────────────────┘

┌──────────────────────────────────────────────────────────────────────┐
│  STEP 4: RESOLVE CONFLICTS                                           │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│   Edit the file manually and choose the correct version:             │
│                                                                      │
│   OPTION A - Keep yours:        OPTION B - Keep theirs:              │
│   ┌──────────────────────┐      ┌──────────────────────┐            │
│   │ {                    │      │ {                    │            │
│   │   "appName": "MyApp",│      │   "appName": "MyApp",│            │
│   │   "version": "2.0",  │      │   "version": "1.5",  │            │
│   │   "port": 3000       │      │   "port": 3000       │            │
│   │ }                    │      │ }                    │            │
│   └──────────────────────┘      └──────────────────────┘            │
│                                                                      │
│   OPTION C - Combine both:      OPTION D - New solution:             │
│   ┌──────────────────────┐      ┌──────────────────────┐            │
│   │ {                    │      │ {                    │            │
│   │   "appName": "MyApp",│      │   "appName": "MyApp",│            │
│   │   "version": "2.1",  │      │   "version": "3.0",  │            │
│   │   "port": 3000       │      │   "port": 3000       │            │
│   │ }                    │      │ }                    │            │
│   └──────────────────────┘      └──────────────────────┘            │
│                                                                      │
│   Remove ALL conflict markers (<<<, ===, >>>)                        │
└──────────────────────────────────────────────────────────────────────┘

┌──────────────────────────────────────────────────────────────────────┐
│  STEP 5: STAGE RESOLVED FILES                                        │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│   $ git add src/app.js                                               │
│   $ git add config/settings.json                                     │
│                                                                      │
│   or add all resolved files:                                         │
│   $ git add .                                                        │
│                                                                      │
│   Check status again:                                                │
│   $ git status                                                       │
│   ┌────────────────────────────────────────────────────────────────┐ │
│   │ On branch main                                                 │ │
│   │ All conflicts fixed but you are still merging.                 │ │
│   │   (use "git commit" to conclude merge)                         │ │
│   │                                                                │ │
│   │ Changes to be committed:                                       │ │
│   │         modified:   src/app.js                                 │ │
│   │         modified:   config/settings.json                       │ │
│   └────────────────────────────────────────────────────────────────┘ │
└──────────────────────────────────────────────────────────────────────┘

┌──────────────────────────────────────────────────────────────────────┐
│  STEP 6: COMPLETE THE MERGE                                          │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│   $ git commit                                                       │
│                                                                      │
│   Git opens editor with default message:                             │
│   ┌────────────────────────────────────────────────────────────────┐ │
│   │ Merge branch 'feature' into main                               │ │
│   │                                                                │ │
│   │ # Conflicts:                                                   │ │
│   │ #       src/app.js                                             │ │
│   │ #       config/settings.json                                   │ │
│   └────────────────────────────────────────────────────────────────┘ │
│                                                                      │
│   Save and close editor to complete merge                            │
│                                                                      │
│   Result:                                                            │
│                       ┌───────●───────●─────────┐                    │
│                       │       C       D         │                    │
│                       │                         │                    │
│   Timeline:   ●───────●                         M                    │
│               A       B                       ╱ ↑                    │
│                       │                     ╱ [main*]                │
│                       │                   ╱                          │
│                       └───────●───────●─────                         │
│                               C       E                              │
│                                       ↑                              │
│                                   [feature]                          │
│                                                                      │
│   M = Merge commit containing conflict resolution                    │
│   ✓ Merge complete! Conflicts resolved.                              │
└──────────────────────────────────────────────────────────────────────┘
```

### Resolution Strategies

**1. Accept Current (Yours)**:
```bash
# Keep changes from current branch (main)
git checkout --ours <file>
git add <file>
```

**2. Accept Incoming (Theirs)**:
```bash
# Keep changes from incoming branch (feature)
git checkout --theirs <file>
git add <file>
```

**3. Manual Resolution**:
```bash
# Edit the file manually
vim <file>

# Remove conflict markers
# Keep desired code

# Stage the resolution
git add <file>
```

**4. Use Merge Tool**:
```bash
# Launch visual merge tool
git mergetool

# Popular tools: kdiff3, meld, vimdiff, VSCode
git config --global merge.tool vscode
git config --global mergetool.vscode.cmd 'code --wait $MERGED'
```

### Complex Conflict Example

```
Scenario: Both branches modified the same function

Common Ancestor (C):
┌─────────────────────────┐
│ function calculate(x) { │
│   return x * 2;         │
│ }                       │
└─────────────────────────┘

Branch 1 (main - D):
┌─────────────────────────┐
│ function calculate(x) { │
│   return x * 3;         │
│   // Updated multiplier │
│ }                       │
└─────────────────────────┘

Branch 2 (feature - E):
┌──────────────────────────────┐
│ function calculate(x, y) {   │
│   return x * 2 + y;          │
│   // Added second parameter  │
│ }                            │
└──────────────────────────────┘

Merge Conflict:
┌────────────────────────────────────┐
│ <<<<<<< HEAD                       │
│ function calculate(x) {            │
│   return x * 3;                    │
│   // Updated multiplier            │
│ =======                            │
│ function calculate(x, y) {         │
│   return x * 2 + y;                │
│   // Added second parameter        │
│ >>>>>>> feature                    │
│ }                                  │
└────────────────────────────────────┘

Resolution (combine both changes):
┌─────────────────────────────────────┐
│ function calculate(x, y = 0) {      │
│   return x * 3 + y;                 │
│   // Updated multiplier + parameter │
│ }                                   │
└─────────────────────────────────────┘
```

### Conflict Prevention Strategies

```
┌──────────────────────────────────────────────────────────────────────┐
│  STRATEGY 1: FREQUENT MERGING                                        │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  ❌ BAD: Infrequent merging (many conflicts)                         │
│                                                                      │
│  main:        ●───●───●───●───●───●───●───●                          │
│               A   B   C   D   E   F   G   H                          │
│               │                           ↑                          │
│               │                      [main]                          │
│               │                                                      │
│               └───●───●───●───●───●───●───┐                          │
│  feature:         X   Y   Z   W   V   U   │                          │
│                                           ↑                          │
│                                       [feature]                      │
│                                                                      │
│  Problem: 6 commits diverged, high chance of conflicts!              │
│  Feature branch is now 7 commits behind main                         │
│                                                                      │
│  ─────────────────────────────────────────────────────────────────   │
│                                                                      │
│  ✅ GOOD: Frequent merging (fewer conflicts)                         │
│                                                                      │
│  main:        ●───●───●───●───●───●───●───●                          │
│               A   B   C   D   E   F   G   H                          │
│               │   │   ↓   ↓   ↓   ↓   │   ↑                          │
│               │   │   │   │   │   │   │ [main]                       │
│               │   │   │   │   │   │   │                              │
│               └───●───●───●───●───●───●───┐                          │
│  feature:         X   Y   Z   W   V   U   │                          │
│                       ↑   ↑   ↑   ↑       ↑                          │
│                    merge merge merge   [feature]                     │
│                    main  main  main                                  │
│                                                                      │
│  Benefit: Regular sync with main, conflicts caught early!            │
│  Feature branch stays current with main                              │
└──────────────────────────────────────────────────────────────────────┘

┌──────────────────────────────────────────────────────────────────────┐
│  STRATEGY 2: SMALL, FOCUSED CHANGES                                  │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  ✅ GOOD: Small, focused branches                                    │
│                                                                      │
│  main:        ●───●───●───●───●───●                                  │
│               A   B   C   D   E   F                                  │
│                   │   │   │   │                                      │
│  feature/         └───●───┘   │                                      │
│  add-button           B'      │    (1 day, 2 commits)                │
│                               │                                      │
│  feature/             ┌───────●───┘                                  │
│  fix-header           │       E'   (1 day, 1 commit)                 │
│                       │                                              │
│  bugfix/              └───●───┘                                      │
│  typo-fix                 D'       (2 hours, 1 commit)               │
│                                                                      │
│  Benefits:                                                           │
│  • Easy to review                                                    │
│  • Less likely to conflict                                           │
│  • Fast to merge                                                     │
│  • Easy to revert if needed                                          │
│                                                                      │
│  ─────────────────────────────────────────────────────────────────   │
│                                                                      │
│  ❌ BAD: Large, unfocused branches                                   │
│                                                                      │
│  main:        ●───●───────────────────────────●                      │
│               A   B                           F                      │
│                   │                                                  │
│  feature/         └───●───●───●───●───●───●───┐                      │
│  redesign-            C   D   E   F   G   H   │                      │
│  entire-app                                    │                      │
│                                           (30 days, 35 commits)      │
│                                                                      │
│  Problems:                                                           │
│  • Hard to review (too much changed)                                 │
│  • High conflict probability                                         │
│  • Difficult to debug issues                                         │
│  • Risky to merge or revert                                          │
│  • Blocks other developers                                           │
└──────────────────────────────────────────────────────────────────────┘

┌──────────────────────────────────────────────────────────────────────┐
│  STRATEGY 3: TEAM COMMUNICATION                                      │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  ┌────────────────────────────────────────────────────────────────┐  │
│  │ BEFORE STARTING WORK:                                          │  │
│  │                                                                │  │
│  │ 1. Check existing branches                                     │  │
│  │    $ git branch -a                                             │  │
│  │                                                                │  │
│  │ 2. Communicate with team                                       │  │
│  │    "I'm working on user authentication"                        │  │
│  │    → Avoid duplicated effort                                   │  │
│  │    → Coordinate on overlapping areas                           │  │
│  │                                                                │  │
│  │ 3. Use feature flags for long-running features                 │  │
│  │    • Merge incomplete code with flag disabled                  │  │
│  │    • Avoid long-lived branches                                 │  │
│  │    • Enable when ready                                         │  │
│  │                                                                │  │
│  │ 4. Code reviews catch conflicts early                          │  │
│  │    • Reviewer may notice overlapping changes                   │  │
│  │    • Can coordinate before conflicts happen                    │  │
│  │                                                                │  │
│  │ 5. Pair programming on overlapping areas                       │  │
│  │    • Work together on shared code                              │  │
│  │    • No conflicts if working together                          │  │
│  └────────────────────────────────────────────────────────────────┘  │
└──────────────────────────────────────────────────────────────────────┘
```

### Aborting a Merge

```bash
# If conflicts are too complex, abort and try again
git merge --abort

# Returns to state before merge attempt
main:     A ─ B ─ C ─ D
                      ↑
                    (clean state restored)
```

---

## Advanced Branching Concepts

### 1. Rebase vs. Merge

**Merge** creates a new commit that combines histories:
```
┌──────────────────────────────────────────────────────────────────────┐
│  MERGE APPROACH: PRESERVES HISTORY                                   │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  BEFORE MERGE:                                                       │
│                                                                      │
│                       ┌───────●───────●                              │
│                       │       C       F                              │
│                       │               ↑                              │
│   Timeline:   ●───────●            [main*]                           │
│               A       B               │                              │
│                       │               │                              │
│                       └───────●───────●                              │
│                               D       E                              │
│                                       ↑                              │
│                                   [feature]                          │
│                                                                      │
│  Command: git checkout main && git merge feature                     │
│                                                                      │
│  ─────────────────────────────────────────────────────────────────   │
│                                                                      │
│  AFTER MERGE:                                                        │
│                                                                      │
│                       ┌───────●───────●─────────┐                    │
│                       │       C       F         │                    │
│                       │                         │                    │
│   Timeline:   ●───────●                         M                    │
│               A       B                       ╱ ↑                    │
│                       │                     ╱ [main*]                │
│                       │                   ╱                          │
│                       └───────●───────●─────                         │
│                               D       E                              │
│                                       ↑                              │
│                                   [feature]                          │
│                                                                      │
│  Result: M = merge commit with 2 parents (F and E)                   │
│                                                                      │
│  ┌────────────────────────────────────────────────────────────────┐  │
│  │ MERGE CHARACTERISTICS:                                         │  │
│  │ ✓ Preserves complete history (shows parallel development)     │  │
│  │ ✓ Non-destructive (original commits unchanged)                │  │
│  │ ✓ Safe for public/shared branches                             │  │
│  │ ✓ Clear feature boundaries in history                         │  │
│  │ ✗ Can create complex history graph with many merges           │  │
│  │ ✗ "Diamond" pattern can be hard to read                       │  │
│  └────────────────────────────────────────────────────────────────┘  │
└──────────────────────────────────────────────────────────────────────┘
```

**Rebase** replays commits on top of another branch:
```
┌──────────────────────────────────────────────────────────────────────┐
│  REBASE APPROACH: REWRITES HISTORY                                   │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  BEFORE REBASE:                                                      │
│                                                                      │
│                       ┌───────●───────●                              │
│                       │       C       F                              │
│                       │               ↑                              │
│   Timeline:   ●───────●            [main]                            │
│               A       B               │                              │
│                       │               │                              │
│                       └───────●───────●                              │
│                               D       E                              │
│                                       ↑                              │
│                                   [feature*]                         │
│                                                                      │
│  Command: git checkout feature && git rebase main                    │
│                                                                      │
│  ─────────────────────────────────────────────────────────────────   │
│                                                                      │
│  AFTER REBASE:                                                       │
│                                                                      │
│   Timeline:   ●───────●───────●───────●───────●───────●              │
│               A       B       C       F       D'      E'             │
│                                       ↑               ↑              │
│                                    [main]        [feature*]          │
│                                                                      │
│  Old commits (now abandoned):                                        │
│               ●───────●───────●   (these are no longer referenced)   │
│               A       B       D E                                    │
│                                                                      │
│  Result: D' and E' are NEW commits                                   │
│          • Same changes as D and E                                   │
│          • Different commit hashes                                   │
│          • Different parent commits                                  │
│          • Linear history (no merge commit)                          │
│                                                                      │
│  ┌────────────────────────────────────────────────────────────────┐  │
│  │ REBASE CHARACTERISTICS:                                        │  │
│  │ ✓ Creates clean, linear history                                │  │
│  │ ✓ Easier to understand (straight line)                         │  │
│  │ ✓ No merge commits cluttering history                          │  │
│  │ ✓ Excellent for local feature branches                         │  │
│  │ ✗ Rewrites history (changes commit hashes)                     │  │
│  │ ✗ Dangerous for public/shared branches                         │  │
│  │ ✗ Loses information about parallel development                 │  │
│  │ ⚠ NEVER rebase commits pushed to shared repository             │  │
│  └────────────────────────────────────────────────────────────────┘  │
└──────────────────────────────────────────────────────────────────────┘
```

**Command Comparison**:
```bash
# Merge approach
git checkout main
git merge feature

# Rebase approach
git checkout feature
git rebase main
git checkout main
git merge feature  # Now a fast-forward merge
```

**Rebase Visual Workflow**:
```
┌──────────────────────────────────────────────────────────────────────┐
│  STEP 1: INITIAL STATE (DIVERGED BRANCHES)                           │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│                       ┌───────●───────●───────●                      │
│                       │       C       F       G                      │
│                       │                       ↑                      │
│   Timeline:   ●───────●                    [main]                    │
│               A       B                                              │
│                       │                                              │
│                       └───────●───────●───────●                      │
│                               C       D       E                      │
│                                               ↑                      │
│                                          [feature*]                  │
│                                                                      │
│   Branches diverged at commit C:                                     │
│   • main added: F, G                                                 │
│   • feature added: D, E                                              │
│                                                                      │
│   Goal: Get feature branch changes on top of latest main             │
└──────────────────────────────────────────────────────────────────────┘

┌──────────────────────────────────────────────────────────────────────┐
│  STEP 2: REBASE FEATURE ONTO MAIN                                    │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│   $ git checkout feature                                             │
│   $ git rebase main                                                  │
│                                                                      │
│   Git performs these actions:                                        │
│   ┌────────────────────────────────────────────────────────────────┐ │
│   │ 1. Save commits D and E temporarily                            │ │
│   │ 2. Reset feature branch to main (commit G)                     │ │
│   │ 3. Replay commit D on top of G → creates D'                    │ │
│   │ 4. Replay commit E on top of D' → creates E'                   │ │
│   │ 5. Move feature pointer to E'                                  │ │
│   └────────────────────────────────────────────────────────────────┘ │
│                                                                      │
│   AFTER REBASE:                                                      │
│                                                                      │
│   Timeline:   ●───────●───────●───────●───────●───────●───────●      │
│               A       B       C       F       G       D'      E'     │
│                                               ↑               ↑      │
│                                            [main]        [feature*]  │
│                                                                      │
│   OLD commits D and E still exist in reflog but are not referenced   │
│   NEW commits D' and E' have same changes but different hashes       │
│                                                                      │
│   Note: Feature branch now has linear history from main!             │
└──────────────────────────────────────────────────────────────────────┘

┌──────────────────────────────────────────────────────────────────────┐
│  STEP 3: FAST-FORWARD MERGE TO MAIN                                  │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│   $ git checkout main                                                │
│   $ git merge feature                                                │
│                                                                      │
│   Output:                                                            │
│   ┌────────────────────────────────────────────────────────────────┐ │
│   │ Updating abc123..def456                                        │ │
│   │ Fast-forward                                                   │ │
│   │  2 files changed, 45 insertions(+), 12 deletions(-)           │ │
│   └────────────────────────────────────────────────────────────────┘ │
│                                                                      │
│   AFTER MERGE:                                                       │
│                                                                      │
│   Timeline:   ●───────●───────●───────●───────●───────●───────●      │
│               A       B       C       F       G       D'      E'     │
│                                                               ↑      │
│                                                           [main*]    │
│                                                          [feature]   │
│                                                                      │
│   Result: Clean, linear history! ✓                                   │
│                                                                      │
│   ┌────────────────────────────────────────────────────────────────┐ │
│   │ BENEFITS OF THIS WORKFLOW:                                     │ │
│   │                                                                │ │
│   │ ✓ No merge commit created                                      │ │
│   │ ✓ Straight line history (easy to read)                         │ │
│   │ ✓ git log shows commits in logical order                       │ │
│   │ ✓ git bisect works more efficiently                            │ │
│   │ ✓ Looks like feature was developed after F and G               │ │
│   │                                                                │ │
│   │ CLEANUP:                                                       │ │
│   │ $ git branch -d feature  (delete feature branch)               │ │
│   └────────────────────────────────────────────────────────────────┘ │
└──────────────────────────────────────────────────────────────────────┘
```

**When to Use Each**:

| Scenario | Use Merge | Use Rebase |
|----------|-----------|------------|
| Public/shared branches | ✅ | ❌ |
| Local feature branches | ✅ | ✅ |
| Want to preserve history | ✅ | ❌ |
| Want clean linear history | ❌ | ✅ |
| Team collaboration | ✅ | ⚠️ (careful) |

**Golden Rule**: Never rebase commits that have been pushed to a shared repository.

---

### 2. Interactive Rebase

**Purpose**: Rewrite commit history before sharing.

```bash
# Rebase last 3 commits
git rebase -i HEAD~3
```

**Interactive Options**:
```
pick  abc1234  Add login feature
pick  def5678  Fix typo
pick  ghi9012  Update tests

Commands:
p, pick   = use commit
r, reword = use commit, but edit message
e, edit   = use commit, but stop for amending
s, squash = meld into previous commit
f, fixup  = like squash, but discard message
d, drop   = remove commit
```

**Example: Squashing Commits**:
```
┌──────────────────────────────────────────────────────────────────────┐
│  BEFORE INTERACTIVE REBASE (MESSY HISTORY)                           │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  feature:   ●───●───●───●───●───●───●                                │
│             A   B   C   D   E   F   G                                │
│                 │   │   │   │   │   │                                │
│                 │   │   │   │   │   └─ "Fix typo again"             │
│                 │   │   │   │   └───── "Fix typo"                   │
│                 │   │   │   └───────── "Add feature"                │
│                 │   │   └───────────── "WIP - not done"             │
│                 │   └───────────────── "Start feature"              │
│                 └───────────────────── "Initial commit"             │
│                                                                      │
│   Problem: 5 messy commits (C, D, E, F, G) for one feature           │
│   Goal: Squash into single, clean commit                             │
└──────────────────────────────────────────────────────────────────────┘

┌──────────────────────────────────────────────────────────────────────┐
│  INTERACTIVE REBASE COMMAND                                           │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│   $ git rebase -i HEAD~5                                             │
│   or                                                                 │
│   $ git rebase -i B    (rebase from commit B)                        │
│                                                                      │
│   Git opens editor with:                                             │
│   ┌────────────────────────────────────────────────────────────────┐ │
│   │ pick  abc123  Start feature                                    │ │
│   │ pick  def456  WIP - not done                                   │ │
│   │ pick  ghi789  Add feature                                      │ │
│   │ pick  jkl012  Fix typo                                         │ │
│   │ pick  mno345  Fix typo again                                   │ │
│   │                                                                │ │
│   │ # Commands:                                                    │ │
│   │ # p, pick   = use commit                                       │ │
│   │ # r, reword = use commit, but edit message                     │ │
│   │ # e, edit   = use commit, but stop for amending               │ │
│   │ # s, squash = use commit, meld into previous                   │ │
│   │ # f, fixup  = like squash, discard message                     │ │
│   │ # d, drop   = remove commit                                    │ │
│   └────────────────────────────────────────────────────────────────┘ │
│                                                                      │
│   Change to:                                                         │
│   ┌────────────────────────────────────────────────────────────────┐ │
│   │ pick  abc123  Start feature                                    │ │
│   │ squash def456  WIP - not done                                  │ │
│   │ squash ghi789  Add feature                                     │ │
│   │ squash jkl012  Fix typo                                        │ │
│   │ squash mno345  Fix typo again                                  │ │
│   └────────────────────────────────────────────────────────────────┘ │
│                                                                      │
│   Save and close editor                                              │
└──────────────────────────────────────────────────────────────────────┘

┌──────────────────────────────────────────────────────────────────────┐
│  AFTER SQUASHING (CLEAN HISTORY)                                     │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  feature:   ●───●───●                                                │
│             A   B   X                                                │
│                 │   ↑                                                │
│                 │ [feature]                                          │
│                 │                                                    │
│                 └─ "Add complete user authentication feature"       │
│                    • Start feature implementation                    │
│                    • Add authentication API                          │
│                    • Add login UI                                    │
│                    • Fix validation bugs                             │
│                                                                      │
│   Result: 5 commits → 1 clean commit                                 │
│                                                                      │
│   ┌────────────────────────────────────────────────────────────────┐ │
│   │ BENEFITS:                                                      │ │
│   │ ✓ Clean commit history                                         │ │
│   │ ✓ One commit per logical change                                │ │
│   │ ✓ Easier to review                                             │ │
│   │ ✓ Easier to revert                                             │ │
│   │ ✓ Professional git log                                         │ │
│   │                                                                │ │
│   │ WHEN TO USE:                                                   │ │
│   │ • Before pushing to remote                                     │ │
│   │ • Before creating pull request                                 │ │
│   │ • Cleaning up work-in-progress commits                         │ │
│   │ • NEVER after pushing to shared branch                         │ │
│   └────────────────────────────────────────────────────────────────┘ │
└──────────────────────────────────────────────────────────────────────┘
```

**Commands**:
```bash
git rebase -i HEAD~5

# In editor:
pick  commit1  Start feature
squash commit2  WIP
squash commit3  Add feature
squash commit4  Fix typo
squash commit5  Fix typo again

# Result: All 5 commits become 1
```

---

### 3. Cherry-Pick

**Purpose**: Apply specific commits from one branch to another.

```
┌──────────────────────────────────────────────────────────────────────┐
│  CHERRY-PICK SCENARIO: SELECTIVE COMMIT APPLICATION                  │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  BEFORE CHERRY-PICK:                                                 │
│                                                                      │
│  main:        ●───────●───────●───────●───────●                      │
│               A       B       C       D       E                      │
│                                               ↑                      │
│                                            [main*]                   │
│                                                                      │
│                                         Need this specific commit!   │
│                                                   ↓                  │
│  feature:     ●───────●───────●───────●───────●───────●───────●      │
│               A       B       X       Y       F       G       H      │
│                                               ↑               ↑      │
│                                          (bug fix)        [feature]  │
│                                                                      │
│   Scenario: Commit F contains a critical bug fix                     │
│   Goal: Apply F to main without merging entire feature branch        │
│                                                                      │
│   Command: git checkout main                                         │
│            git cherry-pick F                                         │
└──────────────────────────────────────────────────────────────────────┘

┌──────────────────────────────────────────────────────────────────────┐
│  AFTER CHERRY-PICK:                                                  │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  main:        ●───────●───────●───────●───────●───────●              │
│               A       B       C       D       E       F'             │
│                                                       ↑              │
│                                                    [main*]           │
│                                                       │              │
│                                   F' is a COPY of F   │              │
│                                   • Same changes      │              │
│                                   • Different hash    │              │
│                                   • Different parent  │              │
│                                                                      │
│                                               Original F still here  │
│                                                       ↓              │
│  feature:     ●───────●───────●───────●───────●───────●───────●      │
│               A       B       X       Y       F       G       H      │
│                                               ↑               ↑      │
│                                          (unchanged)      [feature]  │
│                                                                      │
│  Result: Bug fix applied to main without full merge                  │
│                                                                      │
│  ┌────────────────────────────────────────────────────────────────┐  │
│  │ COMMON USE CASES:                                              │  │
│  │                                                                │  │
│  │ 1. Backporting bug fixes to release branches                   │  │
│  │    main (v2.0) ──●── Bug fix                                   │  │
│  │    release/v1.9 ──●── Cherry-pick bug fix                      │  │
│  │                                                                │  │
│  │ 2. Applying hotfix to multiple versions                        │  │
│  │    Security fix → Cherry-pick to v1.8, v1.9, v2.0             │  │
│  │                                                                │  │
│  │ 3. Selectively applying features                               │  │
│  │    feature branch has commits A, B, C                          │  │
│  │    Only want B → Cherry-pick B to main                         │  │
│  │                                                                │  │
│  │ 4. Rescuing commits from abandoned branches                    │  │
│  │    Old branch has good commit → Cherry-pick before deletion    │  │
│  └────────────────────────────────────────────────────────────────┘  │
└──────────────────────────────────────────────────────────────────────┘

┌──────────────────────────────────────────────────────────────────────┐
│  CHERRY-PICK MULTIPLE COMMITS                                        │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  Scenario: Need commits F, G, H from feature                         │
│                                                                      │
│  feature:     ●───────●───────●───────●───────●───────●───────●      │
│               A       B       X       Y       F       G       H      │
│                                               └───────┴───────┘      │
│                                                  Want these!          │
│                                                                      │
│  Commands:                                                           │
│  ┌────────────────────────────────────────────────────────────────┐  │
│  │ # Cherry-pick range (F through H)                              │  │
│  │ $ git cherry-pick F^..H                                        │  │
│  │                                                                │  │
│  │ # Cherry-pick multiple specific commits                        │  │
│  │ $ git cherry-pick F G H                                        │  │
│  │                                                                │  │
│  │ # Cherry-pick without committing (review first)                │  │
│  │ $ git cherry-pick -n F                                         │  │
│  │ $ git status  # review changes                                 │  │
│  │ $ git commit                                                   │  │
│  └────────────────────────────────────────────────────────────────┘  │
│                                                                      │
│  Result:                                                             │
│  main:        ●───────●───────●───────●───────●                      │
│               ...     E       F'      G'      H'                     │
│                                               ↑                      │
│                                            [main*]                   │
└──────────────────────────────────────────────────────────────────────┘
```

**Commands**:
```bash
# Cherry-pick single commit
git checkout main
git cherry-pick <commit-hash>

# Cherry-pick range of commits
git cherry-pick <start-hash>^..<end-hash>

# Cherry-pick without committing (stage only)
git cherry-pick -n <commit-hash>
```

**Use Cases**:
- Backporting bug fixes to release branches
- Applying hotfix to multiple versions
- Selectively moving features between branches

---

### 4. Branch Tracking

**Local vs. Remote Branches**:
```
┌──────────────────────────────────────────────────────────────────────┐
│  THREE-TIER BRANCH ARCHITECTURE                                      │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  ┌────────────────────────────────────────────────────────────────┐  │
│  │ LOCAL REPOSITORY (Your Computer)                               │  │
│  ├────────────────────────────────────────────────────────────────┤  │
│  │                                                                │  │
│  │  Working Directory                                             │  │
│  │  └── src/                                                      │  │
│  │      └── app.js                                                │  │
│  │                                                                │  │
│  │  ┌──────────────────────────────────────────────────────────┐  │  │
│  │  │ Local Branches (refs/heads/)                             │  │  │
│  │  ├──────────────────────────────────────────────────────────┤  │  │
│  │  │  • main               ───→ commit abc123                 │  │  │
│  │  │  • feature/login      ───→ commit def456                 │  │  │
│  │  │  • bugfix/header      ───→ commit ghi789                 │  │  │
│  │  └──────────────────────────────────────────────────────────┘  │  │
│  │                                                                │  │
│  │  ┌──────────────────────────────────────────────────────────┐  │  │
│  │  │ Remote-Tracking Branches (refs/remotes/origin/)          │  │  │
│  │  ├──────────────────────────────────────────────────────────┤  │  │
│  │  │  • origin/main        ───→ commit abc000                 │  │  │
│  │  │  • origin/feature/login ──→ commit def456                │  │  │
│  │  │  • origin/develop     ───→ commit xyz999                 │  │  │
│  │  │                                                           │  │  │
│  │  │  (Read-only copies of remote branches)                   │  │  │
│  │  │  (Updated by: git fetch, git pull)                       │  │  │
│  │  └──────────────────────────────────────────────────────────┘  │  │
│  └────────────────────────────────────────────────────────────────┘  │
│                                                                      │
│                              ↕ Network                               │
│                      (git fetch, push, pull)                         │
│                                                                      │
│  ┌────────────────────────────────────────────────────────────────┐  │
│  │ REMOTE REPOSITORY (GitHub/GitLab/etc.)                         │  │
│  ├────────────────────────────────────────────────────────────────┤  │
│  │                                                                │  │
│  │  Remote Branches                                               │  │
│  │  ├── main               ───→ commit abc000                     │  │
│  │  ├── develop            ───→ commit xyz999                     │  │
│  │  ├── feature/login      ───→ commit def456                     │  │
│  │  └── release/v1.2       ───→ commit jkl111                     │  │
│  │                                                                │  │
│  │  (Source of truth for team collaboration)                     │  │
│  └────────────────────────────────────────────────────────────────┘  │
└──────────────────────────────────────────────────────────────────────┘
```

**Tracking Relationship**:
```
┌──────────────────────────────────────────────────────────────────────┐
│  BRANCH TRACKING RELATIONSHIPS                                       │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  ┌──────────────────┐         tracks         ┌──────────────────┐   │
│  │  Local Branch    │ ────────────────────→  │ Remote Branch    │   │
│  │                  │                         │                  │   │
│  │  main            │ ────────────────────→  │ origin/main      │   │
│  │  abc123          │     (upstream)          │ abc000           │   │
│  │                  │                         │                  │   │
│  │  Status:         │                         │                  │   │
│  │  ahead 2 ───────→                          │                  │   │
│  │  (2 local commits)                         │                  │   │
│  └──────────────────┘                         └──────────────────┘   │
│                                                                      │
│  $ git branch -vv  (view tracking info)                              │
│  ┌────────────────────────────────────────────────────────────────┐  │
│  │ * main         abc123 [origin/main: ahead 2] Latest commit    │  │
│  │   feature      def456 [origin/feature: behind 1] Feature work │  │
│  │   bugfix       ghi789 [origin/bugfix] Bug fix                 │  │
│  │   experiment   jkl012 No tracking                             │  │
│  └────────────────────────────────────────────────────────────────┘  │
│                                                                      │
│  ┌────────────────────────────────────────────────────────────────┐  │
│  │ TRACKING STATUS:                                               │  │
│  │                                                                │  │
│  │ • ahead 2     = You have 2 local commits not pushed           │  │
│  │ • behind 1    = Remote has 1 commit you don't have            │  │
│  │ • ahead 1, behind 2 = Both have diverged                      │  │
│  │ • No tracking = Local branch not connected to remote          │  │
│  └────────────────────────────────────────────────────────────────┘  │
│                                                                      │
│  ┌────────────────────────────────────────────────────────────────┐  │
│  │ COMMON COMMANDS:                                               │  │
│  │                                                                │  │
│  │ # Set tracking for existing branch                             │  │
│  │ $ git branch --set-upstream-to=origin/main main               │  │
│  │                                                                │  │
│  │ # Push and set tracking in one command                         │  │
│  │ $ git push -u origin feature/login                            │  │
│  │                                                                │  │
│  │ # Fetch updates from remote (updates origin/*)                 │  │
│  │ $ git fetch origin                                             │  │
│  │                                                                │  │
│  │ # Pull changes (fetch + merge)                                 │  │
│  │ $ git pull                                                     │  │
│  │                                                                │  │
│  │ # Push local changes to remote                                 │  │
│  │ $ git push origin main                                         │  │
│  └────────────────────────────────────────────────────────────────┘  │
└──────────────────────────────────────────────────────────────────────┘
```

**Commands**:
```bash
# Set upstream for current branch
git branch --set-upstream-to=origin/main

# Push and set upstream
git push -u origin feature-branch

# See tracking information
git branch -vv

# Fetch updates from remote
git fetch origin

# Pull changes (fetch + merge)
git pull origin main
```

---

### 5. Orphan Branches

**Purpose**: Create a branch with no history (blank slate).

```
┌──────────────────────────────────────────────────────────────────────┐
│  REGULAR BRANCH VS ORPHAN BRANCH                                     │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  REGULAR BRANCH (Shares History):                                    │
│                                                                      │
│  Timeline:   ●───────●───────●───────●───────●                       │
│              A       B       C       D       E                       │
│                              ↑               ↑                       │
│                           [main]         [feature]                   │
│                              │               │                       │
│                              └───────────────┘                       │
│                         All share history A-B-C                      │
│                                                                      │
│  $ git log feature                                                   │
│  Shows: E → D → C → B → A  (complete history)                        │
│                                                                      │
│  ═════════════════════════════════════════════════════════════════   │
│                                                                      │
│  ORPHAN BRANCH (Independent History):                                │
│                                                                      │
│  main:       ●───────●───────●───────●                               │
│              A       B       C       D                               │
│                                      ↑                               │
│                                   [main]                             │
│                                                                      │
│              NO CONNECTION ✗                                         │
│                                                                      │
│  gh-pages:   ●───────●───────●                                       │
│              X       Y       Z                                       │
│                              ↑                                       │
│                          [gh-pages]                                  │
│                                                                      │
│  $ git log gh-pages                                                  │
│  Shows: Z → Y → X  (independent history, no A-B-C-D)                 │
│                                                                      │
│  Use cases:                                                          │
│  • GitHub Pages (gh-pages branch)                                    │
│  • Documentation separate from code                                  │
│  • Completely different content in same repo                         │
│  • Starting fresh without old history                                │
└──────────────────────────────────────────────────────────────────────┘

┌──────────────────────────────────────────────────────────────────────┐
│  CREATING ORPHAN BRANCH                                              │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  STEP 1: Create orphan branch                                        │
│  $ git checkout --orphan gh-pages                                    │
│                                                                      │
│  Output:                                                             │
│  ┌────────────────────────────────────────────────────────────────┐  │
│  │ Switched to a new branch 'gh-pages'                            │  │
│  └────────────────────────────────────────────────────────────────┘  │
│                                                                      │
│  Note: All files from previous branch are still in working directory │
│        but NOT committed to new branch yet                           │
│                                                                      │
│  ─────────────────────────────────────────────────────────────────   │
│                                                                      │
│  STEP 2: Remove all files                                            │
│  $ git rm -rf .                                                      │
│                                                                      │
│  Now you have a clean slate with no files                            │
│                                                                      │
│  ─────────────────────────────────────────────────────────────────   │
│                                                                      │
│  STEP 3: Add new content                                             │
│  $ echo "# Documentation" > README.md                                │
│  $ echo "Hello World" > index.html                                   │
│  $ git add .                                                         │
│  $ git commit -m "Initial documentation commit"                      │
│                                                                      │
│  ─────────────────────────────────────────────────────────────────   │
│                                                                      │
│  RESULT: Two completely separate histories in same repository        │
│                                                                      │
│  main branch:                                                        │
│  ├── src/                                                            │
│  ├── tests/                                                          │
│  └── package.json                                                    │
│                                                                      │
│  gh-pages branch:                                                    │
│  ├── index.html                                                      │
│  ├── styles.css                                                      │
│  └── README.md                                                       │
│                                                                      │
│  No shared files, no shared history!                                 │
└──────────────────────────────────────────────────────────────────────┘
```

**Commands**:
```bash
# Create orphan branch
git checkout --orphan docs

# Remove all files from staging
git rm -rf .

# Start fresh
echo "# Documentation" > README.md
git add README.md
git commit -m "Initial docs commit"
```

**Use Cases**:
- GitHub Pages (gh-pages branch)
- Documentation separate from code
- Project templates

---

## Best Practices

### 1. Branch Naming Conventions

```
Standard Format:
<type>/<description>

Types:
- feature/    → new functionality
- bugfix/     → bug fixes
- hotfix/     → urgent production fixes
- release/    → release preparation
- docs/       → documentation
- test/       → test-related changes
- refactor/   → code refactoring
- chore/      → maintenance tasks

Examples:
✅ feature/user-authentication
✅ bugfix/login-error-handling
✅ hotfix/security-vulnerability-CVE-2024
✅ release/v2.1.0
✅ docs/api-documentation
✅ test/integration-tests
✅ refactor/database-layer
✅ chore/update-dependencies

❌ fix-stuff
❌ johns-branch
❌ temp
❌ new-feature-123456789-with-very-long-name
```

### 2. Commit Hygiene

```
┌──────────────────────────────────────────────────────────────────────┐
│  COMMIT HYGIENE: BAD VS GOOD                                         │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  ❌ BAD: Messy, unclear commit history                               │
│                                                                      │
│  feature:   ●───●───●───●───●───●───●───●───●                        │
│             A   B   C   D   E   F   G   H   I                        │
│                 │   │   │   │   │   │   │   │                        │
│                 │   │   │   │   │   │   │   └─ "fix"                │
│                 │   │   │   │   │   │   └───── "oops"               │
│                 │   │   │   │   │   └───────── "fix typo"           │
│                 │   │   │   │   └───────────── "WIP"                │
│                 │   │   │   └───────────────── "stuff"              │
│                 │   │   └───────────────────── "update"             │
│                 │   └───────────────────────── "add feature"        │
│                 └───────────────────────────── "asdf"               │
│                                                                      │
│  Problems:                                                           │
│  • Vague commit messages ("fix", "stuff", "WIP")                     │
│  • Too many commits for one feature (8 commits)                      │
│  • Hard to review history                                            │
│  • Impossible to understand what each commit does                    │
│  • Difficult to cherry-pick or revert specific changes               │
│  • Unprofessional git log                                            │
│                                                                      │
│  ═════════════════════════════════════════════════════════════════   │
│                                                                      │
│  ✅ GOOD: Clean, descriptive commit history                          │
│                                                                      │
│  feature:   ●───●───●───●                                            │
│             A   B   C   D                                            │
│                 │   │   │                                            │
│                 │   │   └─ "Add password reset functionality"       │
│                 │   │      • Add reset token generation             │
│                 │   │      • Add email notification                 │
│                 │   │      • Add reset password endpoint            │
│                 │   │                                                │
│                 │   └───── "Add user authentication with JWT"       │
│                 │          • Implement JWT generation               │
│                 │          • Add authentication middleware          │
│                 │          • Add login/logout endpoints             │
│                 │                                                    │
│                 └───────── "Add user authentication tests"          │
│                            • Unit tests for JWT                     │
│                            • Integration tests for login/logout     │
│                                                                      │
│  Benefits:                                                           │
│  ✓ Clear, descriptive commit messages                                │
│  ✓ Atomic commits (one logical change per commit)                    │
│  ✓ Easy to review and understand                                     │
│  ✓ Easy to cherry-pick or revert                                     │
│  ✓ Professional git log                                              │
│  ✓ Good documentation of development process                         │
└──────────────────────────────────────────────────────────────────────┘

┌──────────────────────────────────────────────────────────────────────┐
│  HOW TO CLEAN UP MESSY COMMITS                                       │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  Method 1: Interactive Rebase (before pushing)                       │
│  $ git rebase -i main                                                │
│  → Squash, reword, reorder commits                                   │
│                                                                      │
│  Method 2: Squash Merge (when merging)                               │
│  $ git merge --squash feature                                        │
│  $ git commit -m "Clear message describing entire feature"           │
│  → Combines all commits into one                                     │
│                                                                      │
│  Method 3: Soft Reset (start over)                                   │
│  $ git reset --soft main                                             │
│  $ git commit -m "Proper commit message"                             │
│  → Keeps changes, creates new clean commit                           │
│                                                                      │
│  ⚠ IMPORTANT: Only rewrite history before pushing to shared repos!   │
└──────────────────────────────────────────────────────────────────────┘
```

### 3. Branch Lifecycle Management

```
┌──────────────────────────────────────────────────────────────────────┐
│  HEALTHY BRANCH LIFECYCLE                                            │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  Phase:      CREATE   DEVELOP   REVIEW   TEST   MERGE   DELETE      │
│                │         │        │       │       │        │         │
│  Timeline:     │         │        │       │       │        │         │
│  Day 0 ────────●─────────┼────────┼───────┼───────┼────────┼─────    │
│                │         │        │       │       │        │         │
│  Day 1 ────────┼─────────●────────┼───────┼───────┼────────┼─────    │
│                │         │        │       │       │        │         │
│  Day 2 ────────┼─────────●────────┼───────┼───────┼────────┼─────    │
│                │         │        │       │       │        │         │
│  Day 3 ────────┼─────────┼────────●───────┼───────┼────────┼─────    │
│                │         │        │       │       │        │         │
│  Day 4 ────────┼─────────┼────────┼───────●───────●────────┼─────    │
│                │         │        │       │       │        │         │
│  Day 5 ────────┼─────────┼────────┼───────┼───────┼────────●─────    │
│                │         │        │       │       │        │         │
│                ↓         ↓        ↓       ↓       ↓        ↓         │
│                                                                      │
│  ┌────────────────────────────────────────────────────────────────┐  │
│  │ PHASE DETAILS:                                                 │  │
│  │                                                                │  │
│  │ 1. CREATE (Day 0)                                              │  │
│  │    $ git checkout -b feature/user-profile                      │  │
│  │    ✓ Branch from latest main                                   │  │
│  │    ✓ Use descriptive name                                      │  │
│  │                                                                │  │
│  │ 2. DEVELOP (Days 1-2)                                          │  │
│  │    $ git commit -m "..."  (regular commits)                    │  │
│  │    ✓ Make focused commits                                      │  │
│  │    ✓ Keep branch up to date (git pull main)                   │  │
│  │    ✓ Push regularly for backup                                 │  │
│  │                                                                │  │
│  │ 3. REVIEW (Day 3)                                              │  │
│  │    $ git push -u origin feature/user-profile                   │  │
│  │    $ gh pr create  (open Pull Request)                         │  │
│  │    ✓ Code review from peers                                    │  │
│  │    ✓ Address feedback                                          │  │
│  │                                                                │  │
│  │ 4. TEST (Day 4)                                                │  │
│  │    ✓ CI/CD pipeline runs                                       │  │
│  │    ✓ All tests pass                                            │  │
│  │    ✓ Code coverage acceptable                                  │  │
│  │                                                                │  │
│  │ 5. MERGE (Day 4)                                               │  │
│  │    $ git checkout main                                         │  │
│  │    $ git merge feature/user-profile                            │  │
│  │    ✓ Squash or merge commit                                    │  │
│  │    ✓ Push to remote                                            │  │
│  │                                                                │  │
│  │ 6. DELETE (Day 5)                                              │  │
│  │    $ git branch -d feature/user-profile                        │  │
│  │    $ git push origin --delete feature/user-profile             │  │
│  │    ✓ Clean up local and remote                                 │  │
│  └────────────────────────────────────────────────────────────────┘  │
│                                                                      │
│  Total: 5 days (typical for small-medium feature)                    │
└──────────────────────────────────────────────────────────────────────┘

┌──────────────────────────────────────────────────────────────────────┐
│  STALE BRANCH PROBLEM                                                │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  ❌ BAD: Long-lived, stale branch                                    │
│                                                                      │
│  ┌────────────────────────────────────────────────────────────────┐  │
│  │ Branch: feature/new-dashboard                                  │  │
│  ├────────────────────────────────────────────────────────────────┤  │
│  │ Created:        60 days ago                                    │  │
│  │ Last commit:    45 days ago                                    │  │
│  │ Behind main:    45 commits                                     │  │
│  │ Ahead of main:  12 commits                                     │  │
│  │ Conflicts:      23 files                                       │  │
│  │ Status:         ⚠ ABANDONED                                     │  │
│  └────────────────────────────────────────────────────────────────┘  │
│                                                                      │
│  Timeline:                                                           │
│  Day 0    ●─────────────────────────────────────────────────→  Now  │
│            ↑                                                    ↑    │
│         Created                                            60 days   │
│                                                                      │
│  main:    ●─●─●─●─●─●─●─●─●─●─●─●─●─●─●─●─●─●─●─●─●─●─●─●─●→       │
│           │                                              45 commits  │
│           │                                                          │
│  feature: └─●─●─●─●─●─●─●─●─●─●─●─●                                 │
│             12 commits    ↑                                          │
│                     Last activity                                    │
│                     (45 days ago)                                    │
│                                                                      │
│  Problems:                                                           │
│  ⚠ Massive merge conflicts (23 files)                                │
│  ⚠ Code is outdated and likely doesn't work                          │
│  ⚠ Original developer may have left or forgotten context             │
│  ⚠ Blocks branch name for new work                                   │
│  ⚠ Clutters repository with dead branches                            │
│                                                                      │
│  ┌────────────────────────────────────────────────────────────────┐  │
│  │ SOLUTIONS:                                                     │  │
│  │                                                                │  │
│  │ 1. Delete the branch                                           │  │
│  │    $ git branch -D feature/new-dashboard                       │  │
│  │    $ git push origin --delete feature/new-dashboard            │  │
│  │    → Start fresh if feature still needed                       │  │
│  │                                                                │  │
│  │ 2. Update and salvage (if feature is valuable)                 │  │
│  │    $ git checkout feature/new-dashboard                        │  │
│  │    $ git rebase main  (resolve conflicts)                      │  │
│  │    $ git push --force-with-lease                               │  │
│  │    → Requires significant effort                               │  │
│  │                                                                │  │
│  │ 3. Set branch lifecycle policies                               │  │
│  │    • Auto-delete branches after merge                          │  │
│  │    • Alert for branches > 30 days old                          │  │
│  │    • Require regular updates to main                           │  │
│  └────────────────────────────────────────────────────────────────┘  │
└──────────────────────────────────────────────────────────────────────┘
```

**Cleanup Commands**:
```bash
# Delete merged local branches
git branch --merged main | grep -v "main" | xargs git branch -d

# Delete remote-tracking refs for deleted remote branches
git fetch --prune

# List stale branches (not updated in 30 days)
git for-each-ref --sort=-committerdate refs/heads/ \
  --format='%(committerdate:short) %(refname:short)' | \
  awk '$1 < "'$(date -d '30 days ago' +%Y-%m-%d)'"'
```

### 4. Branch Protection Rules

```
Protected Branch Configuration:

main branch protection:
┌────────────────────────────────────────┐
│ ✅ Require pull request reviews        │
│ ✅ Require status checks to pass       │
│ ✅ Require branches to be up to date   │
│ ✅ Require conversation resolution     │
│ ✅ Require signed commits              │
│ ✅ Include administrators              │
│ ❌ Allow force pushes                  │
│ ❌ Allow deletions                     │
└────────────────────────────────────────┘


Git Commands:
# Prevent accidental force push
git config --global push.default simple

# Require explicit push
git config --global branch.autoSetupMerge false
```

### 5. Merge vs. Rebase Decision Tree

```
┌──────────────────────────────────────────────────────────────────────┐
│  MERGE VS REBASE DECISION FLOWCHART                                  │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│                    ┌─────────────────────────┐                       │
│                    │  Need to integrate      │                       │
│                    │  changes?               │                       │
│                    └────────────┬────────────┘                       │
│                                 │                                    │
│                                 ▼                                    │
│                    ┌─────────────────────────┐                       │
│                    │  Has this branch been   │                       │
│                    │  pushed to shared repo? │                       │
│                    └────────────┬────────────┘                       │
│                                 │                                    │
│                   ┌─────────────┴─────────────┐                      │
│                   │                           │                      │
│                  YES                          NO                     │
│                   │                           │                      │
│                   ▼                           ▼                      │
│      ┌────────────────────────┐   ┌────────────────────────┐        │
│      │  Is this a protected   │   │  Do you want linear    │        │
│      │  branch (main/develop)?│   │  history?              │        │
│      └──────────┬─────────────┘   └──────────┬─────────────┘        │
│                 │                             │                      │
│         ┌───────┴───────┐            ┌────────┴────────┐            │
│        YES              NO           YES               NO            │
│         │               │             │                │             │
│         ▼               ▼             ▼                ▼             │
│  ┌───────────┐  ┌────────────┐ ┌──────────┐  ┌──────────────┐      │
│  │ Use MERGE │  │ Ask team   │ │Use REBASE│  │  Use MERGE   │      │
│  │           │  │ preference │ │          │  │              │      │
│  │ ✓ Safe    │  │            │ │✓ Clean   │  │✓ Preserve    │      │
│  │ ✓ Standard│  │ Use MERGE  │ │✓ Linear  │  │  history     │      │
│  └───────────┘  └────────────┘ └──────────┘  └──────────────┘      │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘

┌──────────────────────────────────────────────────────────────────────┐
│  DECISION MATRIX                                                     │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  ┌────────────────────────────────────────────────────────────────┐  │
│  │ SCENARIO 1: Integrating feature into main                     │  │
│  ├────────────────────────────────────────────────────────────────┤  │
│  │ $ git checkout main                                            │  │
│  │ $ git merge feature                                            │  │
│  │                                                                │  │
│  │ ✅ CORRECT: main is public, always use merge                   │  │
│  │                                                                │  │
│  │ $ git checkout main                                            │  │
│  │ $ git rebase feature                                           │  │
│  │                                                                │  │
│  │ ❌ WRONG: Never rebase public branch (main)                    │  │
│  └────────────────────────────────────────────────────────────────┘  │
│                                                                      │
│  ┌────────────────────────────────────────────────────────────────┐  │
│  │ SCENARIO 2: Updating feature with latest main                 │  │
│  ├────────────────────────────────────────────────────────────────┤  │
│  │ $ git checkout feature                                         │  │
│  │ $ git rebase main                                              │  │
│  │                                                                │  │
│  │ ✅ GOOD: Feature is local, rebase for clean history            │  │
│  │                                                                │  │
│  │ $ git checkout feature                                         │  │
│  │ $ git merge main                                               │  │
│  │                                                                │  │
│  │ ✅ ALSO GOOD: Merge if you want to preserve parallel history   │  │
│  └────────────────────────────────────────────────────────────────┘  │
│                                                                      │
│  ┌────────────────────────────────────────────────────────────────┐  │
│  │ SCENARIO 3: Cleaning up local commits before PR               │  │
│  ├────────────────────────────────────────────────────────────────┤  │
│  │ $ git rebase -i main                                           │  │
│  │ # Squash, reorder, reword commits                             │  │
│  │                                                                │  │
│  │ ✅ EXCELLENT: Clean up before sharing                          │  │
│  │ Time: Before git push                                          │  │
│  └────────────────────────────────────────────────────────────────┘  │
│                                                                      │
│  ┌────────────────────────────────────────────────────────────────┐  │
│  │ SCENARIO 4: After pushing to shared feature branch            │  │
│  ├────────────────────────────────────────────────────────────────┤  │
│  │ $ git rebase -i main                                           │  │
│  │                                                                │  │
│  │ ⚠ DANGEROUS: Branch already pushed, others may have it         │  │
│  │ Only if:                                                       │  │
│  │ • You are 100% sure no one else has the branch                 │  │
│  │ • You use --force-with-lease                                   │  │
│  │ • You communicate with team                                    │  │
│  └────────────────────────────────────────────────────────────────┘  │
│                                                                      │
│  ┌────────────────────────────────────────────────────────────────┐  │
│  │ SCENARIO 5: Working on collaborative feature branch           │  │
│  ├────────────────────────────────────────────────────────────────┤  │
│  │ $ git checkout shared-feature                                  │  │
│  │ $ git merge main                                               │  │
│  │                                                                │  │
│  │ ✅ SAFE: Always merge when others work on same branch          │  │
│  │                                                                │  │
│  │ $ git checkout shared-feature                                  │  │
│  │ $ git rebase main                                              │  │
│  │                                                                │  │
│  │ ❌ DANGEROUS: Will cause conflicts for collaborators           │  │
│  └────────────────────────────────────────────────────────────────┘  │
└──────────────────────────────────────────────────────────────────────┘

┌──────────────────────────────────────────────────────────────────────┐
│  GOLDEN RULES                                                        │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  🔒 RULE 1: Never rebase public/shared branches                      │
│     If others might have the branch, use merge                       │
│                                                                      │
│  🧹 RULE 2: Use interactive rebase to clean local history            │
│     Before pushing, clean up your commits with git rebase -i         │
│                                                                      │
│  🔀 RULE 3: Always merge into main/master/develop                    │
│     Protected branches should only receive merges, never rebase      │
│                                                                      │
│  ⚡ RULE 4: Rebase local feature branches for linear history         │
│     Personal branches can be rebased for cleaner history             │
│                                                                      │
│  💬 RULE 5: When in doubt, ask your team                             │
│     Team preferences vary - follow your team's conventions           │
│                                                                      │
│  ⚠️  RULE 6: Force push with --force-with-lease, not --force        │
│     Safer option that checks remote hasn't changed                   │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

### 6. Branching Anti-Patterns

❌ **Long-Lived Feature Branches**:
```
Bad:
feature:  Created 3 months ago
          250 commits behind main
          23 merge conflicts

Solution: Break into smaller features, merge frequently
```

❌ **Too Many Active Branches**:
```
Bad:
$ git branch
  feature/project-1
  feature/project-2
  feature/old-idea
  feature/maybe-someday
  feature/experiment
  bugfix/something
  temp-branch
  test-123
  johns-work
  ...50 more branches

Solution: Delete unused branches, focus on few active ones
```

❌ **Committing Directly to Main**:
```
┌──────────────────────────────────────────────────────────────────────┐
│  ANTI-PATTERN: DIRECT COMMITS TO MAIN                                │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  ❌ BAD: Committing directly to main/master                          │
│                                                                      │
│  main:       ●───────●───────●───────●───────●───────●               │
│              A       B       C       D       E       F               │
│              ↑       ↑       ↑       ↑       ↑       ↑               │
│           initial  direct direct  direct  direct  direct            │
│                    commit commit  commit  commit  commit            │
│                      ↓       ↓       ↓       ↓       ↓               │
│                    No PR   No PR   No PR   No PR   No PR            │
│                                                                      │
│  Problems:                                                           │
│  ⚠ No code review                                                    │
│  ⚠ No CI/CD validation before merge                                  │
│  ⚠ Can break production instantly                                    │
│  ⚠ No discussion or feedback                                         │
│  ⚠ Hard to track what changed and why                                │
│  ⚠ Can't easily revert grouped changes                               │
│  ⚠ Violates team workflow                                            │
│                                                                      │
│  ═════════════════════════════════════════════════════════════════   │
│                                                                      │
│  ✅ GOOD: All changes via feature branches + PRs                     │
│                                                                      │
│  main:       ●─────────────────●─────────────────●─────────────●     │
│              A                 B                 C             D     │
│              ↑                 ↑                 ↑             ↑     │
│           initial          merge PR#42      merge PR#43   merge PR#44│
│              │                 │                 │             │     │
│              │                 │                 │             │     │
│  feature/    │                 │                 │             │     │
│  login:      └────●────●───────┘                 │             │     │
│                   │    │                         │             │     │
│                  dev  dev                        │             │     │
│                   ↓    ↓                         │             │     │
│                PR #42 opened                     │             │     │
│                ✓ Code review                     │             │     │
│                ✓ Tests pass                      │             │     │
│                ✓ Approved                        │             │     │
│                                                  │             │     │
│  feature/                                        │             │     │
│  payment:              ┌────●────●────●──────────┘             │     │
│                        │   dev  dev  fix                       │     │
│                        │    ↓    ↓    ↓                        │     │
│                       PR #43 opened                            │     │
│                       ✓ Code review                            │     │
│                       ✓ CI/CD passed                           │     │
│                                                                │     │
│  bugfix/                                   ┌────●────●─────────┘     │
│  header:                                   │   fix  test             │
│                                            │    ↓    ↓               │
│                                           PR #44 opened              │
│                                           ✓ Quick review             │
│                                           ✓ Tests pass               │
│                                                                      │
│  Benefits:                                                           │
│  ✓ All changes reviewed before merge                                 │
│  ✓ CI/CD validates every change                                      │
│  ✓ Discussion and context preserved in PR                            │
│  ✓ Easy to revert at PR level                                        │
│  ✓ Clear feature boundaries                                          │
│  ✓ Team collaboration and knowledge sharing                          │
│                                                                      │
│  ┌────────────────────────────────────────────────────────────────┐  │
│  │ HOW TO PREVENT:                                                │  │
│  │                                                                │  │
│  │ 1. Enable branch protection on main:                           │  │
│  │    • Require pull request reviews                              │  │
│  │    • Require status checks to pass                             │  │
│  │    • Restrict who can push                                     │  │
│  │                                                                │  │
│  │ 2. Use GitHub/GitLab branch protection rules                   │  │
│  │                                                                │  │
│  │ 3. Set up pre-push hooks                                       │  │
│  │    • Warn/block direct commits to protected branches           │  │
│  └────────────────────────────────────────────────────────────────┘  │
└──────────────────────────────────────────────────────────────────────┘
```

❌ **Meaningless Branch Names**:
```
Bad:
- temp
- test
- my-branch
- branch-123
- final
- final-final
- new

Good:
- feature/payment-integration
- bugfix/cart-calculation-error
- hotfix/security-patch-auth
- docs/api-endpoints
```

---

## Quick Reference

### Essential Commands Cheat Sheet

```bash
# CREATING BRANCHES
git branch <name>                 # Create branch
git checkout -b <name>            # Create and switch
git switch -c <name>              # Modern syntax

# SWITCHING BRANCHES
git checkout <name>               # Switch branch
git switch <name>                 # Modern syntax
git checkout -                    # Switch to previous branch

# VIEWING BRANCHES
git branch                        # List local
git branch -r                     # List remote
git branch -a                     # List all
git branch -v                     # With last commit
git branch -vv                    # With tracking info

# MERGING
git merge <branch>                # Merge into current
git merge --no-ff <branch>        # Force merge commit
git merge --squash <branch>       # Squash merge
git merge --abort                 # Abort merge

# REBASING
git rebase <branch>               # Rebase onto branch
git rebase -i <branch>            # Interactive rebase
git rebase --continue             # Continue after fix
git rebase --abort                # Abort rebase

# DELETING BRANCHES
git branch -d <name>              # Delete merged branch
git branch -D <name>              # Force delete
git push origin --delete <name>   # Delete remote

# TRACKING
git branch -u origin/<name>       # Set upstream
git push -u origin <name>         # Push and set upstream

# VIEWING HISTORY
git log --oneline --graph --all   # Visual history
git log <branch1>..<branch2>      # Commits in branch2 not in branch1

# CONFLICT RESOLUTION
git status                        # Show conflicts
git diff                          # View conflicts
git checkout --ours <file>        # Keep current version
git checkout --theirs <file>      # Keep incoming version
git mergetool                     # Launch merge tool

# ADVANCED
git cherry-pick <commit>          # Apply specific commit
git stash                         # Save work temporarily
git stash pop                     # Restore stashed work
```

### Merge Strategies Summary

```
Fast-Forward:
Command: git merge <branch>
When:    Linear history
Result:  No merge commit

Non-Fast-Forward:
Command: git merge --no-ff <branch>
When:    Want to preserve branch history
Result:  Always creates merge commit

Three-Way:
Command: git merge <branch>
When:    Divergent branches (automatic)
Result:  Merge commit with 2 parents

Squash:
Command: git merge --squash <branch>
When:    Want clean history
Result:  Single commit, no merge commit

Rebase:
Command: git rebase <branch>
When:    Want linear history
Result:  Replayed commits, no merge commit
```

---

## Conclusion

Mastering Git branching and merging is essential for modern software development. Key takeaways:

1. **Branches are lightweight** - create them freely
2. **Choose the right strategy** - match your workflow and team size
3. **Merge regularly** - avoid long-lived branches
4. **Keep history clean** - use rebase and squash appropriately
5. **Communicate with your team** - establish conventions
6. **Practice conflict resolution** - it becomes easier over time
7. **Protect important branches** - use branch protection rules
8. **Delete merged branches** - keep repository clean

Remember: The best branching strategy is the one your team understands and follows consistently.

---

## Additional Resources

### Visual Tools
- GitKraken - Visual Git client
- SourceTree - Free Git GUI
- Git Graph (VS Code extension)
- GitHub Desktop - Simple desktop client

### Learning Resources
- [Git Documentation](https://git-scm.com/doc)
- [Pro Git Book](https://git-scm.com/book)
- [Git Branching Interactive Tutorial](https://learngitbranching.js.org/)
- [Atlassian Git Tutorials](https://www.atlassian.com/git/tutorials)

### Practice
```bash
# Try Git branching interactively
# Visit: https://learngitbranching.js.org/

# Create a practice repository
mkdir git-practice
cd git-practice
git init
echo "Practice makes perfect!" > README.md
git add README.md
git commit -m "Initial commit"

# Now practice branching!
git checkout -b feature/practice
# ... experiment freely
```

---

*Last updated: 2026-02-21*
*Author: Comprehensive Git Guide*
*Version: 1.0*
