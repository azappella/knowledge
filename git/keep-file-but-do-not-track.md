# Keep file in a Git repo, but don't track changes

```
git update-index --skip-worktree FILE_NAME
```

```
git update-index --no-skip-worktree FILE_NAME
```

```
git update-index --assume-unchanged FILE_NAME
```

```
git update-index --no-assume-unchanged FILE_NAME
```

source: https://stackoverflow.com/questions/13630849/git-difference-between-assume-unchanged-and-skip-worktree