
# Common Git Branching Strategies

## 1. Mainline (Trunk-based Development)
- **Description**: All developers work on a single `main` (or `master`) branch with small, frequent commits.
- **Pros**:
  - Simple workflow.
  - Encourages continuous integration.
- **Cons**:
  - Requires discipline to avoid breaking the build.

## 2. Feature Branching
- **Description**: Developers create separate branches for each feature or bug fix, merging them into `main` when complete.
- **Branch Examples**: `feature/add-login`, `bugfix/fix-crash`.
- **Pros**:
  - Isolates features for parallel development.
  - Reduces conflicts.
- **Cons**:
  - Can lead to complex integration during merges.

## 3. Gitflow Workflow
- **Description**: A structured model with dedicated branches for development stages.
- **Branches**:
  - `main`: Production-ready code.
  - `develop`: Integration branch for features.
  - `feature/*`: Feature-specific branches.
  - `release/*`: Pre-release preparation branches.
  - `hotfix/*`: Urgent production fixes.
- **Pros**:
  - Clear process for releases and bug fixes.
  - Supports complex projects.
- **Cons**:
  - Complex to manage.
  - Overkill for small teams or projects.

## 4. GitHub Flow
- **Description**: A simplified workflow where feature branches are created from `main`, reviewed via pull requests, and merged back.
- **Branches**:
  - `main`: Production branch.
  - Feature branches: Short-lived branches for features or fixes.
- **Pros**:
  - Simple and fast.
  - Ideal for continuous integration/delivery.
- **Cons**:
  - Limited structure for large or complex release cycles.

## 5. Forking Workflow
- **Description**: Common in open-source projects, developers fork the repository, work on feature branches, and submit pull requests to the original repository.
- **Pros**:
  - Ideal for open-source collaboration.
  - Strict control over the main repository.
- **Cons**:
  - Requires more coordination.
  - Less practical for small or internal teams.

## 6. Release Branching
- **Description**: A branch is created from `main` (or `develop`) for release preparation, allowing only bug fixes and final tweaks.
- **Branches**: `release/*`, `main`, `develop`.
- **Pros**:
  - Focuses on stable release preparation.
- **Cons**:
  - Can result in longer release cycles.

## 7. Environment Branching
- **Description**: Separate branches are maintained for different environments (e.g., development, staging, production).
- **Branches**: `development`, `stage`, `production`.
- **Pros**:
  - Simplifies environment-specific code management.
- **Cons**:
  - Risk of environment divergence if not properly synchronized.


# Git Workflow Tutorial

## 1. Setting Up Git

Configure your Git environment and SSH keys for secure repository access.

### Configure User Information

Set your name and email for commit authorship.

```bash
git config --global user.name "Ruslan Konovalov"
git config --global user.email "konovalov.ruslan@gmail.com"
```

### Generate and Add SSH Key

Create an SSH key for secure communication with remote repositories.

```bash
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
eval `ssh-agent -s`
ssh-add ~/.ssh/id_rsa
```

### View Configuration

Check your Git configuration settings.

```bash
git config --list
```

## 2. Managing Branches

Create, switch, and delete branches to organize your development workflow.

### Create a New Branch

Create and switch to a new branch.

```bash
git checkout -b new_branch_name
```

### Switch Branches

Move to an existing branch.

```bash
git checkout branch_name
```

### Delete Local Branch

Remove a local branch (e.g., `dev`).

```bash
git branch -d dev
```

### Delete Remote Branch

Remove a branch from the remote repository.

```bash
git push origin --delete branch_name
```

### Sync Remote Branch Locally

Fetch a remote branch and create a local copy.

```bash
git branch -f remote_branch_name origin/remote_branch_name
git checkout remote_branch_name
```

### Delete All Local Branches Except `main`

Remove all local branches except `main`.

```bash
git branch | grep -v "main" | xargs git branch -D
```

## 3. Working with Commits

Manage commits to track changes and maintain a clean history.

### Soft Reset

Move HEAD to a previous state while keeping changes staged.

```bash
git reset --soft HEAD~20
git add .
git commit -m "Squashed commit message"
```

### Hard Reset

Discard changes and reset to a specific commit.

```bash
git reset --hard 149c58d44c6b4c6551f0ca08b24d46230a4a550f
git push origin develop --force
```

### Squash Commits

Combine multiple commits into one using a soft reset.

```bash
git reset --soft HEAD~20
git add .
git commit -m "Combined commit message"
```

### Cherry-Pick a Commit

Apply a specific commit to the current branch.

```bash
git cherry-pick <hash-id>
```

### View Commit History

Display a concise commit log for a branch.

```bash
git log branch_name --oneline
```

### Show Current Commit Details

View details of the latest commit.

```bash
git show
```

## 4. Managing Changes

Handle modified, staged, and untracked files.

### Check for Conflicts

Identify potential merge conflicts.

```bash
git diff --check
```

### Remove Untracked Files

Remove a file from Git tracking without deleting it locally.

```bash
git rm --cached file
```

### Remove Untracked Folder

Stop tracking a folder and commit the change.

```bash
git rm -rf --cached folder_name
git commit -m 'Remove ignored directory "folder_name"'
git push origin main
```

### List Tracked Files

View all files tracked in a branch (e.g., `stage`).

```bash
git ls-tree -r stage --name-only
```

### Stash Changes

Temporarily save uncommitted changes and clean the working directory.

```bash
git stash save --keep-index
```

### Apply Stashed Changes

Restore stashed changes.

```bash
git stash pop
```

### Discard Stashed Changes

Remove the latest stash.

```bash
git stash drop
```

## 5. Merging and Rebasing

Combine changes from different branches or rewrite commit history.

### Merge a Branch

Merge a branch (e.g., `hotfix`) into `main`.

```bash
git checkout main
git merge hotfix
git push origin main
```

### Merge with Conflict Resolution

Use a specific strategy to resolve conflicts (e.g., prioritize `theirs`).

```bash
git merge --strategy-option theirs
```

### Rebase Instead of Merge

Fetch updates and rebase your branch for a cleaner history.

```bash
git fetch
git rebase origin/branch_name
```

### Resolve Overwritten Local Changes

Handle cases where local changes conflict with a pull.

- **Option 1**: Commit changes, then pull and merge.
    
    ```bash
    git add .
    git commit -m "Local changes"
    git pull
    ```
    
- **Option 2**: Stash changes, pull, then apply stash.
    
    ```bash
    git stash
    git pull
    git stash pop
    ```
    
- **Option 3**: Work in a feature branch, then merge.
    
    ```bash
    git checkout -b feature_branch
    git add .
    git commit -m "Feature changes"
    git checkout main
    git pull
    git checkout feature_branch
    git merge main
    ```
    

## 6. Managing Remote Repositories

Interact with remote repositories for collaboration.

### Force Push

Overwrite the remote branch with local changes (use with caution).

```bash
git push -f origin main
```

### Remove Remote Repository

Disconnect a remote repository.

```bash
git remote rm REPO_NAME
```

### Overwrite Local Files with Remote

Reset local files to match the remote branch.

```bash
git fetch --all
git reset --hard origin/main
```

### Download a Specific Folder

Use `svn` to download a single folder from a Git repository.

```bash
svn checkout https://github.com/Yorko/mlcourse_open/trunk/jupyter_notebooks/topic05_bagging_rf
```

## 7. Resetting Repository History

Clear commit history while keeping the current code state.

### Create a New History

Start a fresh history with the current files.

```bash
git checkout --orphan latest_branch
git add -A
git commit -am "Initial commit"
git branch -D main
git branch -m main
git push -f origin main
```

### Create an Empty Branch

Start a new branch with no history.

```bash
git checkout --orphan V2.2
git add --all
git commit -a -m "Initial commit"
```

## 8. Using Git Hooks

Automate tasks with Git hooks (e.g., `pre-push`).

### Set Up a Pre-Push Hook

Configure a custom hook to run before pushing.

```bash
chmod +x ~/.githooks/pre-push
git config --global core.hooksPath $PWD/.githooks/
```
[https://gist.github.com/ColCh/9d48693276aac50cac37a9fce23f9bda](https://gist.github.com/ColCh/9d48693276aac50cac37a9fce23f9bda)

[https://medium.com/backticks-tildes/how-to-automate-your-git-workflow-with-git-hooks-c905296c49bc](https://medium.com/backticks-tildes/how-to-automate-your-git-workflow-with-git-hooks-c905296c49bc)
## 9. Working with Tags

Use tags to mark specific release versions. Tags allow you to identify specific release versions of your code. You can think of a tag as a branch that doesn't change.
Once it is created, it loses the ability to change the history of commits.
tags are just the repository frozen in time

### Understanding Tags

- Tags are snapshots of the repository at a specific point in time.
- They do not change once created, unlike branches.
- Useful for marking release versions (e.g., `v1.0.0`).

## 10. Submodules vs. Subtrees

Understand the difference between submodules and subtrees.

- **Submodule**: A link to another repository, tracked separately.
- **Subtree**: A copy of another repository’s content, integrated into the main repository.

## 11. Development Workflow Example

Organize branches for large projects using a hierarchical structure.

### Branching Strategy

- **Stage**: Integration branch for testing.
- **Epic Branch**: Groups related features.
- **Feature Branch**: Specific feature development.
- **Workflow**:
    - Merge feature branches into the epic branch without squashing.
    - Merge the epic branch into the stage branch with squashing for a clean history.

## 12. Additional Tools and Resources

Enhance your Git workflow with these tools and references.

- **Sourcetree**: A GUI for Git management ([https://www.sourcetreeapp.com](https://www.sourcetreeapp.com/)).
- **Git Flow Cheatsheet**: Guide for Gitflow workflow ([https://danielkummer.github.io/git-flow-cheatsheet/index.ru_RU.html](https://danielkummer.github.io/git-flow-cheatsheet/index.ru_RU.html)).
- **Git Book**: Comprehensive Git documentation ([https://git-scm.com/book/ru/v2](https://git-scm.com/book/ru/v2)).
- **Rebase Tutorial**: Learn rebasing ([https://habr.com/en/post/161009/](https://habr.com/en/post/161009/)).

## Notes

- Always use `git fetch` and `git status` before merging or rebasing to understand the repository state.
- Avoid force pushes (`git push -f`) in shared repositories to prevent overwriting others’ work.
- Use feature branches for isolated development to minimize conflicts in the main branch.
- Regularly back up your repository before performing destructive operations like hard resets or history rewrites.