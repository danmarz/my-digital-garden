---
Title: Git
---

# GIT

#### See What Branch You're On
- `git status` : see current branch

#### List all Branches
- `git branch` \[ --list ] : show local branches
- `git branch -r` : show remote branches
- `git branch -a` : show all remote + local

#### Create a new branch
- `git checkout -b my-branch-name` : create a NEW local branch

#### Switch to a Branch In Your Local Repo
- `git checkout my-branch-name` : switch to a new local branch

#### Switch to a Branch That Came From a Remote Repo
- \[ git pull ] 
- `git checkout --track origin/my-remote-branch-name` : switch to a branch from a remote repo

#### Push to a Branch
Push to a branch if your local branch does not exist on the remote use either:
- `git push -u origin my-branch-name`
- `git push -u origin HEAD`
If branch already exists on the remote repo:
- `git push`

#### Merge a Branch
- `git status`
- `git checkout` \[ master ] : switch to the branch we want to merge INTO
- `git merge my-branch-name`

### Delete Branches
- `git push origin --delete my-branch-name` : delete remote branch
- `git branch [ -d / -D ] my-branch-name` : delete local route, -d deletes branch if it has been merged, -D option is a shortcut for --delete --force & deletes regardless of merge status.

## [[GitHub workflow]] & working with others
![[GitHub workflow]]

### Removing an .env file from GIT history
1. First, add the file to `.gitignore` file
2. Remove the file from existing git repo `git rm -r --cached .env` 
3. To completely remove a file from Git history:
	- `git filter-branch --index-filter "git rm -rf --cached --ignore-unmatch .env" HEAD`
	- `git push --force`