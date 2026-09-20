# Day 3 — Git Branching and Merging

## 1. What is a Git Branch?

A branch is a separate line of development in Git.

It allows us to work on a feature or change without directly modifying the main branch.

Example:

```text
main
  |
  A
  |
  B
  |
  C
```

Create a separate branch:

```text
main
  |
  A
  |
  B
  |
  C
  \
   feature-login
```

Branches are useful for:

* New features
* Bug fixes
* Experiments
* Separate development work

---

## 2. Viewing Branches

```bash
git branch
```

Shows all local branches.

Example:

```text
  feature-login
* main
  feature-payment
```

The `*` indicates the branch currently checked out.

---

## 3. Creating a Branch

```bash
git branch feature-login
```

Creates a branch but does not switch to it.

To create and switch in one command:

```bash
git switch -c feature-login
```

This is the shortcut for:

```bash
git branch feature-login
git switch feature-login
```

---

## 4. Switching Between Branches

```bash
git switch main
```

Switches to `main`.

```bash
git switch feature-login
```

Switches to `feature-login`.

When switching branches, Git updates the working directory to match the selected branch.

---

## 5. Working Tree Behavior When Switching

If there are uncommitted changes, Git may still allow a branch switch if the changes will not be overwritten.

Example:

```text
uncommitted change
       |
git switch feature-payment
       |
changes move with you
```

But if switching would overwrite the local changes, Git blocks the operation.

Important rule:

> Git protects uncommitted work when changing branches.

---

## 6. Commits on Different Branches

A commit made on one branch does not automatically appear on another branch.

Example:

```text
main
 |
 A
 |
 B

feature-login
 |
 A
 |
 B
 |
 C
```

Commit `C` belongs to `feature-login`.

It does not automatically become part of `main`.

---

## 7. What is Git Merge?

Merging combines the work from one branch into another branch.

Example:

```bash
git switch main
git merge feature-login
```

This means:

> Bring the changes from `feature-login` into the current `main` branch.

Important:

The branch you are **currently on** is the branch that receives the merge.

```text
git switch main
git merge feature-login
        |
        └── feature-login → main
```

---

## 8. `Already up to date`

If you run:

```bash
git merge feature-login
```

and Git says:

```text
Already up to date.
```

it means the current branch already contains all commits from `feature-login`.

Git does not merge just because two branch names are different.

It checks whether there are commits that need to be brought into the current branch.

---

## 9. Merge Conflict

A merge conflict happens when Git cannot automatically decide how to combine changes.

A common situation:

```text
main
 |
 A
 |
 B
 |
 change to file.txt

feature-payment
 |
 A
 |
 B
 |
 different change to file.txt
```

When both branches modify the same part of a file, Git may report:

```text
CONFLICT (content): Merge conflict
```

---

## 10. Conflict Markers

Git marks the conflicting area in the file:

```text
<<<<<<< HEAD
content from current branch
=======
content from incoming branch
>>>>>>> feature-payment
```

Meaning:

```text
<<<<<<< HEAD
Current branch's version
=======
Incoming branch's version
>>>>>>> feature-payment
```

You must manually decide what the final file should contain.

---

## 11. Resolving a Merge Conflict

Basic workflow:

```bash
git merge feature-payment
```

If there is a conflict:

### Step 1 — Open the conflicting file

Inspect the conflict markers.

### Step 2 — Edit the file

Choose the correct content and remove:

```text
<<<<<<<
=======
>>>>>>>
```

### Step 3 — Stage the resolved file

```bash
git add git_intial.md
```

Important:

`git add` does not automatically solve the conflict.

You solve the conflict manually first.

`git add` tells Git:

> The conflict in this file has been resolved.

### Step 4 — Complete the merge

```bash
git commit -m "Merge feature-payment into main"
```

The merge is now complete.

---

## 12. Merge Commit

A merge can create a special commit with two parents.

Example:

```text
             feature-payment
                   |
                   C
                  / \
main ─── A ─── B ─── M
                  merge commit
```

The merge commit records that two lines of development were combined.

Example:

```text
8cdad53 Merge feature-payment into main
```

---

## 13. Reading Git History Graph

Useful command:

```bash
git log --oneline --graph --decorate --all
```

Symbols:

```text
*
```

represents a commit.

```text
|
```

represents continuation of history.

```text
|\
```

usually indicates a branch/merge relationship.

Example:

```text
*   8cdad53 (HEAD -> main) Merge feature-payment into main
|\
| * 0de01be (feature-payment) Add payment branch change
* | f935ee6 (origin/main) staged
|/
```

This shows that the merge commit brought together two lines of development.

---

## 14. Aborting a Merge

If a merge is in progress and you decide that you don't want to continue:

```bash
git merge --abort
```

This attempts to return the repository to the state before the merge started.

---

# 15. Practical Branching Workflow

A typical workflow:

```bash
git switch main
git pull
git switch -c feature-login
```

Work on the feature:

```bash
git add .
git commit -m "Add login feature"
```

When the feature is ready:

```bash
git switch main
git merge feature-login
```

If there is a conflict:

```text
merge
  ↓
conflict
  ↓
manually resolve
  ↓
git add <file>
  ↓
git commit
```

---

# 16. Example Project Branches

A real project might have:

```text
main
│
├── feature-login
├── feature-payment
├── feature-search
└── bugfix-cart
```

Each branch can represent a focused piece of work.

Example:

```text
feature-login
    ↓
Login development
    ↓
Commits
    ↓
Merge into main
```

---

# 17. Important Mental Model

Don't think:

> Every feature branch must always merge into main.

Instead think:

> Create a branch from the appropriate starting point, do one focused piece of work, then merge it into the branch where that work belongs.

---

# 18. Day 3 Quick Revision

### Branch

```bash
git branch
```

View branches.

### Create branch

```bash
git branch feature-name
```

### Create + switch

```bash
git switch -c feature-name
```

### Switch

```bash
git switch branch-name
```

### Merge

```bash
git switch main
git merge feature-name
```

### View history

```bash
git log --oneline --graph --decorate --all
```

### Abort merge

```bash
git merge --abort
```

### Resolve conflict

```text
1. Open conflicting file
2. Remove conflict markers
3. Choose/fix final content
4. git add <file>
5. git commit
```

---

# Day 3 Key Takeaways

1. A branch is a separate line of development.
2. `git switch` changes the current branch.
3. `git switch -c` creates and switches to a new branch.
4. Commits made on one branch don't automatically appear on another.
5. `git merge` brings another branch's work into the current branch.
6. The current branch receives the merge.
7. Merge conflicts happen when Git cannot automatically combine changes.
8. Conflict markers show the competing versions.
9. The developer manually resolves the conflict.
10. `git add` marks the resolved file.
11. `git commit` completes the merge.
12. A merge commit can have two parents.
13. `git merge --abort` cancels an in-progress merge.
14. `git log --graph` helps visualize branch history.
15. Branches keep different pieces of development separated.
