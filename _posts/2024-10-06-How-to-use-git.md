---
title: "How to use git"
categories:
  - Git
tags:
  - Git
  - Docker
  - Oracle19c
  - Database
---

## How to Use Git

### Git Reference
- [Git - Official Documentation](https://git-scm.com/docs)

### What is a Git Repository?
- A Git repository is a storage location for your project's files and version history.

### Git GUI
- [GitKraken Legendary Git Tools | GitKraken](https://www.gitkraken.com/)

## Basic Git Commands

### Check Git Version
```bash
git -v
```

### Check the Status of the Git Repository
```bash
git status
```

### Initialize a New Repository
```bash
git init
```

### Add Files to Staging Area (Working Directory → Staging Area)
```bash
git add file1 file2
```

### Add All Changes to Staging Area
```bash
git add .
```

### Commit Changes (Staging Area → Repository)
```bash
git commit -m "Commit message"
```
- If you don't use the `-m` option, Git will open a text editor (like Vim).
- You can change the default text editor by following this link: [Git Setup and Config](https://git-scm.com/book/en/v2/Appendix-C:-Git-Commands-Setup-and-Config).

## Viewing Git Log and Commit Information

### View Commit Logs
```bash
git log
```
- To see the log in a single line per commit, use `--oneline` or `--pretty=oneline --abbrev-commit`.
- [Git - git-log Documentation](https://git-scm.com/docs/git-log)

### Amend the Most Recent Commit
```bash
git add <file>
git commit --amend
```

### Exclude Files and Folders (gitignore)
```bash
touch .gitignore
```
- You can exclude specific files and folders by listing them in the `.gitignore` file.
- Examples: API keys, credentials, system files, secret API keys
  - `foldername/`: Exclude a specific folder
  - `*.log`: Exclude all `.log` files
- [gitignore.io](http://gitignore.io) is a helpful resource for generating `.gitignore` files.

## Best Practices for Git Commits

### Atomic Commits
- Each commit should represent one unit of work or one feature.

### Writing Commit Messages
- Commit messages should be written in the present tense. The first line should summarize the commit.

## Branch Management

### Create and Switch to a Branch
```bash
git branch <branch-name>
git switch <branch-name>
```
- Use the `-c` option to create and switch to a branch at the same time.

### Rename a Branch
```bash
git branch -m <new-branch-name>
```

### Delete a Branch
```bash
git branch -d <branch-name>
```
- You can only delete branches that have been merged.
- Use the `-D` option to force-delete a branch.

### Merge Branches
```bash
git merge <branch-name>
```
- Fast-forward merges are possible when there are no changes on the master branch.
- If conflicts occur, Git will show the conflicting files, allowing you to resolve the conflicts before committing.

## Comparing Changes with Git

### Compare Changes with the Last Commit
```bash
git diff
```

### Compare Staged Changes
```bash
git diff --staged
```

### Compare Specific Files
```bash
git diff HEAD <filename>
```

### Compare Between Branches
```bash
git diff master..<branch-name>
```

### Compare Between Commits
```bash
git diff <commit1>..<commit2>
```

## Stashing Changes with Git

### Stash Changes
```bash
git stash
```
- Temporarily save changes when switching branches.

### Apply and Delete the Latest Stash
```bash
git stash pop
```
- This applies the most recent stash and removes it from the stash stack.

### Apply Stash Without Deleting
```bash
git stash apply
```
- This applies the stash but keeps it in the stash stack.

## Reverting and Resetting Commits

### Revert a Commit
```bash
git revert <commit-hash>
```
- Reverts a previous commit by creating a new commit that undoes the changes. This is useful when working in a team.

### Reset to a Specific Commit
```bash
git reset <commit-hash>
```
- Resets the repository to the state of a specific commit.
- Use the `--hard` option to discard all changes as well.

## Managing Remote Repositories

### Add a Remote Repository
```bash
git remote add <name> <url>
```

### View Remote Repositories
```bash
git remote -v
```

### Push Changes to a Remote Repository
```bash
git push origin master
```

### Push a Local Branch to a Remote Branch
```bash
git push -u origin <branch-name>
```

## Rebasing with Git

### Perform a Rebase
```bash
git rebase <branch>
```
- Rebases the current branch onto another branch, cleaning up the commit history.

### Interactive Rebase
```bash
git rebase -i HEAD~4
```
- Opens an interactive rebase editor to clean up multiple commits.


## Git Reflog

### View Reflog (Reference Log)
```bash
git reflog
```
- Shows a log of all references that have been updated in your repository (including those not visible in git log).
View Reflog for a Specific Branch

```bash
git reflog show HEAD
git reflog show <branch-name>
```
### Reset to a Specific Point in Reflog
```bash
git reset --hard HEAD@{index}
```
- Resets your repository to a specific point in the reference log, discarding changes.

### Compare Differences Using Reflog

```bash
git diff HEAD@{0} HEAD@{2}
```
- Compares changes between two points in the reflog.

## Managing Git Tags

### Create and List Tags
```bash
git tag <tag-name>
```
- Lists all tags: `git tag -l`

### Delete a Tag
```bash
git tag -d <tag-name>
```

### Push Tags to Remote
```bash
git push --tags
```

### Checkout by Tag
```bash
git checkout <tag>
```

## Git Aliases

### Set Up a Git Alias
```bash
git config --global alias.<shortname> '<git-command>'
```
- Example: `git config --global alias.a status` allows you to use `git a` to execute `git status`.
