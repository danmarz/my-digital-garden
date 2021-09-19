# Git

1. **How can I use this?**
	-	Almost all operations are local (except *push*, *pull*, *clone*, *fetch*). Unlike [[Subversion]] where any command you execute is against the remote repository.
	-	Uses *checksums* to maintain data integrity and it *is referred to BY that checksum* (this makes it impossible for changes (can't loose information, or get corrupted files) to be put into [[Git]] without it knowing about it)
	-	Nearly all of [[Git]] actions only add data (this means *there are very few actions that are completely undoable* in [[Git]] ie. a *hard-reset*) in other VCSs you can easily loose data that you haven't commited yet. In [[Git]] it's pretty *hard* to loose things if you commit your changes regularly in [[Git]].
	-	[[Git]] file states:
		-	**Commited**: data is safely stored in your [[Git]] database
		-	**Modified**: means that *you have changes that you haven't commited*.
		-	**Staged**: you have a modified file and it's current version is ready to go into your next *commit* snapshot
		## How Git works
		- The `.git` directory is where metadata and object database are stored:
			- This is what is cloned locally
		- The *working tree* (*working directory*) pulled from the Git directory
		- The *staging area* is a file in the Git directory also known as the *index*

		The basic Git workflow:
			- Files are *modified*
			- *Changes are selectively staged* (files that will be part of your next commit)
			- *Commit* the changes that are staged -> stores staged files as permanent snapshots
			
1. **Why must I use this?**
	- Modern [[version control (VCS)]] system
	- Actively maintained open-source project
	- Developed by Linus Torvalds in 2005
	- Uses a *distributed architecture*
		- Every copy that is cloned out is its own individual repository
		- If cloning from a remote repo, and that repo dies, you have *the ability to restore that remote repo from your working copy*
		- You have the ability to *work with multiple remote hosts* (*origin* and *origin2* or *backup* maybe) and you can *push* to remote hosts. (you CAN'T do that with svn or *cvs* where the remote host is *the single source of truth*, therefore if you loose that single source of truth, you loose your data basically)
	- Designed for performance, security and flexibility.
		- Very **performant**: It uses *Object format* for files and in combination with *delta encoding, compression, it focuses on the contents of files, not the filename* -> *Git understands renames, splits and file re-arranges*
		- **Secure** by design: All objects in the Git repository are *secured with a SHA1*
			- This is to *protect your code and your change history from any kind of accidental or malicious changes*, while also ensuring that your *history is fully traceable*.
		- Very **flexible**: *Branching* and *tagging* are considered to be first class citizens (not the case in [[Subversion]]) and *operations that affect your tags and branches are also stored as part of your history*. 
		
3. **When will I use this?**
	-	Git vs Other [[version control (VCS)]]
		-	Different mindset: [[Git]] stores and thinks about information in a very different way:
		-	Most other [[version control (VCS)]] store information as a list of file-based changes:
			-	this is delta-based version control
		- [[Git]] doesn't store data this way:
			- *Git takes snapshots of the current state* (everytime you make a *commit*) -> *a stream of snapshots*

## Git Command Line Basics
- `git fetch`: synchronize local from remote repository
- `git merge`: Bring changes from remote
- `git rebase`: Bring changes from remote
- `git pull`: `git fetch` + `git merge`
- `git pull --rebase`: `git fetch` + `git rebase`
- `git push --tags`: send tags to remote
- `git diff origin/master master`: see changes I'm about to upload with `git push`

### Create an SSH key pair to use with Github:
- `ssh-keygen -t rsa -b 4096 -C "key@GitHub"`
- import `id_rsa.pub` into GitHub: `cat /home/cloud_user/.ssh/id_rsa.pub`

- `git clone git@github.com:d4n-m/learn-git.git` clone own repository
- `git status` -> "nothing to commit", use "git add"
- `touch index.html`
- `git add .` -> dot adds everything or `*.html`: only add html files
- `git status` -> index.html *staged for commit*
- `git rm --cached <file>...` to *UNSTAGE*
- `git commit -am "Add the index file."`: `-a` for *all*, `-m` for commit *message*

## Configure Git user / email 
- `git config --global user.name "dan m."`
- `git config --global user.email "dumy.tm@gmail.com"`

- `git commit --amend --reset-author` -> amend previous commit with the REAL name / email we've configured

## Configure Git with GPG verified keys
- `gpg --import key.pub`
- `gpg --import key.priv`
- `gpg --list-secret-keys`
- `gpg --edit-key 19226A9742E0FE8D trust quit` -> select `5` (ultimate)
- `git config --global user.signingkey 3AA5C34371567BD2`
- `git config --global commit.gpgsign true` to sign commits from *any* repository

## Changing a remote repository (Removing/Re-adding)
To solve moved repository (rename from d4n-m to dan-dm):
- `git remote -v`
- `git remote rm origin`
- `git remote add origin https://github.com/user/repo.git`
- `git push --set-upstream origin master`

## Solve the "Your branch is based on 'origin/master', but the upstream is gone." error
- use "`git branch --unset-upstream`" to fixup
- It seems fairly likely that what you want, though, is to _switch_ from tracking `origin/master`, to tracking something else: probably `origin/source`, but maybe one of the `octopress/` names.
- You can do this with `git branch --set-upstream-to`
	- `git branch --set-upstream-to=origin/source`
- OR, likely because an empty repository was cloned: 
- `git remote add origin https://(address of your repo)` it can be https or ssh
	then ->
- `git push -u origin master` this creates the missing master branch in the remote repo.
- OR, *ALWAYS INITIALIZE A REPO WITH* `README.md`

## Create a new repository from the command line
- `echo "# learning-git" >> README.md`
- `git init`
- `git add .`
- `git commit -m "first commit"`
- `git remote add origin git@github.com:d4n-m/learn-git.git`
- `git push -u origin master`

## Push an existing repository from the command line
- `git remote add origin git@github.com:d4n-m/learn-git.git`: learn-git repository must be previously created on [[GitHub]]
- `git push -u origin master`
	- `-u`, `--set-upstream`
           For every branch that is up to date or successfully pushed, add upstream (tracking) reference, used by argument-less git-pull(1) and other commands.
	- `git push origin master`
           Find a ref that matches master in the source repository (most likely, it would find refs/heads/master), and update the same ref (e.g.  refs/heads/master) in origin repository with it. *If master did not exist remotely, it would be created*.
   - `git push origin HEAD`
           A handy way to *push the current branch to the same name on the remote*.
	- `git push origin HEAD:master`
           Push the current branch to the remote ref matching master in the origin repository. This form is *convenient to push the current branch without thinking about its local name*.
	- `master` is the *current branch we're on* -> `git branch`
	- `origin` is the github account repo used for `fetch` and `push`. -> `git remote -v`
Because [[Git]] is a distributed [[version control (VCS)]], all changes including `git commit` are *local*, we then need `git push` to upload to the repository

## Pull an (updated) repository
- `git pull origin master`

## Setup git commit [[gpg]] sign

1. Copy the Public/Private key pair over or generate one with
	- `gpg --default-new-key-algo rsa4096 --gen-key`
2. Import and *trust* the keys:
	- `gpg --import dan-m.gpg.public`
	- `gpg --import dan-m.gpg.private`
	- `gpg --list-keys`
	- `gpg --edit-key 19226A9742E0FE8D trust quit` -> select `5` (ultimate)
3. Setup [[Git]] to sign commits with [[gpg]]:
	- `git config --global user.signingkey 19226A9742E0FE8D`
	- `git config --global commit.gpgsign true` to sign commits from *any* repository
	- `git config commit.gpgsign true` to sign commits from *this local repository only*
4. Optionally if using another shell, check for and export the environment variable for `gpg`:
	- `GIT_TRACE=1 git commit -m "ok"` -> use [[Gitmoji]] for *commit messages*
	- `export GPG_TTY=$(tty)` -> add to `.zshrc`

## View file differences (like on [[GitHub]])
- `git diff <FILE_NAME>`

## Viewing last commit with git show
- `git show`: `git show` is equivalent to `git show HEAD`, i.e. the latest commit in the current branch

## Amending to the last commit 
- `touch file1`
- `echo 'test' >> file1`
- `git add .`
- `git commit -am "Add file1"`
- `touch file2`
- `echo 'something something' >> file2`
- `git add file2`
- `git log`
- `git commit --amend -m 'Add file1 and file2'` -> a new `-m` *message is required*
- `git log`

## Unstage(untrack, remove) files from commits
If after a `git add .` we have staged all (file1 and file2 in this case) to be commited: 
```shell
k8s-master ➜  git-unstage git:(master) ✗  git status
On branch master

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)

        new file:   file1
        new file:   file2
```
You can unstage a file with the command `git rm`:
- `git rm --cached file2` or to unstage *all*: `git rm --cached -r .`
```shell
k8s-master ➜  git-unstage git:(master) ✗  git status
On branch master

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)

        new file:   file1

Untracked files:
  (use "git add <file>..." to include in what will be committed)

        file2
```

## Reset a repository to the previous commit state

- `git reset --hard`: reset to last commit state. WATCH OUT, *this command reverses changes in files and deletes files* that were added after the last commit!

## Revert a repository (safer than reset!)
- `revert` is a bit more safe than `reset` because it actually keeps the history
- `git revert HEAD` -> reverts the most recent commit

```shell
k8s-master ➜  git-revert git:(master)  git revert HEAD
[master 1059814] Revert "new changes 3"
 1 file changed, 1 deletion(-)
```
- `git revert a9033320da7d780ee38b4e762e4954e48acb16ca` -> reverts to a previous commit. Must resolve conflicts (additions since.. etc) in file(s), then `git add .` and finally to continue `git revert --continue`
```shell
k8s-master ➜  git-revert git:(master)  git revert a9033320da7d780ee38b4e762e4954e48acb16ca
error: could not revert a903332... new changes 1
hint: after resolving the conflicts, mark the corrected paths
hint: with 'git add <paths>' or 'git rm <paths>'
hint: and commit the result with 'git commit'
k8s-master ➜  git-revert git:(master) ✗  vim revert_file 
k8s-master ➜  git-revert git:(master) ✗  git add .
k8s-master ➜  git-revert git:(master) ✗  git revert --continue
[master 7fbde71] Revert "new changes 1"
 1 file changed, 2 deletions(-)
```

## Moving a file in Git

- Using the regular `mv` command will cause [[Git]] to create a new file and delete the file at the old location. Using the `git mv` command, [[Git]] understands the *move*.
- `git mv file new_dir`
- `git commit -am 'Move file'`

- If we do a regular `mv`:
```shell
k8s-master ➜  git-mv git:(master)  mv mv_file new_dir 
k8s-master ➜  git-mv git:(master) ✗  git status
On branch master
Changes not staged for commit:
  (use "git add/rm <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        deleted:    mv_file

Untracked files:
  (use "git add <file>..." to include in what will be committed)

        new_dir/mv_file

no changes added to commit (use "git add" and/or "git commit -a")
```

## Stash (stash away local un-commited changes)
- can restore deleted files locally
- stashing changes allows us to restore deleted files (locally) and postpone commits
- Git has an area called the *stash* where you can *temporarily store a snapshot of your changes without committing them to the repository*. It’s separate from the working directory, the staging area, or the repository.
- `git stash save|push "optional message for yourself"` or in short `git stash`
```shell
k8s-master ➜  git-mv git:(master) ✗  git stash
Saved working directory and index state WIP on master: 47ad75f add mv_file
```
This saves your changes and *reverts the working directory to what it looked like for the latest commit*. Stashed changes are available from any branch in that repository.

Note that changes you want to stash need to be on tracked files.
- `git stash list`: see what is in your stash
- `git stash show NAME-OF-STASH`: see a *summary of the changes* made in the stash
- `git stash apply [STASH-NAME]`: applies the changes and leaves a copy in the stash
```shell
k8s-master ➜  git-mv git:(master)  git stash apply
Removing mv_file
On branch master
Changes not staged for commit:
  (use "git add/rm <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        deleted:    mv_file

no changes added to commit (use "git add" and/or "git commit -a")
k8s-master ➜  git-mv git:(master) ✗  git commit -am 'remove mv_file'
[master 818120b] remove mv_file
 1 file changed, 1 deletion(-)
 delete mode 100644 mv_file
```
- `git stash pop STASH-NAME`: applies the changes and removes files from the stash
- `git stash drop STASH-NAME`: remove stash changes without applying them
- `git stash clear`: clear the entire stash

## Branch & Merge
Sometimes `dev` branch is merged into `master`, other times `feature_branch` is merged into `dev` and *then* merged into `master` as part of a [[CI/CD]] [[Jenkins]] pipeline.

### git branch
There are 2 ways of creating branches:
1. Create a *branch first* with `git branch dev` then `checkout` to change to it--> 
	- now `git branch` command lists `dev` and `* master` as branches
	- **TO SWITCH BRANCHES** you use the `checkout` command:
		- `git checkout dev`
2. *Shortcut* `-b` flag for the `checkout` command creates&automatically switches branches: 
	- `git checkout -b feature_branch`

### git merge
- First change to the branch we want to *merge INTO*
	- `git checkout dev`
	- `git merge feature_branch` will merge `feature_branch` *INTO* `dev`
	- `git push origin dev` -> to update the remote repository on [[GitHub]]
	- Then repeat to merge into `master`:
		- `git checkout master`
		- `git merge dev`
		- `git push origin master`

### delete a branch
- `git branch -d feature_branch`:
	- `-d`: deletes a *fully merged* branch
	- `-D`: deletes a branch *even if not merged*

### rebase
![[08-10 Integrating a feature into master with and without a rebase.svg]]
When you *merge* a branch into another branch, you're continually moving *forward*, *rebase*-ing works a little bit differently, rebasing is *changing the base of your branch from one commit to another*, making it appear as if you created your branch from a different commit

When should I use `git rebase` ? 
- When working **alone** on a project, to not accidentally delete commits
- **If you use pull requests** as part of your code review process, you need to avoid using `git rebase` after creating the pull request. As soon as you make the pull request, other developers will be looking at your commits, which means that it’s a _public_ branch. *Re-writing its history will make it impossible for Git and your teammates to track any follow-up commits added to the feature*. Any changes from other developers need to be incorporated with `git merge` instead of `git rebase`.
	- For this reason, it’s usually a good idea to clean up your code with an interactive rebase _before_ submitting your pull request.
- by performing a *rebase before the merge*, you’re assured that the merge will be fast-forwarded, resulting in a *perfectly linear history*.
- `git checkout dev`
- `git rebase master` -> merges master into dev

## Git submodules

- A *submodule* is a [[Git]] repository inside a [[Git]] repository, *Inception* style WooHoO!
- Some usecases include, let's say you have a *third-party* library that you want to use and it is in a [[Git]] repository. You can add it as a *submodule*, pinning it at a *desired version*, then you can *test/revert before upgrading the library version* as needed.
- `git submodule add https://github.com/d4n-m/learn-git.git` -> initialize a submodule inside your local repository
- `git add .`
- `git commit -am "Added submodule"`
- Git submodules are defined in the `.gitmodules` file:
```shell
k8s-master ➜  git-submodules git:(master)  cat .gitmodules 
[submodule "learn-git"]
        path = learn-git
        url = https://github.com/d4n-m/learn-git.git
```
- To update a submodule (in submodule *dir*):
	- `git fetch origin` -> fetch latest from remote repo
	- `git pull`-> fetch & merge
	- Back in main *dir*:
	- `git commit -am 'Updated Submodule'` -> commit the changes to use updated sm version
- You are free to use `git checkout dev` to switch branches within submodules, but remember *you need to commit each change in the main working dir*.
- To *remove* a submodule:
	- `git rm --cached learn-git`: remove from index
	- `git rm .gitmodules`: remove `.gitmodules` file or *entry* from file
	- `rm -rf learn-git/.git`: remove `.git` directory
	- `git commit -am 'Deleted submodule'`: commit changes
	- `rm -rf learn-git/`: remove un-tracked directory

## Dealing with Conflicts
- Usually Git is pretty smart at resolving conflicts, but sometimes the *changes commited* by two developers at the same time *are so similar, that a merge conflict occurs*. This needs to be addressed *manually* (ie. which version to keep)
- in a conflict situation the `git pull` command will print this error:
```shell
error: Your local changes to the following files would be overwritten by merge:
        test.html
Please commit your changes or stash them before you merge.
```
- you must then commit your file/changes if you don't want to loose them!
	- `git commit -am 'local change to test.html'`
- Next, again the command `git pull` will now glue both changes inside the `file` as `>>>>> HEAD(local change) ===== ORIGIN(remote) <<<<<<`
- you must manually modify/eliminate changes and re-save the file
- finally you re-commit
	- `git commit -am "conflict resolved"`
- and push final changes to the remote repository
	- `git push origin master`

## Git tags

- Tags are like *branches* (points in time), except *they don't change* (fixed points in history)
- Typically created of of *master* (if using the *master* is stable philosophy)
- Typically used for releases, where you can rollback to an older (*working*) release *instead of rolling back the whole master branch*
- `git tag v1.0`: add a new tag
- `git tag`: lists the tags
- `git push origin v1.0`: pushes a tag to remote repository

You can `git checkout` tags, just like branches:
- first do a `git fetch origin` to download the latest files/tags
- `git checkout v1.0`: this will land you in *'detached HEAD' state*. Because a `tag` cannot be changed, any changes you make here must be checked out to *another*, new, non-tag *branch* via `git checkout -b new_branch_name` 

You can delete a tag with the `-d` parameter:
- `git tag -d 1.0`

# .gitignore file universal one-liner
`touch .gitignore && echo "node_modules/" >> .gitignore && git rm -r --cached node_modules ; git status`

# ---

Tags: #on/softwareDevelopment #on/automation #DevOps 
Topics: [[version control (VCS)]] [[Configuration Management]] [[CI/CD]] [[GitHub]]

