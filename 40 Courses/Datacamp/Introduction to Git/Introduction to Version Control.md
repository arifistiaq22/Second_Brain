---
tags:
  - version-control
  - git
  - terminal
  - basics
aliases:
  - Intro to Git
  - Version Control Basics
date: 2026-03-17
status: review
---
# Version Control & Git Fundamentals

## 1. What is Version Control?
Version control is a system that manages changes to files (documents, code, data) over time. Think of it as a highly detailed "save history" or a time machine for your project. 
* **Track:** Keeps a permanent record of every change made, when it was made, and who made it.
* **Compare:** Allows you to see exactly what was added, modified, or deleted between different versions of your work.
* **Revert:** Gives you the ability to "undo" mistakes by safely rolling back to a previous, working state.

## 2. Introduction to Git
**Git** is the industry standard tool for version control. 
* **Open-Source:** It is free, secure, and continuously improved by the developer community.
* **Scalable:** It is just as effective for a solo worker writing a tiny script as it is for massive global teams building enterprise software.

## 3. Interacting via the Shell (Terminal)
While graphical interfaces exist, the most common, powerful, and universally supported way to interact with Git is through the command line (the shell or terminal). 

Here are the foundational shell commands to navigate your computer before using Git:
* `pwd` **(Print Working Directory):** Tells you exactly which folder you are currently looking at in the terminal.
* `ls` **(List):** Displays all the files and folders inside your current directory.
* `cd` **(Change Directory):** Moves you into a different folder (e.g., typing `cd projects` moves you into the "projects" folder).
* `git --version`: A quick command to verify that Git is installed on your machine and to check which version is running.

## 4. Why it Matters
Using version control is essential for two primary reasons:
1. **Collaboration:** It allows multiple people to work on the exact same project simultaneously without accidentally overwriting each other's work.
2. **Project History:** It maintains a permanent, secure archive of your work's evolution, giving you the confidence to experiment without fear of breaking things.

# Understanding Git Repositories

## 1. What is a Git Repository?
A **Git repository** (often just called a "repo") is essentially your tracked workspace. It is a directory (folder) that contains all of your project's files, along with the complete history of every change made to those files. 

Every repository consists of two distinct parts:
1. **Your Project Files:** The actual documents, code, or data (and sub-directories) you are actively working on.
2. **The `.git` Directory:** A hidden folder created by Git that stores all the "extra information"—the actual version history, tracking data, and configuration settings.

## 2. Creating a Repository
Whether you are starting a brand-new project from scratch or want to start tracking an existing folder full of files, the process is exactly the same:

* **Initialize the Repo:** Open your terminal, navigate to your project's root directory, and run:
```bash
git init
```
_This creates the hidden `.git` folder, officially converting the directory into a Git repository._

- **Verify the Creation:** To check if the repository was successfully created and see its current state, run:
```bash
git status
```

## The Git Workflow

Working with Git is a three-step process: moving from your workspace to a permanent record.

### 1. Working Directory

This is where you do the actual work. You create, edit, and delete files on your computer just like normal.

### 2. Staging Area (`git add`)

Before you save a snapshot, you have to "stage" your changes.

- **The Analogy:** Like placing a letter in an envelope before sealing it.
- **Individual files:** `git add filename.txt`
- **Everything at once:** `git add .` (The dot represents the current directory and everything inside it).

### 3. Local Repository (`git commit`)

This creates a permanent snapshot of your project at that specific moment.

- **The Analogy:** Like mailing the envelope. Once it's sent, that version of the letter is "on the record."
- **The Command:** `git commit -m "Brief description of changes"`
- **Why use `-m`?** It adds a message so your future self (and your teammates) knows exactly why you made those changes.
### 4. Benefits of Committing

Once your changes are committed, you gain a safety net:

- **Compare:** See exactly what lines changed between Tuesday's version and Friday's version.
- **Revert:** If you break your code, you can "teleport" back to a previous commit when everything was working perfectly.

## The Anatomy of a Commit

When you commit, Git doesn't just save the files you changed; it creates a structured "package" consisting of three main layers:
### 1. The Metadata

Every commit contains "data about the data." This includes:

- **Author & Committer:** Who wrote the code and who saved it.
- **Timestamp:** Exactly when the commit was created.
- **Message:** Your description of the changes (the `-m` part).
- **Parent Hash:** A pointer to the previous commit, creating the "chain" of history.
### 2. The Tree (Directory Structure)

The **Tree** represents the folder structure. It maps file names to their contents. If you have a folder named `src/`, the tree knows that `main.py` lives inside it.
### 3. Blobs (File Contents)

Git stores the actual content of your files as **Blobs** (Binary Large Objects).

- **Efficiency Note:** If a file hasn't changed between commits, Git doesn't save it twice. It simply points the new tree to the existing blob from the previous commit.
## 4. Hashes: The Digital Fingerprint

Git doesn't use version numbers like "v1" or "v2." Instead, it uses **SHA-1 Hashes**.

- A hash looks like a long string of 40 characters (e.g., `5a2b3c4...`).
- It is unique: If even a single comma changes in a file, the hash will be completely different.
- This ensures data integrity—you can't change the past without Git noticing.
## 5. Viewing the Timeline

To see this structure in action, you use the `git log` command.

|**Command**|**Result**|
|---|---|
|`git log`|Shows a list of commits, their hashes, authors, and dates.|
|`git log --oneline`|A condensed view (one line per commit) for a quick overview.|
|`git log -p`|Shows the "patch" or the actual lines of code that changed.|

## Advanced Git Log Filtering

Instead of viewing everything, you can "query" your history using these specific flags.

### 1. Limiting the Output (`-n`)

If you only need a quick status check on recent work, use a number flag.

- **The Command:** `git log -n <number>`
- **Example:** `git log -3`
- **Result:** Displays only the **3 most recent** commits.
### 2. File-Specific History

If you want to know "Who changed this specific file and when?", just add the path.

- **The Command:** `git log <filename>`
- **Example:** `git log src/main.v`
- **Result:** Shows only the commits that touched that specific file.

### 3. Time-Based Filtering (`--since` & `--until`)

Git understands "human-friendly" date formats, which is incredibly powerful for tracking weekly or monthly progress.

- **By Date:** `git log --since="2026-03-01"`
- **By Duration:** `git log --since="2 weeks ago"`
- **Combined:** `git log --since="yesterday" --until="1 hour ago"`

### 4. Inspecting a Specific Snapshot (`git show`)

Once you find a commit hash (like `a1b2c3d`) using `log`, you can peek inside it to see the exact lines of code added or removed.

- **The Command:** `git show <hash>`
- **Example:** `git show 5a2b3c4`
- **Result:** Displays the metadata (author/date) and the **diff** (the specific `+` and `-` changes).
## 5. Quick Reference Table

|**Goal**|**Command**|
|---|---|
|**See last 5 commits**|`git log -5`|
|**Check work from last week**|`git log --since="1 week ago"`|
|**History for one file**|`git log path/to/file.txt`|
|**See contents of a commit**|`git show <hash>`|
|**Search by commit message**|`git log --grep="fix"`|

## The Levels of `git diff`

Depending on where your code is (Unstaged, Staged, or Committed), you'll use a different variation of the command.

### 1. Unstaged Changes (Working Directory)

If you’ve edited a file but haven't run `git add` yet, a simple `git diff` compares your current work against the last snapshot.

- **The Command:** `git diff`
- **What you see:** Red lines starting with `-` (deleted) and green lines starting with `+` (added).

### 2. Staged Changes (The Envelope)

Once you’ve staged your files with `git add`, a regular `git diff` will show nothing. You need a specific flag to peek inside the "envelope."

- **The Command:** `git diff --staged` (or `--cached`)
- **Purpose:** Verifies exactly what is about to be committed before you pull the trigger.

## 3. Comparing Across Time (Commits)

You can compare your current state to any point in the past using pointers or unique identifiers.

### 4. Using `HEAD` (The Pointer)

`HEAD` is an alias for your current "location" in the history.

- **Compare to the very last commit:** `git diff HEAD`
- **Compare to the commit before last:** `git diff HEAD~1`
- **Compare to three commits ago:** `git diff HEAD~3`

### 5. Using Hashes

If you want to compare two specific points in history (e.g., today's work vs. work from last month), use the commit hashes you found via `git log`.

- **The Command:** `git diff <old-hash> <new-hash>`
- **Example:** `git diff a1b2c3d e5f6g7h`

## 6. Summary Comparison Table

|**Command**|**What it compares**|
|---|---|
|`git diff`|Working directory vs. Staging area|
|`git diff --staged`|Staging area vs. Last commit|
|`git diff HEAD~1`|Working directory vs. Previous commit|
|`git diff hash1 hash2`|Two specific historical snapshots|
|`git diff --stat`|Shows a summary (which files changed) instead of full code|

