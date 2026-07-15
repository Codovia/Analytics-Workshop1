# Basic Git Commands

## Create a GitHub Account

Visit [https://github.com/](https://github.com/) and sign up for an account.

## Verify Git Installation

Run the following command to verify if Git works on your computer:

```bash
git --version
```

## Configure Git

After installing Git, you must enter your credentials to identify yourself as the author of your work. The username and email address should match the ones you use in GitHub.

Add your username:
```bash
git config --global user.name "your_username"
```

Add your email address:
```bash
git config --global user.email "your_email_address@example.com"
```

To check the configuration, run:
```bash
git config --global --list
```

## Add SSH Keys to Your GitHub Account

SSH keys allow you to interact with GitHub without entering your password every time.

1. Install xclip (if not already installed):
   ```bash
   sudo apt install xclip
   ```

2. Generate SSH key:
   ```bash
   ssh-keygen -t ed25519 -C "your_email@example.com"
   ```

3. Copy SSH key:
   ```bash
   xclip -sel clip < ~/.ssh/id_ed25519.pub
   ```

4. Add the SSH key to GitHub:
   - Visit: https://github.com/settings/ssh/new
   - Paste the key and save

## Fork a Repository

Forking refers to making a copy of a project you want to contribute to.

Follow these steps to fork a project:

1. Go to the project URL using this link: https://github.com/UniCourt/Analytics-Workshop1
2. Click on the **Fork** button in the project page
3. Select your namespace to fork the project into

## Clone a Repository

Cloning a repository means the files from the remote repository are downloaded to your computer, and a connection is created.

This connection requires credentials. There are two ways to add credentials: SSH and HTTPS. We recommend the SSH method.

### Clone with SSH

1. Go to your project's landing page and click the green **Code** button
2. Copy the SSH URL
3. Open a terminal and go to the directory where you want to clone the files
4. Run this command:

   ```bash
   git clone git@github.com:<your-username>/Analytics-Workshop1.git
   ```

   Example:
   ```bash
   git clone git@github.com:johndoe/Analytics-Workshop1.git
   ```

## View Your Remote Repositories

To view the remote repositories that you have added:

```bash
git remote -v
```

## Configure Remote Repositories

We need to configure our local system to the remote repositories in Git. Generally, two remote repositories are maintained: **origin** and **upstream**.

- **origin** - Your forked repository (where you push and pull from)
- **upstream** - The main repository you forked (where you pull updates from)

Run the following command to add the upstream repository:

```bash
git remote add upstream git@github.com:UniCourt/Analytics-Workshop1.git
```

To verify the remotes:
```bash
git remote -v
```

Expected output:
```
origin    git@github.com:your-username/Analytics-Workshop1.git (fetch)
origin    git@github.com:your-username/Analytics-Workshop1.git (push)
upstream  git@github.com:UniCourt/Analytics-Workshop1.git (fetch)
upstream  git@github.com:UniCourt/Analytics-Workshop1.git (push)
```

## Create a Branch

Branches allow you to develop features, fix bugs, or safely experiment with new ideas in a contained area of your repository. You always create a branch from an existing branch.

To create a new branch called `exercise`:
```bash
git checkout -b exercise
```

To switch to an existing branch:
```bash
git checkout <branch_name>
```

To see what branch you are currently on:
```bash
git branch
```

Make sure you are on the exercise branch:
```bash
git checkout exercise
```

## Download the Latest Changes

To get an up-to-date copy of the project, we use the `pull` command. This gets all the changes made to the repository since the last clone or pull.

To get all the latest updates from upstream:
```bash
git pull upstream main
```

Or from origin (your fork):
```bash
git pull origin main
```

## Create a New File

Create a new file `index.txt` in your local system.

Add the following line to your `index.txt` file and save:
```
Welcome to Git workshop!
```

## View the Changed Files

This command displays the state of the working directory and the staging area.

To check all the files you have changed:
```bash
git status
```

## View Differences

See the differences between your local unstaged changes and the latest version:

```bash
git diff
```

## Stage the Local Changes

We use `git add` to add all/required files that have changed to the staging area.

To stage a specific file:
```bash
git add <file_name>
```

To add your `index.txt` file:
```bash
git add index.txt
```

To stage all files in the current directory and subdirectories:
```bash
git add .
```

## Commit the Staged Files

The `git commit` command creates a snapshot of all the staged changes in the project history.

To commit all the changes:
```bash
git commit -m "message"
```

The message should describe the intention of your commit.

Example:
```bash
git commit -m "Add new file with welcome message"
```

## Send Changes to Remote Repository

The `push` command sends all committed changes to the remote repository.

To push your changes:
```bash
git push origin <branch_name>
```

To push your exercise branch:
```bash
git push origin exercise
```

You can now go to GitHub and see the updated code in your browser.

## Create a Pull Request

A Pull Request (PR) is the process of merging your changed version of code into the original version. A Pull Request allows you to visualize the differences between the original code and your proposed code changes.

Steps to create a Pull Request:

1. When you push changes to the remote repository, Git may prompt you with a link to create a Pull Request. You can copy-paste the link in your browser or create a new PR from the project page.

2. Select the:
   - **base branch** - The branch you want to merge into (usually `main`)
   - **compare branch** - Your branch with changes (e.g., `exercise`)

3. Add a proper title and description, then submit the Pull Request.

## Common Git Workflow

Here's the complete workflow for contributing to a project:

```bash
# 1. Fork the repository (on GitHub)

# 2. Clone your fork
git clone git@github.com:your-username/project.git
cd project

# 3. Add upstream remote
git remote add upstream git@github.com:original-owner/project.git

# 4. Create a new branch
git checkout -b feature-branch

# 5. Make changes and commit
git add .
git commit -m "Describe your changes"

# 6. Push to your fork
git push origin feature-branch

# 7. Create Pull Request (on GitHub)

# 8. After merge, update your local main branch
git checkout main
git pull upstream main

# 9. Delete the feature branch
git branch -d feature-branch
git push origin --delete feature-branch
```

## Useful Git Commands Reference

| Command | Description |
|---------|-------------|
| `git init` | Initialize a new Git repository |
| `git clone <url>` | Clone a repository |
| `git status` | Show working tree status |
| `git add <file>` | Stage file for commit |
| `git add .` | Stage all changes |
| `git commit -m "msg"` | Commit staged changes |
| `git push origin <branch>` | Push to remote |
| `git pull origin <branch>` | Pull from remote |
| `git branch` | List branches |
| `git branch <name>` | Create new branch |
| `git checkout <branch>` | Switch branch |
| `git checkout -b <name>` | Create and switch branch |
| `git merge <branch>` | Merge branch into current |
| `git remote -v` | Show remotes |
| `git log` | Show commit history |
| `git diff` | Show unstaged changes |
| `git reset <file>` | Unstage file |
| `git stash` | Stash changes temporarily |
| `git stash pop` | Apply stashed changes |
