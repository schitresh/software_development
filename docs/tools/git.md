## Basics
```
git status                                  gst
git diff                                    gd
git add                                     ga
git add -all                                gaa
git commit -m <msg>                         gcmsg
git log --graph                             glol
```

## Checkout
```
git checkout                                gco
git checkout -b                             gcb
git checkout master                         gcm
Remove unstaged commits                     gco -- .
Checkout files from another branch          gco <branch> -- <files>
```

## Push & Pull
```
git push origin <branch>                    ggp
git pull origin <branch>                    ggl
git pull --rebase origin <branch>           ggu
git fetch                                   gf
git merge origin/master                     gmom
```

## Stash
```
git stash list                              gstl
git stash push                              gsta
git stash push -m 'message'                 gsta -m 'message'
git stash --untracked                       gstu
git stash --all                             gsta
git stash pop                               gstp
git stash apply                             gstaa
git stash apply stash@{n}                   gstaa stash@{n}
git stash drop                              gstd
git stash drop stash@{n}                    gstd stash@{n}
git stash clear                             gstc
git stash show --text                       gsts
git stash show --text -p stash@{1}          gsts stash@{n}
```

## Reset & Revert
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

## Skip
```
git update-index --assume-unchanged <files>
git update-index --no-assume-unchanged <files>
git update-index --skip-worktree <files>
git update-index --no-skip-worktree <files>
```

## Patch
```sh
git diff HEAD > example.patch
git reset --hard
patch -p1 < example.patch
```
