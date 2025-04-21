Here’s a handy **Git Cheat Sheet** that covers the most commonly used commands for day-to-day version control tasks:

---

## 🗂️ **Basic Git Commands**

| Command | Description |
|--------|-------------|
| `git init` | Initialize a new Git repository |
| `git clone <repo-url>` | Clone a remote repository |
| `git status` | Show the status of changes |
| `git add <file>` | Stage a file |
| `git add .` | Stage all changes |
| `git commit -m "message"` | Commit staged changes with a message |
| `git log` | View commit history |
| `git diff` | Show unstaged changes |
| `git diff --staged` | Show staged changes |
| `git config --global user.name "Name"` | Set Git username |
| `git config --global user.email "email@example.com"` | Set Git email |

---

## 🔄 **Branching & Merging**

| Command | Description |
|--------|-------------|
| `git branch` | List all branches |
| `git branch <branch-name>` | Create a new branch |
| `git checkout <branch-name>` | Switch to a branch |
| `git switch <branch-name>` | Newer way to switch branches |
| `git merge <branch>` | Merge another branch into current |
| `git branch -d <branch>` | Delete a local branch |
| `git checkout -b <new-branch>` | Create and switch to a new branch |

---

## 🌍 **Remote Repositories**

| Command | Description |
|--------|-------------|
| `git remote -v` | Show remote URLs |
| `git remote add origin <url>` | Add a remote repo |
| `git push origin <branch>` | Push branch to remote |
| `git pull` | Pull latest changes |
| `git fetch` | Download objects and refs from remote |
| `git push -u origin <branch>` | Push and set upstream branch |

---

## 🧽 **Undoing Changes**

| Command | Description |
|--------|-------------|
| `git reset <file>` | Unstage a file |
| `git checkout -- <file>` | Discard local changes |
| `git reset --hard HEAD` | Revert to last commit completely |
| `git revert <commit>` | Create a new commit that undoes changes |

---

## 🕵️ **Advanced / Useful**

| Command | Description |
|--------|-------------|
| `git stash` | Temporarily save changes |
| `git stash apply` | Reapply stashed changes |
| `git rebase <branch>` | Reapply commits on top of another base |
| `git cherry-pick <commit>` | Apply a specific commit from another branch |
| `git tag <tagname>` | Tag a specific commit |

---

## 🔧 **Config & Help**

| Command | Description |
|--------|-------------|
| `git config -l` | List all config settings |
| `git help <command>` | Get help for a command |

---

Let me know if you want this as a **PDF**, **markdown file**, or a **visual diagram** version!
