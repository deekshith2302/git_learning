# Git Day 2 
## 1. `git status`
Shows the current state of your repository.
```bash
git status
```
It tells you:

* Current branch
* Whether you are ahead/behind the remote
* Modified files
* Staged files
* Untracked files
* Whether the working tree is clean

### Important states

```text
Changes not staged for commit
```

→ File modified, but NOT staged.

```text
Changes to be committed
```

→ File is staged and ready to commit.

```text
nothing to commit, working tree clean
```

→ No pending changes.

---

# 2. `git add`

Moves changes from the **Working Directory → Staging Area**.

```bash
git add filename
```

Example:

```bash
git add git_intial.md
```

### Add multiple files

```bash
git add file1.txt file2.txt
```

### Add everything

```bash
git add .
```

### Important

`git add` does **NOT** create a commit.

It only prepares changes for the next commit.

```text
Working Directory
       ↓
    git add
       ↓
Staging Area
```

---

# 3. Selective Staging

You don't have to commit every modified file.

Example:

```text
app.py       modified
README.md    modified
notes.txt    modified
```

You can stage only:

```bash
git add app.py
```

Now:

```text
app.py       → staged
README.md    → unstaged
notes.txt    → unstaged
```

This is useful when you want to make **small, focused commits**.

---

# 4. `git commit`

Saves staged changes into the **local Git repository**.

```bash
git commit -m "message"
```

Example:

```bash
git commit -m "Update git learning notes"
```

### Important

Only **staged changes** are included in the commit.

```text
Working Directory
       ↓
    git add
       ↓
Staging Area
       ↓
   git commit
       ↓
Local Repository
```

### Good commit messages

Good:

```text
Add login validation
Fix password validation
Update README
Rename practice file
```

Avoid vague messages like:

```text
changes
update
stuff
done
```

---

# 5. `git diff`

Shows differences between versions of your files.

```bash
git diff
```

By default:

```text
Last Commit
     ↕
Working Directory
```

It shows **unstaged changes**.

### Example

If you modify a file but don't run `git add`:

```bash
git diff
```

shows what you changed.

---

# 6. `git diff --staged`

Shows changes that are currently in the **Staging Area**.

```bash
git diff --staged
```

Relationship:

```text
git diff
     ↓
Unstaged changes

git diff --staged
     ↓
Staged changes
```

### Very important

You can use this before committing to check:

> "What exactly am I about to commit?"

---

# 7. `git diff --cached`

Same purpose as:

```bash
git diff --staged
```

These are equivalent:

```bash
git diff --staged
```

```bash
git diff --cached
```

---

# 8. `git diff --stat`

Shows a **summary** instead of the complete changes.

```bash
git diff --stat
```

Example:

```text
git_intial.md | 2 ++
1 file changed, 2 insertions(+)
```

Instead of showing every changed line, it gives a compact overview.

---

# 9. Comparing Two Commits

You can compare any two commits:

```bash
git diff commit1 commit2
```

Example:

```bash
git diff dbdfd67 7dc3cf0
```

Meaning:

> Show the differences between these two commits.

Useful when investigating how a project changed over time.

---

# 10. `git restore`

Used to discard **unstaged changes** in a file.

```bash
git restore filename
```

Example:

```bash
git restore git_intial.md
```

Concept:

```text
Working Directory
       ↓
   git restore
       ↓
Discard unstaged changes
```

### ⚠️ Important

This can permanently discard your uncommitted changes.

Use it carefully.

---

# 11. `git restore --staged`

Removes a file from the **Staging Area** but keeps its changes in the Working Directory.

```bash
git restore --staged filename
```

Example:

```bash
git restore --staged git_intial.md
```

Concept:

```text
Staging Area
     ↓
git restore --staged
     ↓
Working Directory
```

The changes are **not deleted**.

They simply become unstaged.

---

# 12. Difference Between `restore` Commands

### `git restore file`

```bash
git restore file
```

➡️ Discards unstaged changes.

### `git restore --staged file`

```bash
git restore --staged file
```

➡️ Unstages the file but keeps the changes.

### Easy memory trick

```text
restore
    → REMOVE changes from working directory

restore --staged
    → REMOVE changes from staging area
```

---

# 13. `git rm`

Removes a **tracked file** from Git and stages the deletion.

```bash
git rm filename
```

Example:

```bash
git rm git_rm_practice.txt
```

Afterward:

```text
File deleted
     +
Deletion staged
```

Then commit:

```bash
git commit -m "Remove practice file"
```

### Difference from normal deletion

If you use:

```powershell
Remove-Item file.txt
```

Git sees:

```text
File deleted
→ unstaged deletion
```

But:

```bash
git rm file.txt
```

does:

```text
File deleted
+
Deletion staged
```

---

# 14. `git mv`

Used to rename or move a tracked file.

```bash
git mv old_name new_name
```

Example:

```bash
git mv git_mv_practice.txt git_mv_renamed.txt
```

Git automatically stages the rename.

Then:

```bash
git diff --staged
```

can show:

```text
similarity index 100%
rename from git_mv_practice.txt
rename to git_mv_renamed.txt
```

### Important

`similarity index 100%` means:

> The file contents are identical; only the name changed.

Then commit:

```bash
git commit -m "Rename practice file"
```

---

# 15. `git log --oneline`

Shows a compact commit history.

```bash
git log --oneline
```

Example:

```text
6639e75 Add git mv practice file
fe4eb06 Add git rm practice file
7dc3cf0 added some content
dbdfd67 learning basics
```

Each commit has:

```text
commit-hash + commit-message
```

Example:

```text
6639e75
```

is the shortened commit ID.

---

# 16. `HEAD`

`HEAD` points to the commit you are currently on.

Example:

```text
6639e75 (HEAD -> main)
```

Means:

> HEAD is currently pointing to commit `6639e75` on branch `main`.

Simple idea:

```text
HEAD
 ↓
main
 ↓
latest commit
```

---

# 17. Local Repository vs GitHub

This is one of the most important concepts.

### `git commit`

Saves changes locally:

```text
Working Directory
       ↓
Staging Area
       ↓
Local Repository
```

### `git push`

Sends local commits to GitHub:

```text
Local Repository
       ↓
   git push
       ↓
     GitHub
```

Therefore:

```text
git commit ≠ git push
```

Commit does NOT automatically upload to GitHub.

---

# 18. "Ahead of origin/main"

Example:

```text
Your branch is ahead of 'origin/main' by 5 commits.
```

Means:

```text
Local main
   │
   ├── Commit 1
   ├── Commit 2
   ├── Commit 3
   ├── Commit 4
   └── Commit 5
             ↓
       not on GitHub yet
```

To publish them:

```bash
git push
```

---

# 19. Complete Git Workflow

The most important workflow from Day 2:

```text
              Modify files
                   ↓
              git status
                   ↓
              git diff
                   ↓
              git add
                   ↓
         git diff --staged
                   ↓
              git commit
                   ↓
              git status
                   ↓
              git log
                   ↓
              git push
                   ↓
                GitHub
```

---

# 20. Quick Command Cheat Sheet

| Command                     | Purpose                  |
| --------------------------- | ------------------------ |
| `git status`                | Check repository state   |
| `git add file`              | Stage a file             |
| `git add .`                 | Stage all changes        |
| `git commit -m "msg"`       | Create a commit          |
| `git diff`                  | See unstaged changes     |
| `git diff --staged`         | See staged changes       |
| `git diff --cached`         | Same as `--staged`       |
| `git diff --stat`           | Show change summary      |
| `git diff A B`              | Compare two commits      |
| `git restore file`          | Discard unstaged changes |
| `git restore --staged file` | Unstage changes          |
| `git rm file`               | Delete + stage deletion  |
| `git mv old new`            | Rename/move + stage      |
| `git log --oneline`         | View commit history      |
| `git push`                  | Upload commits to GitHub |

---

# 🧠 The 4 Areas You Must Remember

```text
┌─────────────────────┐
│  Working Directory  │
└──────────┬──────────┘
           │
        git add
           ↓
┌─────────────────────┐
│    Staging Area     │
└──────────┬──────────┘
           │
       git commit
           ↓
┌─────────────────────┐
│   Local Repository  │
└──────────┬──────────┘
           │
        git push
           ↓
┌─────────────────────┐
│       GitHub        │
└─────────────────────┘
```

### 🔥 One-line memory trick

**`add → stage` | `commit → save locally` | `push → upload`**

And for checking:

**`status → what is happening`**

**`diff → what changed`**

**`diff --staged → what will be committed`**

**`log → what was committed`**
