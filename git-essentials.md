# Day 05 — Git Essentials

## 1. Git Stash

### What is `git stash`?

`git stash` temporarily stores our uncommitted changes so that we can work on something else without committing unfinished work.

Think of it as a **temporary drawer for your changes**.

```text
Working changes
      ↓
git stash
      ↓
Temporary storage
      ↓
Clean working directory
```

### Basic command

```powershell
git stash
```

This temporarily stores changes to tracked files.

### View stashes

```powershell
git stash list
```

Example:

```text
stash@{0}: WIP on main: ...
stash@{1}: WIP on feature-login: ...
```

### Restore the latest stash

```powershell
git stash pop
```

`pop` restores the changes **and removes the stash entry**.

### Restore but keep the stash

```powershell
git stash apply
```

Difference:

```text
git stash pop
→ restore + remove stash

git stash apply
→ restore + keep stash
```

### Include untracked files

Normally:

```powershell
git stash
```

doesn't include untracked files.

Use:

```powershell
git stash -u
```

to include them.

### Useful commands

```powershell
git stash list
git stash pop
git stash apply
git stash drop
git stash clear
git stash -u
```

### Real-world example

Suppose you're working on:

```text
feature-login
```

but suddenly need to switch to another branch.

Your changes aren't ready to commit.

You can:

```powershell
git stash
git switch main
```

Do the urgent work, then return:

```powershell
git switch feature-login
git stash pop
```

Your unfinished changes come back.

---

# 2. Git Rebase

## What is rebase?

`git rebase` moves/replays your branch's commits on top of another branch.

Example before rebase:

```text
A → B → C        main
     \
      D → E      feature
```

If the feature branch is rebased onto `main`:

```text
A → B → C → D' → E'     feature
```

The feature commits are replayed on top of the latest `main`.

### Command

```powershell
git switch feature
git rebase main
```

### Why use rebase?

It can produce a cleaner, more linear history.

Instead of:

```text
A → B → C
     \   \
      D → E
```

you can get:

```text
A → B → C → D → E
```

### Important warning

Rebase **rewrites commit history**.

The replayed commits normally receive new commit hashes.

Therefore:

> Avoid rebasing commits that other people are already depending on, especially shared/pushed history, unless the team workflow explicitly allows it.

### Merge vs Rebase

```text
Merge:
Preserves the existing branch history.

Rebase:
Replays commits and creates a more linear history.
```

---

# 3. Git Cherry-Pick

## What is cherry-pick?

`git cherry-pick` takes the changes from **one specific commit** and applies them to your current branch as a new commit.

Example:

```text
main:
A → B → C

feature:
A → B → D
```

Suppose we want only commit `D` on `main`.

We can:

```powershell
git switch main
git cherry-pick <commit-hash>
```

Result:

```text
main:
A → B → C → D'
```

`D'` contains the changes from `D`, but it is a **new commit**.

### Basic command

```powershell
git cherry-pick <commit-hash>
```

### Merge vs Cherry-pick

```text
git merge
→ brings a branch's history/changes together

git cherry-pick
→ takes a specific commit's changes
```

Cherry-pick is useful when you need **one particular fix** without bringing an entire branch into your current branch.

### Possible issue: empty cherry-pick

Sometimes Git says:

```text
The previous cherry-pick is now empty
```

This can happen when the changes from that commit are already present in the current branch.

In that situation:

```powershell
git cherry-pick --skip
```

can skip the empty cherry-pick.

---

# 4. Git Reset

`git reset` moves the current branch pointer to another commit.

Example:

```text
A → B → C → D
            ↑
           HEAD
```

If we run:

```powershell
git reset HEAD~1
```

the branch moves back:

```text
A → B → C
        ↑
       HEAD
```

There are three important reset modes.

---

## `git reset --soft`

```powershell
git reset --soft HEAD~1
```

Moves the branch backward but keeps the changes **staged**.

```text
Commit removed from branch
        ↓
Changes remain staged
```

Useful when you want to redo a commit.

---

## `git reset` / `--mixed`

```powershell
git reset HEAD~1
```

This is the default mode.

It moves the branch backward and keeps the changes in the working directory, but **unstaged**.

```text
Commit removed
      ↓
Changes remain
      ↓
Unstaged
```

---

## `git reset --hard`

```powershell
git reset --hard HEAD~1
```

Moves the branch backward and discards tracked working-tree changes associated with the reset.

```text
Commit removed
      ↓
Changes discarded
```

⚠️ This is dangerous.

Don't use `--hard` casually.

---

# 5. Git Revert

`git revert` is different from `reset`.

Instead of moving the branch backward, it creates a **new commit that reverses an earlier commit**.

Example:

```text
A → B → C
```

If we run:

```powershell
git revert C
```

we get:

```text
A → B → C → C-revert
```

The original commit `C` remains in history.

### Why use revert?

It is generally safer for commits that have already been pushed/shared.

```text
reset
→ moves history backward

revert
→ creates a new commit that undoes changes
```

### Simple rule

```text
Private/local history
→ reset can be useful

Shared/pushed history
→ revert is usually safer
```

---

# 6. Reset vs Revert

| Command            | What happens                                |
| ------------------ | ------------------------------------------- |
| `git reset --soft` | Move branch + keep changes staged           |
| `git reset`        | Move branch + keep changes unstaged         |
| `git reset --hard` | Move branch + discard tracked changes       |
| `git revert`       | Create a new commit reversing an old commit |

Mental model:

```text
RESET
"Move me back."

REVERT
"Create a new commit that undoes that change."
```

---

# 7. Git Reflog

## What is `git reflog`?

`git reflog` records movements of references such as `HEAD`.

It can help recover commits after operations such as:

```text
git reset
git rebase
```

or other history-changing operations.

### Command

```powershell
git reflog
```

Example:

```text
40c4558 HEAD@{0}: commit: get like that
c29028b HEAD@{1}: reset: moving to HEAD~1
3bd89bd HEAD@{2}: commit: Temporary hard reset practice
```

In our practice, we created:

```text
3bd89bd Temporary hard reset practice
```

Then performed a hard reset.

The commit disappeared from the normal branch history, but `reflog` still showed:

```text
3bd89bd HEAD@{2}
```

### Recovering a commit

If a commit was accidentally removed, we can potentially recover it using its hash:

```powershell
git reset --hard 3bd89bd
```

⚠️ Only do this when you're sure you want the branch to point there.

### Important difference

```text
git log
→ Shows commits reachable from the current history.

git reflog
→ Shows where HEAD and references have moved.
```

Think:

> `git log` = commit history
> `git reflog` = pointer movement history

---

# 8. `.gitignore`

## What is `.gitignore`?

`.gitignore` tells Git which **untracked files or patterns should be ignored**.

Example:

```text
*.log
.env
__pycache__/
.venv/
node_modules/
```

### Example

If `.gitignore` contains:

```text
*.log
```

then:

```text
debug.log
error.log
server.log
```

will be ignored.

### Create `.gitignore`

Example:

```powershell
Add-Content .gitignore "*.log"
```

Check ignored files:

```powershell
git status --ignored
```

You may see:

```text
Ignored files:
    debug.log
```

---

## Important: `.gitignore` doesn't untrack existing files

Suppose:

```powershell
git add secret.txt
git commit -m "Add secret"
```

Later you add:

```text
secret.txt
```

to `.gitignore`.

Git will still track `secret.txt`.

To stop tracking it while keeping the file locally:

```powershell
git rm --cached secret.txt
```

Then commit the change.

### Important rule

```text
.gitignore
→ prevents untracked files from being tracked

git rm --cached
→ removes an already-tracked file from Git's index
```

---

# 9. Git Clean

`git clean` removes **untracked files** from the working directory.

Example:

```text
tracked-file.txt
temporary.txt
```

If `temporary.txt` is untracked:

```powershell
git clean -n
```

shows:

```text
Would remove temporary.txt
```

`-n` means **dry run**.

It previews what would be removed without actually deleting anything.

### Delete untracked files

```powershell
git clean -f
```

### Include directories

```powershell
git clean -fd
```

### Preview ignored files too

```powershell
git clean -ndx
```

### Delete untracked + ignored files

```powershell
git clean -fdx
```

⚠️ Be extremely careful with `-fdx`.

It can delete ignored files/directories such as:

```text
.env
.venv/
build/
temporary files
```

### Important distinction

```text
git restore
→ undo changes in tracked files

git clean
→ remove untracked files
```

---

# 10. Detached HEAD

Normally:

```text
HEAD → main → commit
```

HEAD points to our current branch.

A detached HEAD happens when we directly move HEAD to a commit instead of a branch.

Example:

```powershell
git switch --detach <commit>
```

Now:

```text
HEAD → specific commit
```

instead of:

```text
HEAD → main → specific commit
```

We can inspect old commits without modifying the branch.

### Why can this be dangerous?

We can create commits while detached.

Those commits are not attached to a normal branch.

If we switch away without saving the work, the commits can become difficult to find later.

### Save the work

If you accidentally made useful work in detached HEAD:

```powershell
git switch -c save-my-work
```

This creates a branch pointing to the current commit.

### Mental model

```text
Detached HEAD
      ↓
Useful work?
      ↓
git switch -c save-my-work
      ↓
Work safely attached to a branch
```

---

# 11. Common Git Mistakes and Recovery

## Accidentally changed a tracked file

For uncommitted changes:

```powershell
git restore file.txt
```

This discards the working-tree changes to that file.

---

## Accidentally staged a file

```powershell
git restore --staged file.txt
```

This removes it from staging but keeps the changes in the working directory.

---

## Accidentally committed something locally

Depending on the situation:

```text
git reset
```

can be used to move local history backward.

---

## Already pushed the unwanted commit

Instead of rewriting shared history:

```powershell
git revert <commit>
```

is generally safer.

---

## Accidentally reset too far

Check:

```powershell
git reflog
```

Find the previous commit and recover it if appropriate.

---

## Unfinished work is blocking a branch switch

Use:

```powershell
git stash
```

Switch branches, work, then:

```powershell
git stash pop
```

---

# 12. Essential Git Command Map

```text
STATUS
git status
→ What is happening right now?

DIFF
git diff
→ What changed?

STAGE
git add
→ Prepare changes for commit.

COMMIT
git commit
→ Save changes to local history.

BRANCH
git switch -c branch-name
→ Create and switch to a branch.

MERGE
git merge branch-name
→ Combine branch histories.

STASH
git stash
→ Temporarily put unfinished changes aside.

REBASE
git rebase main
→ Replay branch commits on top of another branch.

CHERRY-PICK
git cherry-pick <hash>
→ Apply one specific commit's changes.

RESET
git reset
→ Move branch history.

REVERT
git revert <hash>
→ Create a commit that undoes another commit.

REFLOG
git reflog
→ Find previous HEAD/reference positions.

IGNORE
.gitignore
→ Ignore unwanted untracked files.

CLEAN
git clean
→ Remove untracked files.
```

---

# 13. Complete Git Workflow

A typical development workflow looks like this:

```text
                 GitHub
                   ↑
                  push
                   │
             ┌─────────────┐
             │ Local Repo  │
             └─────────────┘
                   ↑
                commit
                   ↑
                staging
                   ↑
             Working files
```

When starting work:

```powershell
git status
git pull
```

Create a branch:

```powershell
git switch -c feature-name
```

Make changes.

Check:

```powershell
git status
git diff
```

Stage:

```powershell
git add .
```

Commit:

```powershell
git commit -m "Describe the change"
```

Push:

```powershell
git push
```

If unfinished work needs to be temporarily moved:

```powershell
git stash
```

If you need to undo a shared commit:

```powershell
git revert <commit>
```

If you accidentally lose local history:

```powershell
git reflog
```

---

# 14. Day 5 Quick Revision

### Stash

```powershell
git stash
git stash list
git stash pop
git stash apply
```

Temporarily stores unfinished changes.

### Rebase

```powershell
git rebase main
```

Replays your branch commits on top of another branch.

### Cherry-pick

```powershell
git cherry-pick <commit>
```

Applies one specific commit's changes.

### Reset

```powershell
git reset --soft HEAD~1
git reset HEAD~1
git reset --hard HEAD~1
```

Moves the branch pointer backward with different effects on changes.

### Revert

```powershell
git revert <commit>
```

Creates a new commit that reverses an earlier commit.

### Reflog

```powershell
git reflog
```

Helps locate previous HEAD/reference positions and recover lost local commits.

### `.gitignore`

```text
*.log
.env
__pycache__/
.venv/
```

Ignores unwanted untracked files.

### Clean

```powershell
git clean -n
git clean -f
```

Preview/remove untracked files.

### Detached HEAD

```powershell
git switch --detach <commit>
```

HEAD points directly to a commit rather than a branch.

---

# 15. Most Important Git Rules to Remember

### Rule 1

```text
git add → staging
git commit → local history
git push → remote repository
```

### Rule 2

```text
git fetch → download remote information
git pull → fetch + integrate
```

### Rule 3

```text
git merge → combine histories
git rebase → replay commits
```

### Rule 4

```text
git reset → move history
git revert → undo through a new commit
```

### Rule 5

```text
git restore → undo working-file changes
git clean → remove untracked files
```

### Rule 6

```text
git reflog → rescue lost local history
```

### Rule 7

```text
.gitignore → ignore unwanted untracked files
```

---

# 🎯 Day 5 Completion

Day 5 covered:

* [x] `git stash`
* [x] `git rebase`
* [x] `git cherry-pick`
* [x] `git reset`
* [x] `git revert`
* [x] `git reflog`
* [x] `.gitignore`
* [x] `git clean`
* [x] Detached HEAD concept
* [x] Git mistake recovery
* [x] Complete Git workflow

## Final Git Mental Model

```text
                 ┌──────────────┐
                 │    GitHub    │
                 └──────┬───────┘
                        ↑
                      push
                        │
                      pull
                        │
                 ┌──────┴───────┐
                 │ Local Repo   │
                 │              │
                 │ branches     │
                 │ commits      │
                 │ history      │
                 └──────┬───────┘
                        ↑
                     commit
                        ↑
                      add
                        ↑
                 ┌──────┴───────┐
                 │ Working Tree │
                 └──────────────┘
```

> **Git = version control and history management.**
> **GitHub = remote hosting and collaboration.**

# 🏁 Git Training Complete

**5-Day Git Journey — COMPLETE ✅**

From Git fundamentals to:

```text
Git basics
   ↓
Staging & commits
   ↓
Branches
   ↓
Merging & conflicts
   ↓
Remote repositories
   ↓
Fetch / Pull / Push
   ↓
Stash
   ↓
Rebase
   ↓
Cherry-pick
   ↓
Reset / Revert
   ↓
Reflog
   ↓
.gitignore
   ↓
Clean
   ↓
Recovery
```

**Git fundamentals + practical essentials completed. 🚀**
