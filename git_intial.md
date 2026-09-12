# Git Learning — Day 1

## 1. What is Git?

**Git** is a Distributed Version Control System (VCS) used to track changes in files and projects.

### Why do we use Git?

* Tracks changes made to files.
* Maintains the history of a project.
* Allows us to go back to previous versions.
* Helps multiple developers work on the same project.
* Provides branches for developing different features.
* Helps recover from mistakes.

### Simple analogy

Git is like **save points in a game**.

Whenever we create a commit, Git saves a snapshot of the project that we can refer to later.

---

# 2. Git vs GitHub

| Git                                               | GitHub                                                          |
| ------------------------------------------------- | --------------------------------------------------------------- |
| Version Control System                            | Online platform for hosting Git repositories                    |
| Works mainly on the local computer                | Works online                                                    |
| Tracks project history                            | Stores/shares Git repositories online                           |
| Provides commands such as `git add`, `git commit` | Provides collaboration, repository hosting, pull requests, etc. |

### In simple words

**Git = tool for version control**

**GitHub = online platform where Git repositories can be stored and shared**

---

# 3. Git Repository

A **Git repository** is a project folder that is being tracked by Git.

We can create a repository using:

```bash
git init
```

This creates a hidden `.git` folder inside the project.

### What does `.git` contain?

It stores important Git information such as:

* Commit history
* Branch information
* Repository configuration
* References to commits
* Other internal Git data

### Check the repository root

```bash
git rev-parse --show-toplevel
```

This command shows the root directory of the current Git repository.

Example:

```text
C:/Users/deekshithvarala/practise python
```

---

# 4. Working Directory

The **working directory** is the actual project folder where we create, modify, and delete files.

For example:

```text
practise python/
│
├── git_learning.txt
├── Python programs
├── notes
└── other files
```

When we modify a file, the change first exists in the **working directory**.

Git can detect these changes using:

```bash
git status
```

### Important

If Git has never tracked a file before, it can appear as:

```text
Untracked files:
    git_learning.txt
```

**Untracked** means Git knows the file exists, but the file has not yet been added to the staging area.

---

# 5. Staging Area — `git add`

The **staging area** is where we select the changes that should be included in the next commit.

### Add one file

```bash
git add git_learning.txt
```

### Add multiple files / changes

```bash
git add .
```

The `.` means the current directory and its relevant changes.

### Important

`git add` does **NOT**:

* Create a commit
* Upload anything to GitHub

It only moves changes from the working directory to the staging area.

### Flow

```text
Working Directory
       │
       │ git add
       ▼
Staging Area
```

---

# 6. Commit — `git commit`

A **commit** is a saved snapshot of the staged changes in the local Git repository.

We create a commit using:

```bash
git commit -m "commit message"
```

Example:

```bash
git commit -m "to understand the git"
```

A successful commit may produce:

```text
[main 99f6903] to understand the git
```

Here:

* `main` → current branch
* `99f6903` → short commit ID
* `to understand the git` → commit message

### Important

A commit is saved **locally**.

It is NOT automatically uploaded to GitHub.

---

# 7. Complete Git Workflow

The basic Git workflow is:

```text
Working Directory
       │
       │ git add
       ▼
Staging Area
       │
       │ git commit
       ▼
Local Git Repository
       │
       │ git push
       ▼
GitHub
```

### Basic commands

```bash
git status
git add .
git commit -m "your message"
git push
```

### What each command does

| Command                   | Purpose                  |
| ------------------------- | ------------------------ |
| `git status`              | Check the current state  |
| `git add .`               | Stage changes            |
| `git commit -m "message"` | Create a local commit    |
| `git push`                | Upload commits to GitHub |

---

# 8. `git push` — Commit vs Files

One important concept learned today:

> **`git push` pushes commits, not simply files.**

For example, suppose we have:

```text
file1.txt → committed
file2.txt → staged but not committed
file3.txt → untracked
```

If we run:

```bash
git push
```

Only the **committed changes** are pushed to GitHub.

### Therefore:

```text
git add
   ↓
Staging only

git commit
   ↓
Creates a commit

git push
   ↓
Pushes the commit to GitHub
```

Even if we run:

```bash
git add .
```

the files will not be pushed until they are committed.

---

# 9. `git diff`

`git diff` shows the **exact changes that have not yet been staged**.

Example:

```bash
git diff
```

Possible output:

```diff
- Old line
+ New line
```

### Meaning

```text
- → removed/old line
+ → added/new line
```

### Important distinction

```bash
git diff
```

shows:

> **Unstaged changes**

while:

```bash
git diff --staged
```

shows:

> **Staged changes**

### Example workflow

```text
Working Directory
       │
       │ git diff
       ▼
See unstaged changes

       │
       │ git add
       ▼

Staging Area
       │
       │ git diff --staged
       ▼
See staged changes
```

### Important observation

After:

```bash
git add git_learning.txt
```

the changes are no longer unstaged.

Therefore:

```bash
git diff
```

may show nothing.

But:

```bash
git diff --staged
```

will show the changes waiting to be committed.

---

# 10. `git log`

`git log` displays the commit history of the repository.

### Full history

```bash
git log
```

This displays information such as:

* Commit ID
* Author
* Date
* Commit message

### Compact history

```bash
git log --oneline
```

Example:

```text
99f6903 to understand the git
a62e146 initial commit
```

### Show only the latest commit

```bash
git log --oneline -1
```

Example:

```text
99f6903 (HEAD -> main) to understand the git
```

### Meaning of `HEAD`

`HEAD` represents the current position in the Git history — normally the commit currently checked out on the current branch.

---

# 11. Understanding `git status`

`git status` is one of the most important Git commands.

Use it whenever you are unsure about the current state of your repository.

For example:

```bash
git status
```

It can tell us:

* Current branch
* Whether the branch is ahead/behind GitHub
* Untracked files
* Modified files
* Staged files
* Whether the working tree is clean

### Clean repository

A clean repository may show:

```text
Your branch is up to date with 'origin/main'.

nothing to commit, working tree clean
```

This means there are no uncommitted changes.

---

# 12. `origin` and `main`

### `main`

`main` is the name of the branch we are currently using.

Example:

```text
On branch main
```

### `origin`

`origin` is the conventional name Git gives to the remote repository.

For our project:

```text
origin → GitHub repository
```

We can check the remote using:

```bash
git remote -v
```

---

# 13. Important Git Concepts to Remember

### Working Directory

Where we actually create and modify files.

### Staging Area

Where we select changes for the next commit.

### Local Repository

Where commits and Git history are stored on our computer.

### Remote Repository

The repository hosted online, such as GitHub.

### Commit

A saved snapshot of staged changes.

### Push

Uploads local commits to the remote repository.

### Untracked File

A file Git sees but is not tracking yet.

### HEAD

Represents the current position in Git history.

---

# 14. Most Important Commands from Day 1

```bash
# Check repository status
git status

# Initialize a repository
git init

# Find repository root
git rev-parse --show-toplevel

# Stage one file
git add filename

# Stage all relevant changes
git add .

# Create a commit
git commit -m "message"

# Push commits to GitHub
git push

# View changes that are not staged
git diff

# View staged changes
git diff --staged

# View commit history
git log

# View compact commit history
git log --oneline

# View latest commit
git log --oneline -1

# View remote repository
git remote -v
```

---

# 15. Day 1 Quick Revision

Remember this single flow:

```text
                GIT WORKFLOW

      Create / Modify a File
                │
                ▼
       Working Directory
                │
             git add
                │
                ▼
          Staging Area
                │
           git commit
                │
                ▼
       Local Repository
                │
             git push
                │
                ▼
             GitHub
```

### Three commands to remember first

```bash
git add .
git commit -m "message"
git push
```

But before using them, always check:

```bash
git status
```

### Golden rule

> **Add → Commit → Push**

```text
git add
   ↓
git commit
   ↓
git push
    
    git is an 

