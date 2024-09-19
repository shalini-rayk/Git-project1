# Git and GitHub

>**As this is my first code on GitHub, I have included all the commands and research gone into creating this simple project, starting from creating a GitHub account to setting up SSH keys, managing remote repositories, and synchronizing changes. This documentation covers everything I’ve learned to ensure a smooth workflow and understanding of Git and GitHub.Continue reading, and I hope you enjoy it!**

## Version Control System

A Version Control System (VCS) helps developers track changes to code and collaborate with others. It saves every version of your project.

1. Centralised version control system  
2. Distributed version control system

### CVCS  
One central server; can't work offline; if the server fails, work stops.

### DVCS  
Every developer has a full copy; can work offline; server failures don’t affect progress.

## Git  
Git is a distributed version control system (VCS) that allows us to track changes in code and manage versions locally on our computer. It helps developers work on the same codebase without conflicts, enabling easy branching, merging, and version tracking.

Branching means to diverge from the main line of development and work on our own changes in isolation without affecting the main codebase.

## GitHub (or GitLab or GitBucket)  
GitHub is a cloud-based platform that hosts Git repositories and adds collaboration features like issue tracking, pull requests, code review, and more. It allows teams to collaborate online and share repositories.

A pull request (PR) is a feature on GitHub that allows developers to request a review of the changes made on a branch before merging it into another branch, typically the main branch.

### Fork  
A fork in GitHub is essentially a personal copy of someone else’s repository. When you fork a repository, you create a duplicate of that project under your own GitHub account.

## Git vs GitHub

- Git is a tool, while GitHub is a service.
- You can use Git without GitHub, but GitHub requires Git (or other Git services) for version control.
- Git is installed locally, while GitHub is a web-based service.

## Git Lifecycle Commands

### Git adding credentials (Configuration)  
Add credentials using the following commands:
```
git config --global user.name "your user name"
git config --global user.email "your email"

```
To check your user name and email, use the following command:
```
git config --list
```
We will get the following:

- credential.helper=osxkeychain  
- user.name=”your user name”  
- user.email=”your email id”

### Initialization (Git init)  
After running `git init` in a directory, the directory becomes a “git repository” with which we can perform all the operations like tracking changes, commits, and managing versions.

To check if a directory is a git repository, we can use the following command:
``` 
ls -al
```
There will be a file `.git` folder in it.

### Git Add  
Stages changes (new files, modifications, deletions) for the next commit. You can stage individual files or all changes at once.
```
git add <file>     # Stage a specific file
git add .          # Stage all changes in the directory
```
### Git Status  
Shows the current status of your working directory and staging area. It lists staged, unstaged, and untracked files.
```
git status <file>
```
### Committing (git commit)  
Records the staged changes in the repository. Each commit has a unique identifier and includes a commit message describing the changes.
```
 git commit <file> -m "Commit message"
```
### Viewing History (Git diff, Git log)  

#### Git Log  
Shows the commit history for the current branch. It displays commit hashes, authors, dates, and messages.

```
git log <file>
git log --oneline <file>   # Shows the logs in one line
git log --graph <file>     # Shows the logs in graph mode
```

> **Note:** While using git log commands, if we are not able to see all logs, we can press the space bar until it shows "end," and we can press `q` after that to quit.

#### Git Diff  
Displays changes between commits, working directory, and staging area.

```
git diff <file> # Changes not yet staged
git diff --staged <file> # Changes staged for the next commit
```

### Undoing Changes (reset --soft, --mixed, --hard, restore, revert)  

| Command                | Purpose                                                     | Effect on History                  | Effect on Staging Area             | Effect on Working Directory        | Example Scenario                                                                 |
|------------------------|-------------------------------------------------------------|------------------------------------|-----------------------------------|------------------------------------|----------------------------------------------------------------------------------|
| `git restore <file>`    | Discards unstaged changes in a file                         | No change                         | No change                         | Discards uncommitted changes in a file | Undo uncommitted changes in the working directory                                   |
| `git restore --staged <file>` | Unstages changes for a specific file                  | No change                         | Unstaged                          | Keeps changes in the working directory | Unstage changes but keep them in the working directory                               |
| `git reset --soft HEAD~1` | Removes commit but keeps changes staged                  | Moves back 1 commit               | Staged                            | No change (changes remain in the working directory) | Undo last commit but keep changes staged                                             |
| `git reset --mixed HEAD~1` | Removes commit and unstages changes                     | Moves back 1 commit               | Unstaged                          | No change (changes remain in the working directory) | Undo last commit and unstage changes                                                  |
| `git reset --hard HEAD~1` | Removes commit and discards all changes                  | Moves back 1 commit               | Discarded                         | Discards all changes in the working directory | Completely remove last commit and all changes, permanently lost                        |
| `git revert <commit_hash>` | Reverts a specific commit and creates a new commit      | Adds a new revert commit          | No change                         | No change (keeps working directory intact) | Undo a specific commit but keep it in the history                                    |
| `git restore --source <commit_hash> <file>` | Reverts file to a specific commit   | No change                         | No change                         | Reverts file content to match the specific commit | Revert a file to a version from a specific commit                                    |
| `git reset --hard`      | Discards all changes in the working directory               | No change                         | No change                         | Resets working directory to the last commit | Discard all uncommitted changes and reset the working directory to the last commit |

> **Note:** Be careful with the Git restore options as they will affect the entire directory, not just a single file.

## Branching and Merging

### Branching  
Branching is the process of creating a new line of development from an existing branch. The following are the different branch types:

| Branch Type          | Purpose                                       | Created From          | Merged Into            | Typical Naming           | Use Case                                          |
|----------------------|-----------------------------------------------|-----------------------|------------------------|--------------------------|--------------------------------------------------|
| **Main Branch (main)**| Holds the stable, production-ready code       | Initial branch or develop | N/A                    | `main`, `master`          | Production branch containing final code          |
| **Feature Branch**    | Develop new features or enhancements          | `develop` or `main`    | `develop` or `main`     | `feature/<feature-name>`  | Used for developing new features or functionalities |
| **Bugfix Branch**     | Fixes specific bugs or issues                 | `develop` or `main`    | `develop` or `main`     | `bugfix/<issue-id>`       | Used for resolving specific bugs or issues       |
| **Release Branch**    | Prepares code for release                     | `develop`              | `main`, `develop`       | `release/<version-number>` | Final testing, bug fixes, and release preparations |
| **Hotfix Branch**     | Fixes critical issues in production           | `main`                 | `main`, `develop`       | `hotfix/<issue-id>`       | Immediate fixes for critical bugs in production code |
| **Experimental Branch** | For experimenting with new ideas            | `develop`, `main`, any | Sometimes not merged    | `experiment/<idea-name>`  | Used for trying out new approaches, refactoring, or libraries |
### Key commands in branching

```
git checkout -b "branch-name" # Creates new branch
git push -u origin "branch-name" # Push branch from local to remote
git checkout "branch-name" # Switches from one branch to another
git branch # Lists branches
git branch -r # Lists branches in remote repository
git branch -d "branch-name" # Deletes branch
```
### Merging

| Feature               | Purpose                                                     | Command                        | History                        | Use Case                        |
|-----------------------|-------------------------------------------------------------|---------------------------------|---------------------------------|---------------------------------|
| **Cherry-pick**        | Apply specific commits to another branch                    | `git cherry-pick <commit_hash>` | Non-linear, adds specific commits | Selectively apply changes      |
| **Merge**             | Combine branches into one                                   | `git merge <branch_name>`       | Non-linear, creates a merge commit | Integrate entire branches      |
| **Rebase**            | Reapply commits on top of another branch                    | `git rebase <branch_name>`      | Linear, rewrites history         | Cleanly integrate changes      |

Merge vs Rebase: Merging integrates two branches into one, preserving the branch history and creating a merge commit, while rebasing re-applies commits from one branch onto another, resulting in a linear history without merge commits.

## Remote Repository Management and Collaboration

### 1. SSH Keys and Establishing Connections  
Git can use SSH keys to authenticate your local repository with a remote service (like GitHub) without needing to enter your username and password every time you push or pull.
### Generating SSH keys:
To set up a secure connection between your local machine and GitHub, you'll need to generate an SSH key.

# SSH Key Generation and Git Operations

## Generating SSH Keys

`ssh-keygen -t rsa`:

- **ssh-keygen**: This command generates a new SSH key pair (private and public keys). It will store the key on your local machine for secure authentication.
- **-t rsa**: Specifies the type of key to generate. RSA (Rivest–Shamir–Adleman) is a widely used encryption algorithm.

You’ll be prompted to enter a location to save the key. Press Enter to accept the default location (`~/.ssh/id_rsa`). You'll also be asked if you want to create a passphrase for added security. This is optional but recommended.

### Adding the SSH Key to Your GitHub Account

1. Run `cat ~/.ssh/id_rsa.pub` to copy the public key from your local machine.
2. Copy the output (the public key).
3. Go to GitHub > Settings > SSH and GPG keys > New SSH key.
4. Paste the public key into the GitHub form and save it. GitHub will now recognize the machine with this key as authorized to access your repositories.

## Managing Remote Repositories

### Adding a Remote Repository

`git remote add <remote_name> <url>`:

- Adds a remote repository to your local Git configuration.
- `<remote_name>` is typically set as `origin` by convention.
- `<url>` is the URL of the remote repository (e.g., a GitHub repo URL).

**Example use**: Adding a remote to a local repository after creating it.

### Viewing Remote Repositories

`git remote -v`:

- Displays a list of remote repositories and their URLs. The output will be similar to:
1. `origin https://github.com/username/repo.git (fetch) `
2. `origin https://github.com/username/repo.git (push)`

### Cloning a Remote Repository

`git clone <url>`:

- Downloads a remote repository to your local machine and initializes it as a local Git repository.

**Example use**: Cloning an existing remote repository to start working on it locally.

### Git Clone vs. Git Remote Add

- **git remote add**: Adds references to remote repositories.
- **git clone**: Creates a local copy of an entire remote repository.

### Git Clone vs. Git Fork

- **git clone**: Used for creating a local copy of any existing remote repository, whether it's your own or someone else’s.
- **git fork**: Creates a personal copy of someone else’s repository for making changes independently, usually for contributing back to the original repository.

**Note**: You can clone a forked repository to your local machine using `git clone`.

## Synchronizing with Remote Repositories

### Pushing Changes

`git push <remote_name> <branch_name>`:

- Pushes your local commits from the specified branch to the remote repository.
- If remote and local branches are correctly set up, you can use a simple `git push`.

**Example**: `git push origin main` (branch name).

### Pulling Changes

`git pull`:

- Fetches the latest changes from the remote repository and merges them into your local repository.

**Example**: `git pull origin main` (branch name).

Thank you for reading! 😊

