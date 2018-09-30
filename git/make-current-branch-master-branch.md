# Make the current branch the master branch

```
git checkout bb-devel
git merge --strategy=ours master    # keep the content of this branch, but record a merge
git checkout master
git merge bb-devel             # fast-forward master up to the merge
```

source: https://stackoverflow.com/questions/2763006/make-the-current-git-branch-a-master-branch

## Allow merge from unrelated histories

"git merge" used to allow merging two branches that have no common base by default, which led to a brand new history of an existing project created and then get pulled by an unsuspecting maintainer, which allowed an unnecessary parallel history merged into the existing project. The command has been taught not to allow this by default, with an escape hatch --allow-unrelated-histories option to be used in a rare event that merges histories of two projects that started their lives independently.

```
git merge --allow-unrelated-histories --strategy=ours master
```