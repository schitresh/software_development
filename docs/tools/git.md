# Git

### Basics

```
git status
git diff
git add
git add -all
git commit -m <msg>
git log --graph
```

### Checkout

```
git checkout
git checkout -b
git checkout master
git checkout -- .                           Remove unstaged commits
git checkout <branch> -- <files>            Checkout files from another branch
```

### Push & Pull

```
git push origin <branch>
git pull origin <branch>
git pull --rebase origin <branch>
git fetch
git merge origin/master
```

### Stash

```
git stash list
git stash push
git stash push -m 'message'
git stash --untracked
git stash --all
git stash pop
git stash apply
git stash apply stash@{n}
git stash drop
git stash drop stash@{n}
git stash clear
git stash show --text
git stash show --text -p stash@{1}
```

### Reset & Revert

```
git reset --hard
git reset --hard hash
git reset --hard ~HEAD
git reset --soft HEAD~1
git restore .
git restore -S .
git clean -df
git revert hash
git rebase -i HEAD~n
git commit --amend -m 'message'
git remote add <origin>
```

### Worktree

```
git update-index --skip-worktree <files>
git update-index --no-skip-worktree <files>

git update-index --assume-unchanged <files>
git update-index --no-assume-unchanged <files>
```

### Patch

```sh
git diff HEAD > example.patch
git reset --hard
patch -p1 < example.patch
```
