# git workflows

## Don't include merge commits 

```
git checkout new-feature # Go to the feature branch named "new-feature"
git rebase master
# Now your feature have all the commits from master
git checkout master #Go back to master
git merge --ff-only new-feature
git push
```
The ff-only option appends your commits to master cleanly without a merge commit.

If unfortunately someone pushed more code to the remote master while  you are doing this, your push might fail. You can pull, rebase and push  again like so:

```
git pull --rebase && git push
```

### Script

```bash
#!/usr/bin/env bash
CURRBRANCH=$(git rev-parse --abbrev-ref HEAD)
if [ $CURRBRANCH = "master" ]
then
    echo "Already on master, aborting..."
    exit 1 
fi

echo "Merging the change from $CURRBRANCH to master..."
echo "Rebasing branch $CURRBRANCH to latest master"
git fetch origin master && \
git rebase origin/master && \
echo "Checking out to master and pull" && \
git checkout master && \
git rebase origin/master && \
echo "Merging the change from $CURRBRANCH to master..." && \
git merge --ff-only $CURRBRANCH && \
git log | less 
echo "DONE. You may want to do one last test before pushing"
```



Source: https://shinglyu.com/web/2018/03/25/merge-pull-requests-without-merge-commits.html

## Reset master-next


```
git fetch
git checkout master-next
git reset --hard origin/master
GIT_MERGE_AUTOEDIT=no
export GIT_MERGE_AUTOEDIT=no

git merge feature-branch
```

