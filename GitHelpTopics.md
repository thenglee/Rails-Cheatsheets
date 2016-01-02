## Git Help Topics

#### 1. Change remote origin from HTTPS to SSH
```
git remote set-url origin git@github.com:<username>/<project-name>.git
git remote -v  # To view remote URL
```

#### 2. Undo a local commit and put changes made in the last commit back into staging area
```
git reset --soft HEAD^
```

#### 3. Undo a commit after `git push`, remove all changes made in the last commit
```
git reset --hard HEAD~1
git push -f <remote> <branch>
```

#### 4. Revert a commit: Adds another commit that undo the commit
```
git revert <6 digit commit hash>
git push # To push to remote after revert
```
