# Git Cheat Sheet

Quick reference for everyday Git. See [29-husky.md](../docs/29-husky.md).

## Setup & basics

```bash
git init
git clone <url>
git status
git add <file>        # or git add .
git commit -m "message"
git log --oneline --graph --decorate
```

## Branching

```bash
git branch                    # list
git switch -c feature/x       # create + switch (modern)
git switch main               # switch
git branch -d feature/x       # delete merged
git branch -D feature/x       # force delete
```

## Syncing

```bash
git fetch
git pull                      # fetch + merge
git pull --rebase             # fetch + rebase
git push
git push -u origin feature/x  # set upstream
```

## Merging & rebasing

```bash
git merge feature/x
git rebase main
git rebase --continue / --abort
git cherry-pick <hash>
```

## Undoing

```bash
git restore <file>            # discard working changes
git restore --staged <file>   # unstage
git reset --soft HEAD~1       # undo commit, keep changes staged
git reset --hard HEAD~1       # undo commit, discard changes (destructive)
git revert <hash>             # safe undo via new commit
```

## Stash

```bash
git stash
git stash pop
git stash list
```

## Inspecting

```bash
git diff                      # unstaged
git diff --staged             # staged
git show <hash>
git blame <file>
```

## Conventional commits

```
feat: add login form
fix: correct token refresh loop
docs: update RTK Query chapter
refactor: extract useActiveUsers hook
test: add error-state test
chore: bump dependencies
```
