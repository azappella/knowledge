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


## Alternatively

At a previous job, everyone had their own local settings branch on which we committed our individual settings. This branch was based off master; any topic branches were branched off of settings. When topics were ready for integration, we rebased them onto master. This seems to be what @koljaTM is suggesting.

```
A---B---C  master
         \
          F  settings
           \
            D  topic

rebase --onto master settings topic

A---B---C  master
        |\
        | F  settings
         \
          D  topic
```
When new changes hit master, we would rebase settings of master, then rebase any topics we were working on off of settings.

Admittedly, this isn't a one-step "leave this floating forever" solution, but it is clean and simple enough.

source: https://stackoverflow.com/questions/13276909/how-to-do-a-local-only-commit-in-git